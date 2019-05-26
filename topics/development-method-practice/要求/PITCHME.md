### Requirements Analysis

![development-flow](assets/img/developmemt-flow.png)

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](要求分析)
@snapend

#### 私たちが機能を実装する上で考えていることは？   
- その機能が実現したいこと
- ユーザーがどのようにその機能を利用するのか
- どのように作るか
- etc...

TODO具体化

@snap[south-west template-note text-gray]
@snapend

Note:
TODO
要求と要件の狭間のグレーな部分を図示するとわかりやすいかも

要求に対する共通認識を持たずに実装をしていくとどうなるの？

---
@snap[north-west text-gray span-100]
@size[1.5em](Requirements Analysis)
@snapend

どのように作るかを概念的に整理する
  - 要求について共通理解を得る
  - 機能要求の全体像を把握する
  - 要件: 制約や条件

@snap[south-west template-note text-gray]
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
@snapend

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](Use Case Descriptions)
@snapend

### TODO サンプルの記述

@snap[south-west template-note text-gray]
@snapend

---
@snap[north-west text-gray span-100]
@size[1.5em](ユースケース記述とは)
@snapend

### TODO 基本的な記述のルール

@snap[south-west template-note text-gray]
@snapend

---
@snap[north-west text-gray span-100]
@size[1.5em](ユースケース記述)
@snapend

#### TODO
- ユースケース記述によってシステムの対話が明確になる
- 明確になって行くと、詳細の考慮が具体化する

@snap[south-west template-note text-gray]
@snapend

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](Requirements)
@snapend

ユーザーは、戦士に武器を装備することができる

@snap[south-west template-note text-gray]
Our customer requirement
Users can have their warrior equip weapons.
@snapend

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](GUI Prototyping)
@snapend

TODO

@snap[south-west template-note text-gray]
@snapend

Note:

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
@size[1.5em](First Draft Domain Models)
@snapend

- 戦士
- 武器
- 装備

Note:

ここではこれが Entity/VO なのかについては考えず、出てきたキーワードを挙げています。

TODO

* 初稿のドメインモデルがなぜ必要か
    * 言葉の概念をあわせていくことで、変更のコストを抑えるため
  

---

@snap[north-west text-gray span-100]
@size[1.5em](First Draft Use Case Descriptions)
@snapend

```text
ユーザーは、自分の所有している戦士の中から1人を選んでタップする
システムは、戦士の詳細画面を表示する
ユーザーは、武器装備ボタンをタップする
システムは、武器一覧を取得し、武器装備画面に表示する
ユーザーは、戦士に装備したい武器を一覧から選択して、決定ボタンをタップする
システムは、選択した武器を装備した戦士を作成する
システムは、"武器を装備しました"というメッセージを画面に表示する
```

Note:

TODO:

ここではまだ正常系とは言わない。つまり、分析がまだ終わっていないということを表す

---

@snap[north-west text-gray span-100]
@size[1.5em](Conversation with PO)
@snapend

#### 戦士はどんな武器でも装備できるんですか？

- いきなり強い武器を装備できないように武器を装備する条件を付けたい
- 光の戦士に闇の魔法の杖とか装備できるのは変なので属性で装備できる武器も制限したい

---

@snap[north-west text-gray span-100]
@size[1.5em](Update Domain Models)
@snapend

- 戦士
- 武器
- 装備
- 装備する条件
- 属性

---

@snap[north-west text-gray span-100]
@size[1.5em](Update Use Case Descriptions)
@snapend

```text
ユーザーは、自分の所有している戦士の中から1人を選んでタップする
システムは、戦士の詳細画面を表示する
ユーザーは、武器装備ボタンをタップする
システムは、武器一覧を取得し、武器装備画面に表示する
ユーザーは、戦士に装備したい武器を一覧から選択して、決定ボタンをタップする
システムは、戦士のレベルが選択された武器のレベル条件以上であるかを確認する
システムは、選択した武器の属性と戦士の属性が同じであるかを確認する
システムは、選択した武器を装備した戦士を作成する
システムは、"武器を装備しました"というメッセージを画面に表示する
```

Note: 

先程のPOとの対話からわかる制約
武器を装備するためには、指定レベルを満たす必要がある
武器と戦士の属性が同じである必要がある

---
@snap[north-west text-gray span-100]
@size[1.5em](Abnormal Cases)
@snapend

#### 先程のPOとの対話からわかる制約
- 武器を装備するためには、指定レベルを満たす必要がある
- 武器と戦士の属性が同じである必要がある

---
@snap[north-west text-gray span-100]
@size[1.5em](Update Use Case Descriptions)
@snapend

#### Abnormal Cases
```text
・戦士のレベルが選択した武器のレベル条件を満たしていない場合
システムは、"戦士が武器のレベル条件を満たしていないので装備できません"というメッセージを戦士詳細画面に表示する

・戦士の属性と選択した武器の属性が異なる場合
システムは、"戦士と武器の属性が異なるため装備できません"というメッセージを戦士詳細画面に表示する

・戦士のレベルも属性も異なる場合
システムは、"戦士が武器のレベル条件を満たしていないので装備できません 
且つ 戦士と武器の属性が異なるため装備できません"というメッセージを戦士詳細画面に表示する
```

Note:

ここで、いままではいわゆる正常系というものを考えてきました。しかし、ユーザーは常にこちらの意図通りの入力をするとは限りません。

もし、制約を違反するような操作をされた場合、どうなるか、つまり異条系についても考えなければなりません。

もちろん制約があるのならば、制約を違反したい場合どうなるのかという考えになるのはごく自然なことです。

しかし、システムのふるまいはどうあるべきかの定義も必要です。
異常系まで含めて定義しておくことで、考慮漏れによるバグをより体系的に防ぐことができます。

---

@snap[north-west text-gray span-100]
@size[1.5em](Conversation with PO)
@snapend

#### 戦士に対して武器って何個も持たせられるんですか？
#### 武器って剣みたいな攻撃するものだけですか？ 防具みたいなものもあるんですか？

Note:
開発者が要件を具体化する中で疑問がわきました。

---

@snap[north-west text-gray span-100]
@size[1.5em](Conversation with PO)
@snapend

#### 戦士に対して武器って何個も持たせられるんですか？
#### 武器って剣みたいなものだけですか？ 防具みたいなものもあるんですか？
- 武器は1つだけ持たせられるようにしたい
- 今後は防具のようなアイテムも装備できるようにしたいが、今は武器だけで良い

Note:
POと対話する中で、...
ということがわかりました

---

@snap[north-west text-gray span-100]
@size[1.5em](Update Domain Models)
@snapend

- 戦士
- 武器
- アイテム
- 装備
- 装備する条件
- 属性

---

@snap[north-west text-gray span-100]
@size[1.5em](Update Use Case Descriptions)
@snapend

#### Normal Case

```text
ユーザーは、自分の所有している戦士の中から1人を選んでタップする
システムは、戦士詳細画面を表示する
ユーザーは、装備ボタンをタップする
システムは、武器一覧を取得し、武器一覧画面に表示する
ユーザーは、戦士に装備したい武器を一覧から選択して、決定ボタンをタップする
システムは、戦士のレベルが選択された武器のレベル条件以上であるかを確認する
システムは、選択した武器の属性と戦士の属性が同じであるかを確認する
システムは、選択した武器を装備した戦士を作成する
システムは、"装備が完了しました"というメッセージを画面に表示する
```

---

@snap[north-west text-gray span-100]
@size[1.5em](Update Use Case Descriptions)
@snapend

#### Abnormal Cases
```text
・戦士のレベルが選択した武器のレベル条件を満たしていない場合
システムは、"戦士が武器のレベル条件を満たしていないので装備できません"というメッセージを戦士詳細画面に表示する

・戦士の属性と選択した武器の属性が異なる場合
システムは、"戦士と武器の属性が異なるため装備できません"というメッセージを戦士詳細画面に表示する

・戦士のレベルも属性も異なる場合
システムは、"戦士が武器のレベル条件を満たしていないので装備できません 
且つ 戦士と武器の属性が異なるため装備できません"というメッセージを戦士詳細画面に表示する

```

---

@snap[north-west text-gray span-100]
@size[1.5em](Keys of the Use Case Descriptions)
@snapend

- 叙述的な記述
- SVOの文法を意識する
- 詳細の技術に関する概念を用いない(ex. Database、Http)

@snap[south-west template-note text-gray]
@snapend

Note:
ここで抽出したモデルは、DBなどの固有の技術に依存する概念ではなく、
このソフトウェア上で扱う業務に関連したモデルである

TODO もっと色々具体的な例を入れて重要な観点をわかりやすく説明したい

---
@snap[north-west text-gray span-100]
@size[1.5em](ICONIX Process)
@snapend

- 不明瞭が要求から具体的な振る舞い要求を導出できる
- 形式知
    - ドメインモデル
    - ユースケース記述
    - GUIプロトタイプ
- プラガブルなプロセス
    - 小さなイテレーション
- 特定の技術に依存しない

@snap[south-west template-note text-gray]
TODO: 日本語字幕
@snapend

Note:

このフェーズで利用する手法として ICONIX プロセスを紹介します。
