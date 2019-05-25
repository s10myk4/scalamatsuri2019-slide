### 実装

Note:
ここでは、主に設計フェーズによって具体化した仕様やモデルを使って実装をしていきます

---
### Implement Domain

```scala
import cats.data.ValidatedNel
import cats.syntax.contravariantSemigroupal._
import cats.syntax.validated._
import com.s10myk4.domain.model.Attribute
import com.s10myk4.domain.model.weapon.Weapon

sealed abstract case class Warrior(
    id: WarriorId,
    name: WarriorName,
    attribute: Attribute,
    weapon: Option[Weapon],
    level: WarriorLevel
) extends {

  import Warrior._

  def equip(weapon: Weapon): ValidatedNel[WarriorError, Warrior] = {
    (isNotSameAttribute(weapon), isNotOverLevel(weapon))
      .mapN((_, _) => new Warrior(id, name, attribute, Some(weapon), level) {})
  }

  private def isNotSameAttribute(weapon: Weapon): ValidatedNel[WarriorError, Weapon] =
    if (attribute == weapon.attribute) weapon.validNel else DifferentAttributeError.invalidNel

  private def isNotOverLevel(weapon: Weapon): ValidatedNel[WarriorError, Weapon] =
    if (level.value >= weapon.levelConditionOfEquipment) weapon.validNel else NotOverLevelError.invalidNel

}
```
---
### Implement Domain UnitTest


---
### Implement UseCase
```scala
package object cont {
  type UseCaseCont[F[_], A] = ContT[F, UseCaseResult, A]
}
```
```scala
final class EquipWeaponToWarrior[F[_]: Monad](
    repository: WarriorRepository[F]
) {

  import EquipWeaponToWarrior._

  def exec(warrior: Warrior, newWeapon: Weapon): UseCaseCont[F, UseCaseResult] =
    UseCaseCont { f =>
      warrior.equip(newWeapon) match {
        case Valid(w)     => repository.update(w).flatMap(_ => f(NormalCase))
        case Invalid(err) => Monad[F].point(err)
      }
    }
}
```
---
### Implement UseCase UnitTest

```scala
class EquipWeaponToWarriorSpec
  extends FlatSpec with DiagrammedAssertions with MockitoSugar {

  behavior of "戦士に武器を装備できる"

  it should "正常系" in {
    val warrior = (for {
      name  <- WarriorName.of("せんしくん").toOption
      level <- WarriorLevel.of(40).toOption
    } yield {
      Warrior.createWithoutWeapon(WarriorId(1L), name, LightAttribute, level)
    }).get

    assert(useCase.exec(warrior, GoldSword).run(Applicative[Id].pure) === NormalCase)
  }

  it should "異常系: 戦士のレベルが選択した武器のレベル条件を満たしていない場合" in {
    val warrior = (for {
      name  <- WarriorName.of("せんしくん").toOption
      level <- WarriorLevel.of(29).toOption
    } yield {
      Warrior.createWithoutWeapon(WarriorId(1L), name, LightAttribute, level)
    }).get

    assert(
      useCase.exec(warrior, GoldSword).run(Applicative[Id].pure) === NotOverLevel
    )
  }

  it should "異常系: 戦士の属性と選択した武器の属性が異なる場合" in {
    val warrior = (for {
      name  <- WarriorName.of("せんしくん").toOption
      level <- WarriorLevel.of(40).toOption
    } yield {
      Warrior.createWithoutWeapon(WarriorId(1L), name, DarkAttribute, level)
    }).get

    assert(
      useCase.exec(warrior, GoldSword).run(Applicative[Id].pure) === DifferentAttribute
    )
  }

  it should "異常系: 戦士のレベルが選択した武器のレベル条件を満たしていない、かつ、戦士の属性と選択した武器の属性が異なる場合" in {
    val warrior = (for {
      name  <- WarriorName.of("せんしくん").toOption
      level <- WarriorLevel.of(29).toOption
    } yield {
      Warrior.createWithoutWeapon(WarriorId(1L), name, DarkAttribute, level)
    }).get

    assert(
      useCase.exec(warrior, GoldSword).run(Applicative[Id].pure) ===
        DifferentAttributeAndNotOverLevel
    )
  }

  private val repository: WarriorRepository[Id] = mock[WarriorRepository[Id]]
  when(repository.update(any[Warrior])).thenReturn(())

  private val useCase = new EquipWeaponToWarrior[Id](repository)

}
```

---
### Implement Controller

```scala
final class WarriorController(
    cc: ControllerComponents,
    findWarrior: FindWarrior[IO],
    warriorEquippedNewWeapon: EquipWeaponToWarrior[IO],
    presenter: Presenter[UseCaseResult, Result],
    val ec: ExecutionContext
) extends AbstractController(cc)
    with FormHelper with ActionSupport {

  def equipWeapon(id: Long): EssentialAction = Action.async { implicit r =>
    val composedCont: ContT[IO, UseCaseResult, UseCaseResult] = for {
      form <- EquipWeaponForm.apply.bindCont[IO]
      (warriorId, weapon) = (WarriorId(id), form.weapon)
      warrior <- findWarrior.exec(warriorId)
      result <- warriorEquippedNewWeapon.exec(warrior, weapon)
    } yield result

    composedCont
      .run(Applicative[IO].pure)
      .map(presenter.present)
      .toFuture
  }
  
}
```

---
### Implement Controller

```scala
private[http] trait FormHelper {

  implicit class FormSyntax[A](form: Form[A]) {
    def bindCont[F[_]: Applicative](implicit req: Request[AnyContent]): UseCaseCont[F, A] =
      ContT(
        f =>
          form.bindFromRequest.fold[F[UseCaseResult]](
            error => Applicative[F].pure(InvalidInputParameters(errors = convertFormErrorsToMap(error.errors))),
            a => f(a)
          )
      )
  }

  private def convertFormErrorsToMap(errors: Seq[FormError]): Map[String, String] = {
    errors.map(e => e.key -> e.message).toMap
  }
}
```

```scala
private[http] trait ActionSupport {

  def ec: ExecutionContext

  implicit class IOSyntax[A](io: IO[A]) {
    def toFuture: Future[A] = Future(io.unsafeRunSync())(ec)
  }

}
```

---
### 問題の本質はドメインに、ソフトウェア要件の詳細はユースケースに
- ユースケースという構造が持つ意味
    - 機能を実現するための知識を隠蔽する
    - 機能要求(正常、異常)を満たすことを検証
    
--- 
### 分析した概念をそのままソフトウェアに表現する
- ビジネスや要件の変更に対して、強い設計
- ソフトウェア上のオブジェクトは開発者が創造したものではない

- 要件の変化に対する実装に変更箇所は局所化される

---
### アーキテクチャとは？

Note:
詳細設計によって具体化した仕様を実装してみたいところですが、
土台となるアーキテクチャについて触れる必要があるでしょう

先進的なアーキテクチャは一見多くのことを解決してくれるように感じますが、本当にそうなのでしょうか？
アーキテクチャが解決する問題の本質について少し考えてみたいと思います

あなたはアーキテクチャに対してどのようなことを期待していますか？

---
### アーキテクチャの役割
https://speakerdeck.com/jooohn/introduction-to-clean-architecture?slide=30

---
### 重要な観点

重要な点は、クリーンアーキテクチャを使うためにユースケースを表現するのではなく、
分析したユースケースの知識をソフトウェアに落とし込むために、クリーンアーキテクチャを利用する点です

---
### 単方向な依存関係によりプラガブルな設計

Note:
依存性の逆転を利用して、固有の実装への依存をプラガブルバーツとしてに外部に置く

---
### 実装レビュー

要件定義のフェーズごとにレビューをしているので、実装後のレビューでの観点は、実装的な観点に閉じている
- 要件やモデリングに対する指摘が発生することを極めて抑えられる

---
