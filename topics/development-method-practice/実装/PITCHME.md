## Implementation 

Note:
ここでは、主に設計フェーズによって明らかになった要件や概念を使って実装をしていきます

---
@snap[north-west text-gray span-100]
@size[1.5em](Implement Domain)
@snapend

```scala
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
    if (attribute == weapon.attribute) weapon.validNel 
    else DifferentAttributeError.invalidNel

  private def isNotOverLevel(weapon: Weapon): ValidatedNel[WarriorError, Weapon] =
    if (level.value >= weapon.levelConditionOfEquipment) weapon.validNel 
    else NotOverLevelError.invalidNel

}
```
---
@snap[north-west text-gray span-100]
@size[1.5em](Implement Use Case)
@snapend

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
@snap[north-west text-gray span-100]
@size[1.5em](Implement Controller)
@snapend

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
@snap[north-west text-gray span-100]
@size[1.5em](Implement Controller)
@snapend

```scala
private[http] trait FormHelper {

  implicit class FormSyntax[A](form: Form[A]) {
    def bindCont[F[_]: Applicative](implicit req: Request[AnyContent]): UseCaseCont[F, A] =
      ContT(
        f =>
          form.bindFromRequest.fold[F[UseCaseResult]](
            error => Applicative[F].pure(
              InvalidInputParameters(errors = convertFormErrorsToMap(error.errors))),
            a => f(a)
          )
      )
  }

  private def convertFormErrorsToMap(errors: Seq[FormError]): Map[String, String] = 
    errors.map(e => e.key -> e.message).toMap
    
}
```
---
@snap[north-west text-gray span-100]
@size[1.5em](Implement Controller)
@snapend

```scala
private[http] trait ActionSupport {

  def ec: ExecutionContext

  implicit class IOSyntax[A](io: IO[A]) {
    def toFuture: Future[A] = Future(io.unsafeRunSync())(ec)
  }

}
```

---
@snap[north-west text-gray span-100]
@size[1.5em](Implement Domain Test)
@snapend


---
@snap[north-west text-gray span-100]
@size[1.5em](Implement Use Case Test)
@snapend

### Normal Case
```scala
it should "正常系" in {
  val warrior = (for {
    name  <- WarriorName.of("せんしくん").toOption
    level <- WarriorLevel.of(40).toOption
  } yield {
    Warrior.createWithoutWeapon(WarriorId(1L), name, LightAttribute, level)
  }).get

  assert(useCase.exec(warrior, GoldSword).run(Applicative[Id].pure) === NormalCase)
}
```

---
@snap[north-west text-gray span-100]
@size[1.5em](Implement Use Case Test)
@snapend

### Abnormal Case
```scala
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
```

---
@snap[north-west text-gray span-100]
@size[1.3em](What Do Domain & Use Case Express?)
@snapend


@snap[west]
#### 問題の本質はドメインに、ソフトウェア要件の詳細はユースケースに

@ul(false)
- ドメインロジック 
  - ソフトウェアの要件に限らない業務の知識

- ユースケース
  - ソフトウェアの要件を実現するための知識
@ulend
@snapend

@snap[south-west template-note text-gray]
Business knowledge in the domain.  
Software requirements in use cases.
@snapend
   
Note:
TODO 仮書き
ドメインロジックとユースケースというのはどのような違いがあるのでしょうか？
ということについて少し整理しておく必要がありそうです

これまで話にでた正常系、異常系のようなソフトウェアの要件を実現するための知識は
そのユースケースの中に閉じ込めることで、凝集度の高いコンポーネントを実現することができます

現金で支払いをしたら差額を返却するように
ソフトウェアの要件に限らず、その業務の特性はドメインロジックとすることで
業務を遂行する存在に限らず、ドメインの一貫性を保つことができますね

では今回の例とした`ユーザーは、戦士に武器を装備することができる`でより具体的に考えると
戦士に武器を装備するための制約をドメインロジックとして持ち、
ユースケースでは戦士に武器を装備した結果どのようにハンドリングすることで要件を満たすことができるのかということを考えてきました

---
@snap[north-west text-gray span-100]
@size[1.3em](What Do Domain & Use Case Express?)
@snapend

#### 信用できないドキュメント

@snap[south-west template-note text-gray]
Issue: Untrusted documents
@snapend

Note:
Issuesのところで話した、ドキュメントの課題の話に戻りますが、

要件変更や改修のたびに頑張って、ユースケース記述のドキュメントを更新してたがやはり大変だった
コードは常に実際の要件に追従する


--- 
@snap[north-west text-gray span-100]
@size[1.5em](Object Extracted by Analysis Express in Software)
@snapend

- ビジネスルールや要件の変更に対して柔軟な設計 |
    - 改修による変更対応箇所を局所化
- ユースケースが要件の変更を追従する |
- ソフトウェア上のオブジェクトは開発者が創造したものではない |
    - ユビキタス言語

Note:
要求の分析によって抽出された概念をそのままソフトウェアの実装に表現することで、どのような利点があるのでしょうか？
あらためて整理したいと思います

前のスライドで話したように
業務ルールの変化によるソフトウェアの変更箇所は対象のドメインモデルとユースケースに閉じ、
要件の変更に対するソフトウェアの変更箇所は対象のユースケースに閉じることで、改修による変更対応箇所を局所化されることで
ビジネスルールや要件の変更に対して柔軟な設計を実現できます

要件の変更に対しても、ユースケースの知識として表現されているのでトラックすることを実現します
前段でとりあげた信用できないドキュメントに惑わされることもなくなるかなと思います

今回の例である、戦士や武器という概念は開発者が勝手に創造したものではないですよね？
要求や要件定義のプロセスの中で抽出した概念です

開発者とビジネス関係者との対話において概念に対して同じ認識ができることによって、
コミュニケーションを活性化し要求者の求めていることをより効率的に理解する事ができるようになります
それをコード上に表現することで、実装での表現の乖離を防ぎ概念的な理解を促します
つまりそれが、ユビキタス言語なんですね
ソフトウェア上にオブジェクトとして定義されるものは開発者が創造したものではないのです

---
@snap[north-west text-gray span-100]
@size[1.5em](Implementation Review)
@snapend

- MRで要件やモデリングに対する指摘が発生することを極力抑えられる

Note:
要件定義、設計におけるフェーズの成果物を段階的にレビューをしているので、
実装後のMRのレビューは実装の観点に閉じています

---
@snap[north-west text-gray span-100]
@size[1.5em](Thinking About Architecture)
@snapend

Note: 
TODO 画像？

概念的な設計における意味合いについておさらいしてきましたが、
実装の側面においてはアーキテクチャについても少し考える必要があるかもしれません

みなさんはアーキテクチャに対してどのようなことを期待していますか？

---
@snap[north-west text-gray span-100]
@size[1.5em](Purpose of the Architecture?)
@snapend

- 求められるシステムを構築・保守するために必要な人材を最小限に抑えることである |

@snap[south-east template-note]
@box[text-white rounded bg-orange box-padding text-05](Source: [Clean Architecture 達人に学ぶソフトウェアの構造と設計](https://www.kadokawa.co.jp/product/301806000678/))
@snapend

Note:
先進的なアーキテクチャは一見多くのことを解決してくれるように感じますが、本当にそうなのでしょうか？

---
@snap[north-west text-gray span-100]
@size[1.5em](Important Point of Architecture)
@snapend

- ドメインとユースケースのレイヤを用いることで業務知識と要件を実現するための知識をカプセル化
- 単方向な依存関係によりプラガブルな設計
    - Interface Adapter
    - DIP

Note:
依存性の逆転を利用して、固有の実装への依存をプラガブルバーツとしてに外部に置く

