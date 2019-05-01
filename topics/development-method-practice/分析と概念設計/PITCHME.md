### 分析と概念設計

![development-flow](assets/img/developmemt-flow.png)

Note:

* TODO: このフェーズでは以下を説明する 
    * 使う手法
    * How(実際にその手法を適用してみる)
    * アウトプット(適用した結果こうなる)

---

@snap[north-west text-gray span-100]
@size[1.5em](分析と概念設計フェーズの問題領域)
@snapend

#### ユースケース記述を分析・検証

- ドメインモデルのアップデート
- 新たなドメインモデルの抽出
- ドメインモデルに振る舞いを割り当てる

@snap[south-west template-note text-gray]
@snapend

Note:

このフェーズの目的は前のフェーズでのアウトプットであるユースケース記述の妥当性を検証すること、
新たなドメインモデルの発掘・アップデートです。

また、ドメインモデルとふるまいの関係を明らかにしてきます。

これがそのまま成果物となります。

---

@snap[north-west text-gray span-100]
@size[1.5em](概念設計で重要なこと)
@snapend

TODO どこに差し込むか再考
####  概念設計の段階では、詳細の技術に関する概念を用いない
(ex. DB、http)

Note:
ここで抽出したモデルは、DBなどの固有の技術に依存する概念ではなく、
このソフトウェア上で扱う業務に関連したモデルである

---

@snap[north-west text-gray span-100]
@size[1.5em](利用する手法)
@snapend

#### ロバストネス分析

@snap[south-west template-note text-gray]
@snapend

Note:

TODO

* 簡単なロバストネス分析のサンプル
* BCE の説明

---

@snap[north-west text-gray span-100]
@size[1.5em](ロバストネス分析とは)
@snapend

- 奥義
#### ユースケース記述が正しく書かれていることを検証する
#### 発見した振る舞いがどのオブジェクトに関連するかを分析する

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](概念(BCE)抽出?)
@snapend

#### 正常系
```
ユーザーは、自分の所有している戦士(E)の中から1人を選んでタップする(C)
システムは、戦士詳細画面(B)を表示する(C)
ユーザーは、装備ボタン(B)をタップする(C)
システムは、武器一覧(E)を取得し、武器一覧画面(B)に表示する(C)
ユーザーは、戦士(E)に装備したい武器(E)を一覧から選択して、決定ボタン(B)をタップする(C)
システムは、戦士のレベル(E)が選択された武器のレベル(E)条件以上であるかを確認する(C)
システムは、選択した武器の属性(E)と戦士の属性(E)が同じであるかを確認する(C)
システムは、選択した武器を装備した戦士(E)を作成し、保存する(C)
システムは、"装備が完了しました"というメッセージを画面(B)に表示する(C)
```

Note:

ロバストネス分析の図を作成する前に、ロバストネス分析の図にマッピングするオブジェクトを抽出しておきましょう。

---

@snap[north-west text-gray span-100]
@size[1.5em](概念(BCE)抽出?)
@snapend


#### 異常系
```
・戦士のレベル(E)が選択した武器のレベル条件(E)を満たしていない場合
システムは、"この武器を装備するするための条件を満たしていません"というメッセージを戦士詳細画面(B)に表示する(C)

・戦士の属性(E)と選択した武器の属性(E)が異なる場合
システムは、"この武器を装備するための条件を満たしていません"というメッセージを戦士詳細画面(B)に表示する(C)

・武器(E)がすでに装備されている場合
システムは、"すでに武器が装備されています"というメッセージを戦士詳細画面(B)に表示する(C)
```

---

@snap[north-west text-gray span-100]
@size[1.5em](ロバストネス分析)
@snapend

![](assets/img/robustness_sumple1.jpg)

@snap[south-west template-note text-gray]
@snapend

Note:

TODO 武器を装備するコントロールがないので追加

ここでロバストネス分析を眺めてもらう。後述の実はミスがあったことの前振り。

---

@snap[north-west text-gray span-100]
@size[1.5em](ロバストネス分析)
@snapend

- TODO ユースケース記述がブラッシュアップされる例を示せると良いな(難しい)
    - 実際に抜けがあったので、これ以降のページでブラッシュアップする様を書く!


@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](ロバストネス分析)
@snapend

- TODO ドメインモデルに振る舞いを割り当てる

@snap[south-west template-note text-gray]
@snapend

---

### これって全部のユースケースにやるの大変じゃない？
//TODO ブラックアウト

---

@snap[north-west text-gray span-100]
@size[1.5em](ロバストネス分析のポイント)
@snapend

#### ビジネスの観点で重要なドメインにおいて実施するのが良さそう

- 簡単にロバストネス分析をする上でのポイントを説明
- 詳細は本か私のスライドを参照してねw

@snap[south-west template-note text-gray]
@snapend

