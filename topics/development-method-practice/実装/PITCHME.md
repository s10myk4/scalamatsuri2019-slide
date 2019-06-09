@snap[north-west text-gray span-100]
@size[1.5em](Implementation)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-focus5.png)
@snapend

Note:
参考程度に、実装例を紹介します

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
@size[1.3em](Avoid Fat Controller)
@snapend

図: Fat Controllerのサンプル

Note:
TODO

コントローラーの責務は、入力を受け付けて、要求を実現するためのユースケースを呼び出し結果を返すが責務です
コントローラーにドメインロジックなどが漏れると、テスタビリティが低下し改修コストが高くなりますよね
ユースケースの概念を用いることで、コントローラからロジックを排除します

---
@snap[north-west text-gray span-100]
@size[1.3em](Love Scala)
@snapend
TODO

- High expression
- Highly abstract programming
- Functional programming
- Referential transparency
- etc

