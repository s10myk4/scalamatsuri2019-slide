### 詳細設計

---

![development-flow](assets/img/developmemt-flow.png)

---

@snap[north-west text-gray span-100]
@size[1.5em](詳細設計)
@snapend

#### ここでの問題領域

@snap[south-west template-note text-gray]
@snapend

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

---

@snap[north-west text-gray span-100]
@size[1.5em](詳細設計)
@snapend

#### 要求と分析のフェーズでの成果物を元に概念モデルをコードに表現する

@snap[south-west template-note text-gray]
@snapend

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

```scala
final case class Warrior(
  id: WarriorId,
  name: WarriorName,
  attribute: Attribute,
  weapon: Option[Weapon],
  level: WarriorLevel,
) extends BaseEntity[WarriorId] {

  def equip(weapon: Weapon): Either[ConditionViolatedException, Warrior] = ???
}
```

+++

```scala
final case class WarriorId(value: Long)

final case class WarriorLevel(value: Int) {
  require(1 <= value & value <= 99)
}


final case class WarriorName(value: String)

```

+++

```scala
sealed trait Weapon {
  val name: String
  val offensivePower: Int
  val attribute: Attribute
  val levelConditionOfEquipment: Int
}
```

+++

```scala
object Weapon {

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
@size[1.5em](詳細設計: ユースケース)
@snapend

#### 抽出したユースケース記述(正常系、異常系)

図を記載

Note:
要求と分析フェーズで具体化したユースケースを実際にコードに落としてみます

@snap[south-west template-note text-gray]
@snapend

---

```scala
trait EquipNewWeaponToWarrior[F[_] {
  def apply(warrior: Warrior, newWeapon: Weapon): F[UseCaseResult]
}

object EquipNewWeaponToWarrior {

  case object InvalidCondition extends AbnormalCase {
    val cause: String = "この武器を装備するための条件を満たしていません"
  }
}
```

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
