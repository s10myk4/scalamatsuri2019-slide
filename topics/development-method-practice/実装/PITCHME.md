@snap[north-west text-gray span-100]
@size[1.5em](Implementation)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-focus5.png)
@snapend

Note:

参考程度に、実装例を紹介します。

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

Note:

戦士のふるまいとして、武器を装備するメソッドを追加しました。装備させる武器の属性やレベルの制約をz表現しています。

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
    
    ContT { f =>
      warrior.equip(newWeapon) match {
        case Valid(w)     =>
          repository.update(w).flatMap(_ => f(NormalCase))
        case Invalid(err) =>
          Monad[F].point(err.toUseCaseResult)
      }
    }
}
```

Note:

ここでは継続モナドを利用してユースケースを表現しています。

---
@snap[north-west text-gray span-100]
@size[1.5em](Why use Continuation Monad?)
@snapend

``` scala
case class Cont[R, A](run: (A => R) => R) { ... }
```

- 後続の処理を関数として取り、自分の処理結果によって後続の処理を継続して行うかをハンドリングする事ができる
    - 正常/異常系の網羅されたコンポーネントの実現
    - ユースケース利用側で合成しやすい
    - 構造がシンプルで異なる粒度を表現できる

@snap[south-east template-note]
@box[text-white rounded bg-orange box-padding text-05](Ref: [継続モナドを使ってwebアプリケーションのユースケース(ICONIX)を表現/実装する](http://labs.septeni.co.jp/entry/2018/03/18/135106))
@snapend

Note:

ユースケースをなぜ継続モナドを利用して表現したかですが、いろいろと利点があります。

まず、継続モナド自体の特徴は `後続の処理を関数として取り、自分の処理結果によって後続の処理を継続して行うかをハンドリングする事ができる` という点にあります。

例えば先に見たとおり、1 つのユースケースの結果は正常系と異常系にわかれます。自身の処理によって継続するかどうかをコントロールできる仕組みはユースケースの表現にうってつけです。

過去に弊社ブログでも記事にしているので詳細についてご興味のある方はご覧ください。

---
@snap[north-west text-gray span-100]
@size[1.5em](Implement Controller)
@snapend

```scala
final class WarriorController(...) extends ... {

  def equipWeapon(id: Long): EssentialAction = Action.async { implicit r =>
    // 継続モナドを合成
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

Note:

ではユースケースを利用するコントローラの実装について見てみましょう。

ここでは今まで取り上げてきた 戦士に武器を装備させる ユースケース(warriorEquippedNewWeapon)の他に、
play のフォーム処理と任意の戦士を取り出すユースケース(findWarrior) をそれぞれ合成しています。 
合成のしやすさも継続モナド利用のうれしいところの 1 つですね。

---
@snap[north-west text-gray span-100]
@size[1.3em](Avoid Fat Controller)
@snapend

@snap[west snap-20]
<img src="assets/img/fat-controller.png" width="50%"/>
@snapend

@snap[midpoint snap-80]
#### コントローラーの責務

@ul[](false)
- 入力の受け付け
- ユースケース実行
- 結果を返す 
@ulend
@snapend

Note:

コントローラーの責務は、入力を受け付けて、要求を実現するためのユースケースを呼び出し結果を返すことです。

先のフェーズでもユースケースとドメインモデルの凝集度についてお話しましたが、コントローラーにロジックが漏れると、テスタビリティの低下や改修コストが高くなってしまいます。

ユースケースの概念を用いることで、コントローラからロジックを排除します。

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

  assert(
    useCase.exec(warrior, GoldSword).run(Applicative[Id].pure)
      === NormalCase
  )
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

  assert(useCase.exec(warrior, GoldSword).run(Applicative[Id].pure)
    === NotOverLevel)
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
  assert(useCase.exec(warrior, GoldSword).run(Applicative[Id].pure)
    === DifferentAttribute)
}

it should """異常系: 戦士のレベルが選択した武器のレベル条件を満たしていない、かつ、
             戦士の属性と選択した武器の属性が異なる場合""" in {
  ...
  assert(useCase.exec(warrior, GoldSword).run(Applicative[Id].pure)
    === DifferentAttributeAndNotOverLevel)
}
```


