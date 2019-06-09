### Detailed Design

---
@snap[north-west text-gray span-100]
@size[1.5em](Detailed Design)
@snapend

![development-flow](assets/img/developmemt-flow.png)

---
@snap[north-west text-gray span-100]
@size[1.5em](Detailed Design)
@snapend

* 概念モデルが要求を満たしているかの解釈を一致させる
    * ドメインモデルの属性とそのふるまいの定義
    * ユースケースの要件

@snap[south-west template-note text-gray]
@snapend

Note:

詳細設計では前フェーズまでの分析をもとに、概念モデルについてレビューをします。

ここでのレビューとは、実装者同士が概念モデルに対して同じ解釈ができるかの確認をするということです。

いきなり作り始める前にこのフェーズを経ることで、解釈の不一致を防ぐことができます。

---
@snap[north-west text-gray span-100]
@size[1.5em](Outputs)
@snapend

- ドメインモデル
- ユースケースの定義
- テストケース

@snap[south-west template-note text-gray]
@snapend

Note:
ここで期待する成果物は、概念モデルを以下のソースコードで表現するところまで行うこととします。

- ドメインモデル
- ユースケースの定義
- テストケース

---
@snap[north-west text-gray span-100]
@size[1.5em](Detailed Design)
@snapend

#### 要求と分析のフェーズでの成果物を元に概念モデルをコードに表現する

@snap[south-west template-note text-gray]
Express conceptual models in code based on deliverables in requirements and analysis phase.
@snapend

Note:
それでは前フェーズの成果物を元に、概念モデルをコードで表現してみましょう。

---
@snap[north-west text-gray span-100]
@size[1.5em](Define Warrior Level)
@snapend

```scala
import cats.data.ValidatedNel
import cats.syntax.validated._

sealed abstract case class WarriorLevel(value: Int)

object WarriorLevel {

  def of(value: Int): ValidatedNel[WarriorLevelError, WarriorLevel] = ???

  final case class WarriorLevelError(value: Int) extends WarriorError {
    val cause = s"$value is a invalid warrior level"
  }
  
}
```

---
@snap[north-west text-gray span-100]
@size[1.5em](Define Warrior Name)
@snapend

```scala
import cats.data.ValidatedNel
import cats.syntax.validated._

sealed abstract case class WarriorName(value: String)

object WarriorName {

  def of(value: String): ValidatedNel[WarriorError, WarriorName] = ???

  final case class WarriorNameLengthError(length: Int) extends WarriorError {
    val cause = s"$length is a invalid warrior name size"
  }

}
```

---
@snap[north-west text-gray span-100]
@size[1.5em](Define Weapon)
@snapend

```scala
import enumeratum._

sealed trait Weapon extends EnumEntry {
  val name: String
  val offensivePower: Int
  val attribute: Attribute
  val levelConditionOfEquipment: Int
}
```

---
@snap[north-west text-gray span-100]
@size[1.5em](Define Weapon)
@snapend

```scala
object Weapon extends Enum[Weapon] {

  val values = findValues

  case object GoldSword extends Weapon {
    val name: String = "gold sword"
    val offensivePower: Int = 44
    val attribute: Attribute = LightAttribute
    val levelConditionOfEquipment: Int = 30
  }

  case object BlackSword extends Weapon {
    val name: String = "black sword"
    val offensivePower: Int = 50
    val attribute: Attribute = DarkAttribute
    val levelConditionOfEquipment: Int = 40
  }
}
```

---
@snap[north-west text-gray span-100]
@size[1.5em](Define Attribute)
@snapend

```scala
import enumeratum._

sealed trait Attribute extends EnumEntry

object Attribute extends Enum[Attribute] {

  case object LightAttribute extends Attribute
  case object DarkAttribute extends Attribute
  case object WaterAttribute extends Attribute
  case object FireAttribute extends Attribute
  case object NormalAttribute extends Attribute

  val values = findValues
}
```

---
@snap[north-west text-gray span-100]
@size[1.5em](Define Warrior)
@snapend

```scala
trait WarriorError extends DomainError

final case class WarriorId(value: Long)

sealed abstract case class Warrior(
    id: WarriorId,
    name: WarriorName,
    attribute: Attribute,
    weapon: Option[Weapon],
    level: WarriorLevel
) {
  def equip(weapon: Weapon): ValidatedNel[WarriorError, Warrior] = ???
}

object Warrior {
  object DifferentAttributeError extends WarriorError

  object NotOverLevelError extends WarriorError
}
```
---
@snap[north-west text-gray span-100]
@size[1.5em](Define Warrior Repository)
@snapend

```scala
trait WarriorRepository[F[_]] {
  def update(warrior: Warrior): F[Unit]
  def resolveBy(id: WarriorId): F[Option[Warrior]]
}
```

---
@snap[north-west text-gray span-100]
@size[1.5em](Define Use Case Result)
@snapend

```scala
sealed trait UseCaseResult

object NormalCase extends UseCaseResult

trait AbnormalCase extends UseCaseResult {
  val cause: String
}

object NotConsideredDomainError extends AbnormalCase {
  val cause = "This domain error is not considered in this UseCase"
}
```
Note:
実装に依存しない固有のユースケースの実行結果を表す型を定義します

---
@snap[north-west text-gray span-100]
@size[1.5em](Define Use Case)
@snapend

```scala
final class EquipWeaponToWarrior[F[_]] {
  def exec(warrior: Warrior, newWeapon: Weapon): F[UseCaseResult] = ???
}
```

---
@snap[north-west text-gray span-100]
@size[1.5em](Define Abnormal Case)
@snapend

```scala
object EquipWeaponToWarrior {

  final case class EquipWeaponToWarriorInput(
      weapon: Weapon
  )

  object DifferentAttributeAndNotOverLevel extends AbnormalCase {
    val cause: String = s"${DifferentAttribute.cause} 且つ ${NotOverLevel.cause}"
  }

  object DifferentAttribute extends AbnormalCase {
    val cause: String = "戦士と武器の属性が異なるため装備できません"
  }

  object NotOverLevel extends AbnormalCase {
    val cause: String = "戦士が武器のレベル条件を満たしていないので装備できません"
  }

}
```

---
@snap[north-west text-gray span-100]
@size[1.5em](Test Cases)
@snapend

``` scala
behavior of "戦士に武器を装備できる"

it should "正常系" in {}

it should "異常系: 戦士のレベルが選択した武器のレベル条件を満たしていない場合" in {}

it should "異常系: 戦士の属性と選択した武器の属性が異なる場合" in {}

it should
  """
  異常系: 戦士のレベルが選択した武器のレベル条件を満たしていない、かつ、
               戦士の属性と選択した武器の属性が異なる場合
  """ in {}
```

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](Detailed Design)
@snapend

#### レビューコスト
- MRで要件漏れや誤解について指摘される |
- 分析手法を用いることで属人的な設計を排除する |


@snap[south-west template-note text-gray]
Issue: Review Cost
@snapend

Note:

