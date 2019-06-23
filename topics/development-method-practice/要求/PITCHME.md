@snap[north-west text-gray span-100]
@size[1.5em](Requirements Analysis)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-focus2.png)
@snapend

Note:

取り組むべき問題、要求を決定しました。次はこの要求に対して分析を行います。

---
@snap[north-west text-gray span-100]
@size[1.5em](Requirements Analysis)
@snapend

@snap[west]
#### 私たちが機能を実装する上で考えていることは？

@ul[](false)
- その機能が実現したいこと
- ユーザーはどのようにその機能を利用するのか
- どのように作るか
- etc...
@ulend
@snapend

@snap[south-west template-note text-gray]
What we think of when implementing features
@snapend

Note:

本題に入る前にいったん私達が考えていることを振り返ってみましょう。

ステークホルダーからの要求を実現する際に、どのようなことを考えますか?

たとえば、その機能が実現したいことはなんだろうか? ユーザーはどのようにその機能を利用するのか? どのように作ろうか...といったことに考えを巡らすと思います。

---
@snap[north-west text-gray span-100]
@size[1.5em](Requirements Analysis)
@snapend

@snap[west]
#### どのように実現するかを概念レベルで整理する

@ul[](false)
- 要求について共通理解を得る
- 機能要求の全体像を把握する
- 要件: 制約や条件
@ulend

@snapend

@snap[south-west template-note text-gray]
Sort out at the conceptual level how to realize
@snapend

Note:

前フェーズで要求を導き出しました。

戦士が武器を装備できるという要求を実現するにあったって、どのようなことを理解しておく必要があるでしょうか?

要求を実現するために、どのような制約や条件が存在するのか、 つまり `要件` を分析によって明らかにしていきます。

このフェーズではその要求に対して分析を行い、振る舞いと制約を定義します。
要求を正しく理解していないと要件が定まりません。

---

@snap[north-west text-gray span-100]
@size[1.5em](Outputs)
@snapend

@snap[west]
@ul[](false)
- 初稿のユースケース記述  
    (ユーザーとシステムの振る舞い)

- 初稿のドメインモデル
@ulend
@snapend

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

ユースケース記述の例を挙げます。すべて `xxx は yyy を zzz する。` という書き方に統一されていますね。

---
@snap[north-west text-gray span-100]
@size[1.5em](Use Case Descriptions)
@snapend

@snap[west]
ポイント: 叙述的に書く / SVO を明確に
<hr/>

@ul[]()
- NG: パスワードを使った認証方式でログインできる
- OK: ユーザーは、IDとパスワードを入力して、<br/>
「ログイン」ボタンをクリックする
@ulend
@snapend

@snap[south-west template-note text-gray]
Write use case in active voice instead of passive voice and clarify its subject/verb/object  
@snapend

Note:

ユースケース記述のポイントは叙述的に書くことと、 SVO を明確にすることです。

叙述的に書くというのは、ログインできる、といった指示的な表現ではなく、~する、といったように書くということです。

~できるといった要求的な表現はアクターの振る舞いを隠してしまいがちです。
SVO も明確に記述することで、要件に対する認識の齟齬を減らせます。

---
@snap[north-west text-gray span-100]
@size[1.5em](Use Case Descriptions)
@snapend

@snap[west]
@ul[](false)
- ユースケース記述によって;
    - ユーザーとシステムの対話が明確になる
    - 明確になるにつれ、詳細の考慮が具体化する
@ulend
<hr/>

ユーザーは、IDとパスワードを入力して、  
「ログイン」ボタンをクリックする
@snapend

@snap[south-west template-note text-gray]
Use case descriptions help you clarify communications between users and the system.
@snapend

Note:

先のポイントを押さえたユースケース記述を行うことで、ユーザーとシステムとの対話が明確になります。

またこういった記述を行うことで、考慮すべき他の詳細に対しても具体化するでしょう。

---
@snap[north-west text-gray span-100]
@size[1.5em](Requirements)
@snapend

ユーザーは、戦士に武器を装備することができる

@snap[south-west template-note text-gray]
Our customer requirement: Users can have their warrior equip weapons.
@snapend

Note:

では、今回の例の顧客の要求である `ユーザーは、戦士に武器を装備することができる` について、ユースケース記述で表現してみましょう。

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

@snap[west span-33]
@img[domain_events](assets/img/warrior-index-page.png)
@snapend

@snap[midpoint span-33]
@img[domain_events](assets/img/warrior-detail-page.png)
@snapend

@snap[east span-33]
@img[domain_events](assets/img/weapon-index-page.png)
@snapend

Note:

それぞれ戦士一覧画面、戦士詳細画面、武器一覧画面です。

ここでのモックアップはあくまでチーム内でのコミュニケーションを促進するためだけのものなので、作成に時間をかける必要はありません。
あまり凝ったデザインを最初から作ってしまうと、ここで焦点を当てたい議論からそれてしまう(例えば細かいスタイリングやボタンの位置)かもしれないからです。

---

@snap[north-west text-gray span-100]
@size[1.5em](GUI Prototyping)
@snapend

@snap[west span-33]
@img[domain_events](assets/img/warrior-index-page_01.png)
@snapend

@snap[east span-66]
@ul[]()
ユーザーは、@color[Red](戦士一覧画面)から武器を装備したい@color[Red](戦士)を選択する
@ulend
@snapend

Note:

`ユーザーは、戦士に武器を装備することができる` ことを実現するには、はじめにユーザーが武器を装備させたい戦士を選択する必要があるでしょう。

---

@snap[north-west text-gray span-100]
@size[1.5em](GUI Prototyping)
@snapend

@snap[west span-33]
@img[domain_events](assets/img/warrior-detail-page_01.png)
@snapend

@snap[east span-67]
@ul[]()
- システムは、@color[Red](戦士詳細画面)を表示する
- ユーザーは、@color[Red](装備ボタン)をタップする
@ulend
@snapend

Note:

ユーザーが戦士を選択したら、システムが戦士の詳細画面を表示させます。

その後、ユーザーは、武器を装備させることを選択します。

---

@snap[north-west text-gray span-100]
@size[1.5em](GUI Prototyping)
@snapend

@snap[west span-33]
@img[domain_events](assets/img/weapon-index-page_01.png)
@snapend

@snap[east span-67]
@ul[]()
- システムは、@color[Red](武器一覧)を取得し、@color[Red](武器一覧画面)を表示する
- ユーザーは、戦士に装備したい@color[Red](武器)を一覧から選択して、@color[Red](決定ボタン)をタップする
@ulend
@snapend

Note:

システムは武器一覧をどこからか取得し、ユーザーに見せます。ユーザーはその一覧から装備させたい武器を選択します。

---

@snap[north-west text-gray span-100]
@size[1.5em](First Draft Use Case Descriptions)
@snapend

@ul[mylist]()
- ユーザーは、@color[Red](戦士一覧画面)から武器を装備したい@color[Red](戦士)を選択する
- システムは、@color[Red](戦士詳細画面)を表示する
- ユーザーは、@color[Red](装備ボタン)をタップする
- システムは、@color[Red](武器一覧)を取得し、@color[Red](武器一覧画面)を表示する
- ユーザーは、戦士に装備したい@color[Red](武器)を一覧から選択して、@color[Red](決定ボタン)をタップする
- システムは、選択した武器を@color[Red](装備)した@color[Red](戦士)を作成する
- システムは、"武器を装備しました"というメッセージを@color[Red](戦士詳細画面)に表示する
@ulend

Note:

武器装備後のふるまいも追加し、初稿のユースケース記述ができました。

ユースケース記述がうまく書けないなと思ったら、GUI プロトタイプを利用することを検討してみてください。
一覧やその対象物はユーザーが決定をくだすための判断材料、つまりモデルとなるでしょうし、ボタンといった要素はなんらかのイベントを生成するリスナになりますので、ふるまいを見つけるヒントとなることでしょう。

TODO:

ここではまだ正常系とは言わない。つまり、分析がまだ終わっていないということを表す

---
@snap[north-west text-gray span-100]
@size[1.5em](First Draft Domain Models)
@snapend

@snap[west]
@ul[](false)
- 戦士
- 武器
- 装備
@ulend
@snapend

Note:

また、もう 1 つのアウトプットであるドメインモデルについても今までの情報から抽出しておきます。

ここではこれが Entity/VO なのかについては考えず、出てきたキーワードを挙げています。

---

@snap[north-west text-gray span-100]
@size[1.5em](Conversation with PO)
@snapend

@snap[west]
#### 戦士はどんな武器でも装備できるんですか？
@snapend

Note:

ユースケース記述を記述しているうちに、1 つ疑問がわきました。戦士はどんな武器でも装備できてしまうんでしょうか? PO と相談してみましょう。

---

@snap[north-west text-gray span-100]
@size[1.5em](Can warriors equip every weapon?)
@snapend


@snap[west span-33]
@img[domain_events](assets/img/warrior-detail-page_02.png)
@snapend

@snap[east span-67]
@ul[](false)
- いきなり強い武器を装備できないよう、武器を@color[Red](装備する条件)に@color[Red](レベル)をつけたい
- 光の戦士に闇の魔法の杖を装備できるのは変なので、@color[Red](属性)で装備できる武器も制限したい
@ulend
@snapend

Note:

PO からはこのような返答がありました。

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
+レベル
+属性
```

Note:

この PO との会話をもとに、ドメインモデルをアップデートしました。

---

@snap[north-west text-gray span-100]
@size[1.5em](Update Use Case Descriptions)
@snapend

``` diff
ユーザーは、戦士一覧画面から武器を装備したい戦士を選択する
システムは、戦士詳細画面を表示する
ユーザーは、装備ボタンをタップする
システムは、武器一覧を取得し、武器一覧画面を表示する
ユーザーは、戦士に装備したい武器を一覧から選択して、決定ボタンをタップする
+システムは、戦士のレベルが選択された武器のレベル条件以上であるかを確認する
+システムは、選択した武器の属性と戦士の属性が同じであるかを確認する
システムは、選択した武器を装備した戦士を作成する
システムは、"武器を装備しました"というメッセージを戦士詳細画面に表示する
```

Note: 

ユースケースも同様に更新しましょう。

先程のPOとの対話からわかる制約として、武器を装備するためには、`指定レベル以上であること` と、 `武器と戦士の属性が一致すること` を満たす必要があるようです。

システムのふるまいとして追加しました。  

---
@snap[north-west text-gray span-100]
@size[1.5em](Abnormal Cases)
@snapend

@snap[west]
#### 制約を満たさない場合のふるまいはどうする?

@ul[]()
- レベル条件を満たしていない
- 武器と戦士の属性が違う
@ulend

@snapend

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

@snap[west]
#### Abnormal Cases 

@ul[mylist](false)
- 戦士のレベルが選択した武器のレベル条件を満たしていない場合<br/>システムは、`戦士が武器のレベル条件を満たしていないので装備できません`と戦士詳細画面に表示する

- 戦士の属性と選択した武器の属性が異なる場合<br/>システムは、`戦士と武器の属性が異なるため装備できません` と戦士詳細画面に表示する

- 戦士のレベルも属性も異なる場合<br/>システムは、`戦士が武器のレベル条件を満たしていないので装備できません 且つ 戦士と武器の属性が異なるため装備できません` と戦士詳細画面に表示する
@ulend
@snapend

Note:

異常系についてもユースケース記述を記載します。

---
@snap[north-west text-gray span-100]
@size[1.5em](Keys of the Use Case Descriptions)
@snapend

@snap[west]
@ul[](false)
- 叙述的な記述
- SVOの文法を意識する
- 詳細の技術に関する概念を用いない
    - e.g. Database, HTTP
@ulend
@snapend

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
  <img src="assets/img/udd-cover.jpg" width="75%" />
<a/>
@snapend

@snap[south-west template-note text-gray]
ICONIX process helps you analyze and model in your development process.
@snapend

Note:

このフェーズで利用してきたユースケース記述などは ICONIX プロセスというアプローチのなかで言及されています。

もし詳細について知りたい方は `ユースケース駆動開発実践ガイド` を参照してみてください。 
