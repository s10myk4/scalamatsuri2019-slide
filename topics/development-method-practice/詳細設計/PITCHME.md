### 詳細設計

---

![development-flow](assets/img/developmemt-flow.png)

---

@snap[north-west text-gray span-100]
@size[1.5em](詳細設計)
@snapend

#### ここでの問題領域

* 概念モデルが要求を満たしているかのレビュー
    * ドメインモデルの属性とそのふるまいの定義
    * ユースケースの要件
* いきなりすべてを実装しない

@snap[south-west template-note text-gray]
@snapend

Note:

詳細設計では前フェーズまでの分析をもとに、概念モデルについてレビューをします。いきなり作り始めるのではなく、要求が網羅されているかを確認できるようにします。

---

@snap[north-west text-gray span-100]
@size[1.5em](詳細設計)
@snapend

#### 期待する成果物

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
@size[1.5em](詳細設計)
@snapend

#### 要求と分析のフェーズでの成果物を元に概念モデルをコードに表現する

@snap[south-west template-note text-gray]
@snapend

Note:

それでは前フェーズの成果物を元に、概念モデルをコードで表現してみましょう。

---

@snap[north-west text-gray span-100]
@size[1.5em](詳細設計: ドメインモデル)
@snapend

#### 抽出したドメインモデル

図を記載

Note:
要求と分析フェーズで具体化したドメインモデルを実際にコードに落としてみます

@snap[south-west template-note text-gray]
@snapend

---
### Define WarriorLevel

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
### Define WarriorName

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
### Define Weapon

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
### Define Weapon

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

### Define Attribute

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
### Define Warrior

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
  final case class DifferentAttributeError(warriorAttr: Attribute, weapon: Weapon) extends WarriorError

  final case class NotOverLevelError(warriorLevel: WarriorLevel, weapon: Weapon) extends WarriorError
}
```
---
### Define Warrior Repository

```scala
trait WarriorRepository[F[_]] {
  def update(warrior: Warrior): F[Unit]
  def resolveBy(id: WarriorId): F[Option[Warrior]]
}
```
---

@snap[north-west text-gray span-100]
@size[1.5em](詳細設計: ユースケース)
@snapend

#### 抽出したユースケース記述(正常系、異常系)

図を記載

Note:
要求と分析フェーズで具体化したユースケースを実際にコードに落としてみます

@snap[south-west template-note text-gray]
@snapend

---
### Define UseCaseResult
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

### Define UseCase

```scala
final class EquipWeaponToWarrior[F[_]] {
  def exec(warrior: Warrior, newWeapon: Weapon): F[UseCaseResult] = ???
}
```
---
### Define AbnormalCase

```scala
object EquipWeaponToWarrior {

  final case class EquipWeaponToWarriorInput(
    weapon: Weapon
  )

  final case class DifferentAttribute(err: DifferentAttributeError) 
    extends AbnormalCase {
    
    val cause: String = s"Weapon attribute:${err.weapon.attribute.entryName}
      is different warrior attribute:${err.warriorAttr.entryName}"
  }

  final case class NotOverLevel(err: NotOverLevelError) 
    extends AbnormalCase {
    val cause: String = s"Warrior level:${err.warriorLevel.value}
      is not over weapon level:${err.weapon.levelConditionOfEquipment}"
  }
  
  final case class DifferentAttributeAndNotOverLevel(
    err1: DifferentAttributeError,
    err2: NotOverLevelError
  ) extends AbnormalCase {
        
    val cause: String = 
      s"${DifferentAttribute(err1).cause} and ${NotOverLevel(err2).cause}"
  }
}
```
---

@snap[north-west text-gray span-100]
@size[1.5em](詳細設計: テストケース)
@snapend

#### ユースケース記述を元にテストケースを書き起こす
#### 正常系 + 異常系の数だけケースある

図を記載

Note:

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](詳細設計: テストケース)
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

@snap[south-west template-note text-gray]
@snapend

Note:

---

### 課題の話

---

@snap[north-west text-gray span-100]
@size[1.5em](詳細設計: ユースケース)
@snapend

#### レビューコストの話
- MRで要件漏れや誤解について指摘される
- 分析手法を用いることで属人的な設計を排除する

Note:

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](詳細設計: ユースケース)
@snapend

#### 信用できないドキュメントの話

Note:
Issuesのところで話した、ドキュメントの課題の話に戻りますが、


@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](詳細設計: ユースケース)
@snapend

#### コードのユースケースの定義をドキュメントとして活用する

Note:


@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](詳細設計: ユースケース)
@snapend

#### 実体験

- 要件変更や改修のたびに頑張って、ユースケース記述のドキュメントを更新してたがやはり大変だった
- 図(キャプチャ)入れる

- コードは常に実際の仕様と同期する

@snap[south-west template-note text-gray]
@snapend
