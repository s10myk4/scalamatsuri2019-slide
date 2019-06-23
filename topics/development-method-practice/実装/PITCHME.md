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
sealed abstract case class Warrior(...) {

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
final class EquipWeaponToWarrior[F[_]: Monad](
  repository: WarriorRepository[F]
) {

  def exec(warrior: Warrior, newWeapon: Weapon):
      ContT[F, UseCaseResult, UseCaseResult] =
    
    UseCaseCont { f =>
      warrior.equip(newWeapon) match {
        case Valid(w)     => repository.update(w).flatMap(_ => f(NormalCase))
        case Invalid(err) => Monad[F].point(err.toUseCaseResult)
      }
    }
}
```

---
@snap[north-west text-gray span-100]
@size[1.5em](Why use Continuation Monad?)
@snapend

- 後続の処理を関数として取り、自分の処理結果によって後続の処理を継続して行うかをハンドリングする事ができる
TODO
継続とEitherではどのような点が違う？

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](Implement Controller)
@snapend

```scala
final class WarriorController(...) extends ... {

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

  assert(useCase.exec(warrior, GoldSword).run(Applicative[Id].pure) === 
    NotOverLevel)
}
```

---
@snap[north-west text-gray span-100]
@size[1.5em](Implement Use Case Test)
@snapend

### Abnormal Case
```scala
it should "異常系: 戦士の属性と選択した武器の属性が異なる場合" in {
  ...
  assert(useCase.exec(warrior, GoldSword).run(Applicative[Id].pure) === 
    DifferentAttribute)
}

it should """異常系: 戦士のレベルが選択した武器のレベル条件を満たしていない、かつ、
             戦士の属性と選択した武器の属性が異なる場合""" in {
  ...
  assert(useCase.exec(warrior, GoldSword).run(Applicative[Id].pure) ===
    DifferentAttributeAndNotOverLevel)
}
```


