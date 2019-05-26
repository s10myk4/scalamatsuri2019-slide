### Requirements Analysis

![development-flow](assets/img/developmemt-flow.png)

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](Requirements Analysis)
@snapend

#### 私たちが機能を実装する上で考えていることは？   
- その機能が実現したいこと
- ユーザーがどのようにその機能を利用するのか
- どのように作るか
- etc...

@snap[south-west template-note text-gray]
What we think of when implementing features
@snapend

Note:
TODO
要求と要件の狭間のグレーな部分を図示するとわかりやすいかも

要求に対する共通認識を持たずに実装をしていくとどうなるの？

---
@snap[north-west text-gray span-100]
@size[1.5em](Requirements Analysis)
@snapend

#### どのように実現するかを概念レベルで整理する
- 要求について共通理解を得る
- 機能要求の全体像を把握する
- 要件: 制約や条件

@snap[south-west template-note text-gray]
Sort out at the conceptual level how to realize
@snapend

Note:

TODO整理

前フェーズで要求を導き出しました。

戦士が武器を装備できるという要求を
を実現するにあったってどのよう概念が

要求を実現するために、どのような制約や条件が存在するのか、 つまり要件を分析によって
明らかにしていきます。

このフェーズではその要求に対して分析を行い、振る舞いと制約を定義します。
要求を正しく理解していないと要件が定まりません。

---

@snap[north-west text-gray span-100]
@size[1.5em](Outputs)
@snapend

- 初稿のユースケース記述  
    (ユーザーとシステムの振る舞い)

- 初稿のドメインモデル

@snap[south-west template-note text-gray]
In this phase, outputs are first draft use case descriptions and domain models. 
@snapend

Note:

このフェーズでのアウトプットは以下です。

- 初稿のユースケース記述(ユーザーとシステムの振る舞い)
- 初稿のドメインモデル

ユースケース記述というのはあまり馴染みの無い方もいらっしゃるかもしれませんね。

---
@snap[north-west text-gray span-100]
@size[1.5em](Use Case Descriptions)
@snapend

- システムは、ログイン画面を表示する
- ユーザーは、IDとパスワードを入力して、<br/>
「ログイン」ボタンをクリックする
- システムは、...

@snap[south-west template-note text-gray]
An example of use case description
@snapend

Note:

ユースケース記述の例を挙げます。

---
@snap[north-west text-gray span-100]
@size[1.5em](Use Case Descriptions)
@snapend

ポイント: 叙述的に書く / SVO を明確に

<hr/>

- OK: ユーザーは、IDとパスワードを入力して、<br/>
「ログイン」ボタンをクリックする
- NG: パスワードを使った認証方式でログインできる

@snap[south-west template-note text-gray]
Write use case in active voice instead of passive voice and clarify its subject/verb/object  
@snapend

---
@snap[north-west text-gray span-100]
@size[1.5em](Use Case Descriptions)
@snapend

- ユースケース記述によって;
    - システムの対話が明確になる
    - 明確になるにつれ、詳細の考慮が具体化する

@snap[south-west template-note text-gray]
Use case descriptions help you clarify communications between users and the system.
@snapend

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](Requirements)
@snapend

ユーザーは、戦士に武器を装備することができる

@snap[south-west template-note text-gray]
Our customer requirement: Users can have their warrior equip weapons.
@snapend

Note:

今回の例では、顧客の要求は `ユーザーは、戦士に武器を装備することができる` というものでした。これをユースケース記述で表現してみましょう。

---
@snap[north-west text-gray span-100]
@size[1.5em](GUI Prototyping)
@snapend

GUI プロトタイプを作成し、視覚的な支援を得る

@snap[south-west template-note text-gray]
GUI prototyping is often useful visual aids when writing a use case.
@snapend

Note:

とはいえ、いきなりユースケース記述が書けるかが想像しづらかったり、ふるまいについてチーム内で認識の齟齬があるかもしれません。

そのようなときは GUI プロトタイプを作成してみましょう。視覚的な支援がユースケース記述に対するヒントになるかもしれません。

今回のユースケースに対してもいくつか作ってみました。

---

@snap[north-west text-gray span-100]
@size[1.5em](GUI Prototyping)
@snapend

![](assets/img/warrior-index-page.png)

---
@snap[north-west text-gray span-100]
@size[1.5em](GUI Prototyping)
@snapend

![](assets/img/warrior-detail-page.png)

---
@snap[north-west text-gray span-100]
@size[1.5em](GUI Prototyping)
@snapend

![](assets/img/weapon-index-page.png)

---
@snap[north-west text-gray span-100]
@size[1.5em](First Draft Use Case Descriptions)
@snapend

@ul[mylist]()
- ユーザーは、自分の所有している戦士の中から1人を選んでタップする
- システムは、戦士の詳細画面を表示する
- ユーザーは、武器装備ボタンをタップする
- システムは、武器一覧を取得し、武器装備画面に表示する
- ユーザーは、戦士に装備したい武器を一覧から選択して、決定ボタンをタップする
- システムは、選択した武器を装備した戦士を作成する
- システムは、"武器を装備しました"というメッセージを画面に表示する
@ulend

Note:

GUI プロトタイプも参考に、初稿のユースケース記述ができました。

TODO:

ここではまだ正常系とは言わない。つまり、分析がまだ終わっていないということを表す

---
@snap[north-west text-gray span-100]
@size[1.5em](First Draft Domain Models)
@snapend

- 戦士
- 武器
- 装備

Note:

また、もう 1 つのアウトプットであるドメインモデルについても今までの情報から抽出しておきます。

ここではこれが Entity/VO なのかについては考えず、出てきたキーワードを挙げています。

---

@snap[north-west text-gray span-100]
@size[1.5em](Conversation with PO)
@snapend

#### 戦士はどんな武器でも装備できるんですか？ 

- いきなり強い武器を装備できないよう、武器を装備するレベル条件を付けたい |
- 光の戦士に闇の魔法の杖を装備できるのは変なので、属性で装備できる武器も制限したい |

Note:

ユースケース記述を記述しているうちに、1 つ疑問がわきました。戦士はどんな武器でも装備できてしまうんでしょうか? PO と相談してみましょう。

- いきなり強い武器を装備できないよう、装備するレベル条件を付けたい
- 光の戦士に闇の魔法の杖を装備できるのは変なので、属性で装備できる武器も制限したい

なるほど。装備するにも制約があるようですね。

---
@snap[north-west text-gray span-100]
@size[1.5em](Update Domain Models)
@snapend

``` diff
戦士
武器
装備
+装備する条件
+属性
```

Note:

この PO との会話をもとに、ドメインモデルをアップデートしました。

---

@snap[north-west text-gray span-100]
@size[1.5em](Update Use Case Descriptions)
@snapend

``` diff
ユーザーは、自分の所有している戦士の中から1人を選んでタップする
システムは、戦士の詳細画面を表示する
ユーザーは、武器装備ボタンをタップする
システムは、武器一覧を取得し、武器装備画面に表示する
ユーザーは、戦士に装備したい武器を一覧から選択して、決定ボタンをタップする
+システムは、戦士のレベルが選択された武器のレベル条件以上であるかを確認する
+システムは、選択した武器の属性と戦士の属性が同じであるかを確認する
システムは、選択した武器を装備した戦士を作成する
システムは、"武器を装備しました"というメッセージを画面に表示する
```

Note: 

ユースケースも同様に更新しましょう。

先程のPOとの対話からわかる制約として、武器を装備するためには、`指定レベルを満たす必要がある` および `武器と戦士の属性が同じである必要がある`ようです。

システムのふるまいを追加しました。  

---
@snap[north-west text-gray span-100]
@size[1.5em](Abnormal Cases)
@snapend

#### 以下制約を満たさない場合のふるまいはどうする?
- レベル条件を満たす必要がある
- 武器と戦士の属性が同じである必要がある

@snap[south-west template-note text-gray]
What if users select an invalid weapon?
@snapend

Note:

ここで、いままではいわゆる正常系というものを考えてきました。しかし、ユーザーは常にこちらの意図通りの入力をするとは限りません。

もし、制約を違反するような操作をされた場合どうなるか、つまり異常系についても考えなければなりません。

もちろん制約があるのならば、制約を違反したい場合どうなるのかという考えになるのはごく自然なことです。

しかし、システムのふるまいはどうあるべきかの定義も必要です。
異常系まで含めて定義しておくことで、考慮漏れによるバグをより体系的に防ぐことができます。

---
@snap[north-west text-gray span-100]
@size[1.5em](Update Use Case Descriptions)
@snapend

#### Abnormal Cases 

@ul[mylist](false)
- 戦士のレベルが選択した武器のレベル条件を満たしていない場合<br/>システムは、`戦士が武器のレベル条件を満たしていないので装備できません`と戦士詳細画面に表示する

- 戦士の属性と選択した武器の属性が異なる場合<br/>システムは、`戦士と武器の属性が異なるため装備できません` と戦士詳細画面に表示する

- 戦士のレベルも属性も異なる場合<br/>システムは、`戦士が武器のレベル条件を満たしていないので装備できません 且つ 戦士と武器の属性が異なるため装備できません` と戦士詳細画面に表示する
@ulend

Note:

異常系についてもユースケース記述を記載します。

---
@snap[north-west text-gray span-100]
@size[1.5em](Keys of the Use Case Descriptions)
@snapend

- 叙述的な記述
- SVOの文法を意識する
- 詳細の技術に関する概念を用いない
    - e.g. Database, HTTP

@snap[south-west template-note text-gray]
@snapend

Note:

ひととおりユースケース記述が完成したので振り返ってみます。ポイントは叙述的な記述をすること、SVO の文法を意識することでした。

また、ユースケース記述の中に、DB に保存するであったり、HTTP リクエストの内容を変換する、といった言葉が含まれていないことに気づかれた方もいらっしゃるかもしれません。
ここで抽出したモデルは、DB などの固有の技術に依存する概念ではなく、このソフトウェア上で扱う業務に関連したモデルですので、特定の技術に依存しないように書くことが重要です。

TODO もっと色々具体的な例を入れて重要な観点をわかりやすく説明したい

---
@snap[north-west text-gray span-100]
@size[1.5em](ICONIX Process)
@snapend

@snap[west span-50]
@ul[mylist](false)
- 不明瞭な要求から具体的な振る舞い要求を導出できる
- 形式知
    - ドメインモデル
    - ユースケース記述
    - GUIプロトタイプ
- プラガブルなプロセス
    - 小さなイテレーション
- 特定の技術に依存しない
@ulend
@snapend

@snap[east span-50]
<a href="https://www.seshop.com/product/detail/8320">
  <img src="https://www.shoeisha.co.jp/static/splus/1323/book/image/9784798114453.jpg" width="75%" />
<a/>
@snapend

@snap[south-west template-note text-gray]
ICONIX process helps you analyze and model in your development process.
@snapend

Note:

このフェーズで利用してきたユースケース記述などは ICONIX プロセスというアプローチのなかで言及されています。

もし詳細について知りたい方は `ユースケース駆動開発実践ガイド` を参照してみてください。 
