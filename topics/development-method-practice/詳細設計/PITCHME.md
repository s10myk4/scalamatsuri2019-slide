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
Express use cases & domain models as Code.  
Consider the implementation policy of adapters.
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
Outputs in this phase is defined domain models, use cases, test cases and adapter IF as Code.
@snapend

Note:
ここで期待する成果物は、概念モデルをソースコードで表現することです。

また、ユースケースを実行する、ユースケースから実行されるような、外界とのやりとりを担うアダプターの実装方針の決定もここで行います。

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

Note:

では、ドメインモデルからソースコードで表現してみましょう。

これは武器の定義です。武器の名前や攻撃力、属性や装備するのに必要なレベルを持ちます。

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

Note:

武器の実体はここでは簡単のために定数として定義しています。実際にはこういった情報は何らかのストレージに保存されるでしょう。

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

Note:

武器の属性も同様に定数として定義しました。

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

Note:

次に戦士の定義です。戦士の ID や名前、属性、武器の装備有無、レベルを持ちます。

また、ドメインエラーとして、戦士の属性やレベルと合わない武器を装備したときのエラーを定義しています。

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

次にユースケースを表現します。

先にユースケースの実行結果を表す型を定義しました。

これは正常系か異常系かのみを表現するようにし、実装に依存しない形にしています。

---
@snap[north-west text-gray span-100]
@size[1.5em](Define Use Case)
@snapend

```scala
final class EquipWeaponToWarrior[F[_]] {
  def exec(warrior: Warrior, newWeapon: Weapon): F[UseCaseResult] = ???
}

object EquipWeaponToWarrior {

  object DifferentAttributeAndNotOverLevel extends AbnormalCase { ... }

  object DifferentAttribute extends AbnormalCase { ... }

  object NotOverLevel extends AbnormalCase { ... } 
}
```

Note:

戦士に武器を装備させるユースケースを表すクラスです。対象の戦士と武器を引数に、先ほど定義したユースケースの結果を表す型を返します。

異常系についてはそれぞれの結果がわかるように、具体的な型を定義してみました。

---
@snap[north-west text-gray span-100]
@size[1.5em](Write Test Cases)
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

次にテストケースです。ユースケースの単体テストを記述することで、ユースケースが正常系と異常系の要件を満たすことを期待します。

詳細設計の段階でテストケースを記述することで、実装者に依らず、実装が要件を満たすことを担保します。

---
@snap[north-west text-gray span-100]
@size[1.3em](What Do Domain & Use Case Express?)
@snapend

@snap[west]
#### 問題の本質はドメインに、ソフトウェア要件の詳細はユースケースに

@ul[](false)
- ドメインモデル  
  - ソフトウェアの要件に限らない業務の知識

- ユースケース
  - ソフトウェアの要件を実現するための処理
@ulend
@snapend

@snap[south-west template-note text-gray]
Business knowledge in the domain.  
Software requirements in use cases.
@snapend
   
Note:
ここまで進めてきましたが、ドメインロジックとユースケースというのはどのような違いがあるのでしょうか？
ということについてあらためて整理しておきます。

あくまで説明する上で、少々語弊のある表現かもしれませんが、
現金で支払いをしたら差額を返却する、という業務の特性があったとき、
これはソフトウェアの要件に限らない、ドメインロジックです。
ドメインロジックを特定のドメインモデルに閉じ込めることで業務を遂行する存在に限らず、ドメインの一貫性を保つことができます。

一方、これまで話にでた正常系、異常系はソフトウェアの要件を実現するための知識です。
この知識はユースケースの中に閉じこめます。

これらはそれぞれ役割に応じた責務を持つ、凝集度の高いコンポーネントになります。

では、今回の例である `ユーザーは、戦士に武器を装備することができる` に対して当てはめてみます。
戦士に武器を装備するためのレベルや属性に制約があること、これはドメインロジックです。
ユースケースは戦士に武器を装備させる際の結果が要件を満たすかを表現します。

--- 
@snap[north-west text-gray span-100]
@size[1.5em](Extracted Objects in Software)
@snapend

- 凝集性の高いコンポーネント |
    - 改修による変更対応箇所を局所化 |
         - 要求の変更に対して柔軟 |
- ソフトウェア上のオブジェクトは開発者だけで創造したものではない |
    - ユビキタス言語 |

@snap[south-west template-note text-gray]
Enable flexible design to requirements and business logic changes.
@snapend

Note:
ドメインモデルとユースケース、それぞれが凝集度の高いコンポーネントとなることについて考えました。

ここで、要求の分析によって抽出された概念をそのままソフトウェアの実装に表現することで、どのような利点があるのでしょうか？
あらためて整理したいと思います。

前のスライドでは、
業務ルールの変化によるソフトウェアの変更箇所は対象のドメインモデルに閉じ、
要件の変更に対するソフトウェアの変更箇所は対象のユースケースに閉じることで
凝集性の高いコンポーネントとなるとお話しました。
凝集性の高いコンポーネントは改修による変更対応箇所を局所化しますので、ビジネスルールや要件の変更に対して柔軟になります。

また、普段の会話で出てこないような、ソフトウェアの中だけに存在する概念も起こりづらくなります。
今回の例である、戦士や武器という概念は開発者だけで創造したものではないですよね？
要求や要件定義のプロセスの中で抽出した概念です。

開発者とビジネス関係者との対話において、同じ概念に対しては同じ認識ができることによって、
コミュニケーションを活性化し要求者の求めていることをより正確に理解する事を促します。
それをコード上に表現することで、実現したいことと実装の乖離を防ぐことができます。

つまりそれが、ユビキタス言語なんですね。
ソフトウェア上にオブジェクトとして定義されるものは開発者が創造したものではないのです。

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

概念モデルの表現とその設計について述べてきましたが、
実装におけるアーキテクチャについてもここで考えておきましょう。

実際にアプリケーションが価値を提供するためには、ユースケースとドメインモデルだけではなく、ユースケースを実行したり、ユースケースから実行されるアダプタが必要です。

このクリーンアーキテクチャの図からするとコントローラーから外側の部分ですね。

ところでみなさんはアーキテクチャに対してどのようなことを期待していますか？

---
@snap[north-west text-gray span-100]
@size[1.5em](Purpose of the Architecture?)
@snapend

@ul[west]
- 求められるシステムを構築・保守するために必要な人材を最小限に抑えることである
    - The goal of software architecture is to minimize the human resources required to build and maintain the required system.
@ulend

@snap[south-east template-note]
@box[text-white rounded bg-orange box-padding text-05](Source: [Clean Architecture 達人に学ぶソフトウェアの構造と設計](https://www.kadokawa.co.jp/product/301806000678/))
@snapend

Note:
アーキテクチャは何のためにあるのでしょう?

これはアンクルボブの著書からの引用ですが、`求められるシステムを構築・保守するために必要な人材を最小限に抑えること` とありました。

では、求められるシステムを構築・保守するために必要な人材を最小限に抑えるためにできることとはなんでしょうか。

---
@snap[north-west text-gray span-100]
@size[1.5em](Important Point of Architecture)
@snapend

@ul[west](false)
- ドメインとユースケースのレイヤを用いることで業務知識と要件を実現するための処理を構造化
- 単方向な依存関係によりプラガブルな設計
    - Interface Adapters
    - DIP
@ulend

Note:

先ほどまで記載したユースケースやドメインモデルは凝集度の高いコンポーネントでした。これはビジネスのルールの変化に合わせて変更されていきます。

と同時に、このプロダクトが求められる性能や信頼性も時とともに変化していきます。
最初は DB だけでじゅうぶんだったものがキャッシュが必要になる、といったケースは典型例かと思います。

ユースケースやドメインモデルの凝集度を保つためには、外界とのやり取りを表す部分とを疎結合にする必要があります。

具体的にはインターフェースを切るなど依存性の逆転を利用して、固有の実装への依存をプラガブルなバーツとしてに外部におくことになります。

クリーンアーキテクチャはこうしたレイヤリングをうまく表現していると感じます。

---
@snap[north-west text-gray span-100]
@size[1.5em](What resolved)
@snapend

- 暗黙知を形式地に
    - コードレビューで要件やモデリングに対する指摘が発生することを極力抑えられる

Note:
ここまでをチームで行った結果として、暗黙知を関係者同士の形式知としました。

いきなり実装に入ったレビューとなった場合、どうしても解釈の不一致から議論が発散してしまうことがあります。

あらかじめ認識をあわせておくことで、スムーズにレビューを行うことができるでしょう。

@snap[south-west template-note text-gray]
Good communication reduces source code review cost.
@snapend
