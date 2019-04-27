### 要求定義

![development-flow](assets/img/developmemt-flow.png)

---

@snap[north-west text-gray span-100]
@size[1.5em](要求フェーズの問題領域)
@snapend

#### ここでの問題領域
- 機能要求の全体像を把握
- ドメインモデルを抽出

@snap[south-west template-note text-gray]
@snapend

Note:
ドメインモデルとは、あるプロジェクトやサービスに関する知識

---

@snap[north-west text-gray span-100]
@size[1.5em](要求フェーズでの成果物)
@snapend

#### 要求フェーズで期待する成果物

#### 振る舞い要求(ユースケース記述)
#### 初稿のドメインモデル

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](想定する要求:ユースケース)
@snapend

### ユーザーは、戦士に武器を装備することができる

@snap[south-west template-note text-gray]
Customer requested specification  

The user can equip the weapon to the warrior.
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](要求定義)
@snapend

#### 書くこと TODO
- ユースケース記述によってシステムの対話が明確になる
- 明確になって行くと、詳細の考慮が具体化する

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](要求定義)
@snapend

#### 私たちが機能を実装する上で考えていることは？  

<br>
- その機能が実現したいこと
- ユーザーがどのようにその機能を利用するのか
- どのように作るか

etc...

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](要求定義)
@snapend

#### どのように作るかを概念的に整理する

#### ->振る舞い要求を明らかにする

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](ユースケース記述とは)
@snapend

### ユースケース記述
### TODO

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](GUIプロトタイピング)
@snapend

![](assets/img/warrior-index-page.png)

---
@snap[north-west text-gray span-100]
@size[1.5em](GUIプロトタイピング)
@snapend

![](assets/img/warrior-detail-page.png)

---
@snap[north-west text-gray span-100]
@size[1.5em](GUIプロトタイピング)
@snapend

![](assets/img/weapon-index-page.png)

---

@snap[north-west text-gray span-100]
@size[1.5em](初稿のドメインモデル)
@snapend

- 戦士
- 武器
- 装備

---

@snap[north-west text-gray span-100]
@size[1.5em](初稿のユースケース記述)
@snapend

#### 正常系(晴れの日コース)
```
ユーザーは、自分の所有している戦士の中から1人を選んでタップする
システムは、戦士の詳細画面を表示する
ユーザーは、武器装備ボタンをタップする
システムは、武器一覧を取得し、武器装備画面に表示する
ユーザーは、戦士に装備したい武器を一覧から選択して、決定ボタンをタップする
システムは、選択した武器を装備した戦士を作成し、保存する
システムは、"武器を装備しました"というメッセージを画面に表示する
```

---

@snap[north-west text-gray span-100]
@size[1.5em](POとの対話)
@snapend

#### 戦士はどんな武器でも装備できるんですか？
- いきなり強い武器を装備できてもチート感強すぎるので、武器を装備する条件を付けたい
- 戦士に魔法の杖とか装備できるのは変なので、属性で装備できる武器も制限したい

---

@snap[north-west text-gray span-100]
@size[1.5em](ドメインモデルの更新)
@snapend

- 戦士
- 武器
- 装備
- 装備する条件
- 属性
- 課金

---

@snap[north-west text-gray span-100]
@size[1.5em](ユースケース記述の更新)
@snapend

#### 正常系(晴れの日コース)
```
ユーザーは、自分の所有している戦士の中から1人を選んでタップする
システムは、戦士の詳細画面を表示する
ユーザーは、武器装備ボタンをタップする
システムは、武器一覧を取得し、武器装備画面に表示する
ユーザーは、戦士に装備したい武器を一覧から選択して、決定ボタンをタップする
システムは、戦士のレベルが選択された武器のレベル条件以上であるかを確認する
システムは、選択した武器の属性と戦士の属性が同じであるかを確認する
システムは、選択した武器を装備した戦士を作成し、保存する
システムは、"武器を装備しました"というメッセージを画面に表示する
```

Note: 

先程のPOとの対話からわかる制約
武器を装備するためには、指定レベルを満たす必要がある
武器と戦士の属性が同じである必要がある

---

@snap[north-west text-gray span-100]
@size[1.5em](異常系について考える)
@snapend

#### 先程のPOとの対話からわかる制約
- 武器を装備するためには、指定レベルを満たす必要がある
- 武器と戦士の属性が同じである必要がある

---

@snap[north-west text-gray span-100]
@size[1.5em](ユースケース記述の更新)
@snapend

#### 異常系(雨の日コース)
```
・戦士のレベルが選択した武器のレベル条件を満たしていない場合
システムは、"この武器を装備するための条件を満たしていません"というメッセージを戦士詳細画面に表示する

・戦士の属性と選択した武器の属性が異なる場合
システムは、"この武器を装備するための条件を満たしていません"というメッセージを戦士詳細画面に表示する
```

---

@snap[north-west text-gray span-100]
@size[1.5em](POとの対話)
@snapend

#### 戦士に対して武器って何個も持たせられるんですか？
#### 武器って剣みたいな攻撃するものだけですか？ 防具みたいなものもあるんですか？

Note:
開発者が要件を具体化する中で疑問がわきました。

---

@snap[north-west text-gray span-100]
@size[1.5em](POとの対話)
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
@size[1.5em](ドメインモデルの更新)
@snapend

- 戦士
- 武器
- アイテム
- 装備
- 装備する条件
- 属性
- 課金

---

@snap[north-west text-gray span-100]
@size[1.5em](ユースケース記述の更新)
@snapend

#### 正常系(晴れの日コース)
```
ユーザーは、自分の所有している戦士の中から1人を選んでタップする
システムは、戦士詳細画面を表示する
ユーザーは、装備ボタンをタップする
システムは、武器一覧を取得し、武器一覧画面に表示する
ユーザーは、戦士に装備したい武器を一覧から選択して、決定ボタンをタップする
システムは、戦士のレベルが選択された武器のレベル条件以上であるかを確認する
システムは、選択した武器の属性と戦士の属性が同じであるかを確認する
システムは、選択した武器を装備した戦士を作成し、保存する
システムは、"装備が完了しました"というメッセージを画面に表示する
```

---

@snap[north-west text-gray span-100]
@size[1.5em](ユースケース記述の更新)
@snapend

#### 異常系(雨の日コース)
```
・戦士のレベルが選択した武器のレベル条件を満たしていない場合
システムは、"この武器を装備するするための条件を満たしていません"というメッセージを戦士詳細画面に表示する

・戦士の属性と選択した武器の属性が異なる場合
システムは、"この武器を装備するための条件を満たしていません"というメッセージを戦士詳細画面に表示する

・武器がすでに装備されている場合
システムは、"すでに武器が装備されています"というメッセージを戦士詳細画面に表示する
```

---

@snap[north-west text-gray span-100]
@size[1.5em](ユースケース記述のポイント)
@snapend

- 別紙にまとめるかもしれない
- 叙述的な記述
- SVOの文法を意識する
...

#### 簡単にユースケース記述する上でのポイントを説明する
#### 詳細は本か私のスライドを参照してねw

@snap[south-west template-note text-gray]
@snapend


