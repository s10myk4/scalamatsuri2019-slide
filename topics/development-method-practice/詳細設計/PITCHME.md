@snap[north-west text-gray span-100]
@size[1.5em](Detailed Design)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-focus4.png)
@snapend

---
@snap[north-west text-gray span-100]
@size[1.5em](Detailed Design)
@snapend

* ユースケース(正常系/異常系)、ドメインモデルをコードに定義する
* 外部との接合部(アダプター)の実装方針を決める

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

@ul[west](false)
- ドメインモデルの定義
- ユースケースの定義
- テストケースの定義
- アダプターの定義/実装方針
    - 入力受付(ex. Web)
    - 外部とのやり取り(ex. DB, File)
    - 出力方法(ex. JSON, HTML)
@ulend

@snap[south-west template-note text-gray]
@snapend

Note:
ここで期待する成果物は、概念モデルを以下のソースコードで表現し、アダプターの実装方針を決める

---
@snap[north-west text-gray span-100]
@size[1.5em](Define Weapon)
@snapend

```scala
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
```

```scala
object Warrior {
  sealed trait WarriorError extends DomainError
  
  object DifferentAttributeError extends WarriorError

  object NotOverLevelError extends WarriorError
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
@size[1.5em](Write Test Cases)
@snapend

- テストケース = 正常系 + 異常系の数

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
ユースケースの単体テストでユースケースが正常系と異常系の要件を満たすことを期待します。
詳細設計の段階でテストケースを記述することで、実装者に依存せずに実装が要件を満たすことを担保します。

---
@snap[north-west text-gray span-100]
@size[1.3em](What Do Domain & Use Case Express?)
@snapend

@snap[west]
#### 問題の本質はドメインに、ソフトウェア要件の詳細はユースケースに

@ul[](false)
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
@size[1.5em](Extracted Objects in Software)
@snapend

@ul[west]
- ビジネスロジックや要求の変更に対して柔軟な設計 
    - 凝集性の高いコンポーネント
    - 改修による変更対応箇所を局所化
    
- ユースケースが要件の変更を追従し要件を示す

- ソフトウェア上のオブジェクトは開発者が創造したものではない
    - ユビキタス言語
@ulend

Note:
要求の分析によって抽出された概念をそのままソフトウェアの実装に表現することで、どのような利点があるのでしょうか？
あらためて整理したいと思います

前のスライドで話したように
業務ルールの変化によるソフトウェアの変更箇所は対象のドメインモデルとユースケースに閉じ、
要件の変更に対するソフトウェアの変更箇所は対象のユースケースに閉じることで、改修による変更対応箇所を局所化されることで
凝集性の高いコンポーネントを実現し、ビジネスルールや要件の変更に対して柔軟な設計を実現できます

要件の変更に対しても、ユースケースの知識として表現されているのでトラックすることを実現します
前段でとりあげた信用できないドキュメントに惑わされることもなくなるかなと思います

今回の例である、戦士や武器という概念は開発者が勝手に創造したものではないですよね？
要求や要件定義のプロセスの中で抽出した概念です

開発者とビジネス関係者との対話において概念に対して同じ認識ができることによって、
コミュニケーションを活性化し要求者の求めていることをより正確に理解する事を促します
それをコード上に表現することで、実装での表現の乖離を防ぎ概念的な理解を促します
つまりそれが、ユビキタス言語なんですね
ソフトウェア上にオブジェクトとして定義されるものは開発者が創造したものではないのです

---
@snap[north-west text-gray span-100]
@size[1.5em](Architecture Strategy)
@snapend

@snap[west span-70]
![development-flow](assets/img/CleanArchitecture.jpg)
@snapend

@snap[south-east template-note]
@box[text-white rounded bg-orange box-padding text-05](Source: [The Clean Code Blog](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html))
@snapend

Note: 

概念的な設計における意味合いについておさらいしてきましたが、
実装の側面においてはアーキテクチャについても少し考える必要があるかもしれません

みなさんはアーキテクチャに対してどのようなことを期待していますか？

---
@snap[north-west text-gray span-100]
@size[1.5em](Purpose of the Architecture?)
@snapend

@ul[west]
- 求められるシステムを構築・保守するために必要な人材を最小限に抑えることである
@ulend

@snap[south-east template-note]
@box[text-white rounded bg-orange box-padding text-05](Source: [Clean Architecture 達人に学ぶソフトウェアの構造と設計](https://www.kadokawa.co.jp/product/301806000678/))
@snapend

Note:
先進的なアーキテクチャは一見多くのことを解決してくれるように感じますが、本当にそうなのでしょうか？

---
@snap[north-west text-gray span-100]
@size[1.5em](Important Point of Architecture)
@snapend

@ul[west](false)
- ドメインとユースケースのレイヤを用いることで業務知識と要件を実現するための知識をカプセル化
- 単方向な依存関係によりプラガブルな設計
    - Interface Adapters
    - DIP
@ulend

Note:
依存性の逆転を利用して、固有の実装への依存をプラガブルバーツとしてに外部に置く

---
@snap[north-west text-gray span-100]
@size[1.5em](Reduce Source Code Review Cost)
@snapend

- MRで要件やモデリングに対する指摘が発生することを極力抑えられる

Note:
要件定義、設計におけるフェーズの成果物を段階的にレビューをしているので、
実装後のMRのレビューは実装の観点に閉じています
