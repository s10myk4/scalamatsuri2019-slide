@snap[north-west text-gray span-100]
@size[1.5em](Analysis and Conceptual Design)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-focus3.png)
@snapend

Note:

* TODO: このフェーズでは以下を説明する 
    * 使う手法
    * How(実際にその手法を適用してみる)
    * アウトプット(適用した結果こうなる)

---

@snap[north-west text-gray span-100]
@size[1.5em](Analysis and Conceptual Design)
@snapend

ユースケース記述の曖昧さを解消する
- 新たなドメインモデルの発見
- ドメインモデルと振る舞いの関連性

@snap[south-west template-note text-gray]
Our purpose in this phase is to disambiguate use case descriptions.
@snapend

Note:
TODO 

ICONIXプロセスは初稿のドメインモデルを不完全なものだと仮定しており、
取りこぼしていたオブジェクトがロバストネス分析中で発見されると想定しています。
初稿のユースケース記述はたいてい曖昧で不完全であり、不正確なものになりがちです
それをサポートするためにロバストネス図による分析を行います。

ロバストネス分析の主要な目的は2つです。
ユースケース記述から曖昧さを取り除く
見逃したドメインモデルを発見する

---
@snap[north-west text-gray span-100]
@size[1.5em](Robustness Analysis)
@snapend

![](assets/img/robustness/robustness-icon.png)

@snap[south-west template-note text-gray]
Boundary: UI, interface of external service<br>
Entity: Object in business domain<br>
Control: function of software
@snapend

Note:
バウンダリ、エンティティ、コントローラというステレオタイプを用いて図を作成していきます

バウンダリは、システムと外部とのインターフェースを表していて、Webページの画面とかが具体的な例です
エンティティは、ドメイン上に出てくるモデル
コントローラは、バウンダリやエンティティを繋いだり、ソフトウェアの機能を表します

TODO
* 簡単なロバストネス分析のサンプル
---
@snap[north-west text-gray span-100]
@size[1.5em](Sample Image)
@snapend

![](assets/img/robustness/robustness-sample.png)
@snap[south-east template-note]
@box[text-white rounded bg-orange box-padding text-05](Source: [ユースケース駆動開発実践ガイド 図5.31](https://www.shoeisha.co.jp/book/detail/9784798114453))
@snapend

---
@snap[north-west text-gray span-100]
@size[1.5em](Rules)
@snapend

![](assets/img/robustness/robustness-rules.png)

Note:

---

@snap[north-west text-gray span-100]
@size[1.5em](Elements Extracting)
@snapend

#### Normal Case

```
B:バウンダリ      C:コントローラ     E:エンティティ
```
@ol[mylist-s](false)
- ユーザーは、自分の所有している戦士(E)の中から1人を選んでタップする(C)
- システムは、戦士詳細画面(B)を表示する(C)
- ユーザーは、装備ボタン(B)をタップする(C)
- システムは、武器一覧(E)を取得し、武器一覧画面(B)に表示する(C)
- ユーザーは、戦士(E)に装備したい武器(E)を一覧から選択して、決定ボタン(B)をタップする(C)
- システムは、戦士のレベル(E)が選択された武器のレベル(E)条件以上であるかを確認する(C)
- システムは、選択した武器の属性(E)と戦士の属性(E)が同じであるかを確認する(C)
- システムは、選択した武器を装備した戦士(E)を作成する(C)
- システムは、"装備が完了しました"というメッセージを画面(B)に表示する(C)
@olend

@snap[south-west template-note text-gray]
Mark use case descriptions and extract elements.
@snapend

Note:
ロバストネス図を作成する前に、ロバストネス図にマッピングするオブジェクトを抽出しておくと効率的です
バウンダリをB、コントローラをC、エンティティをEとしてユースケース記述内にマークをつけていきます

---
@snap[north-west text-gray span-100]
@size[1.5em](Elements Extracting)
@snapend


#### Abnormal Cases

```
B:バウンダリ      C:コントローラ     E:エンティティ
```
@ul[mylist-s](false)
- 戦士のレベル(E)が選択した武器のレベル条件(E)を満たしていない場合<br>システムは、"戦士が武器のレベル条件を満たしていないので装備できません"と戦士詳細画面(B)に表示する(C)

- 戦士の属性(E)と選択した武器の属性(E)が異なる場合<br>システムは、"戦士と武器の属性が異なるため装備できません"と戦士詳細画面(B)に表示する(C)

- 戦士のレベル(E)も属性(E)も異なる場合<br>システムは、"戦士が武器のレベル条件を満たしていないので装備できません 且つ 戦士と武器の属性が異なるため装備できません"と戦士詳細画面(B)に表示する(C)
@ulend

---
@snap[north-west text-gray span-100]
@size[1.5em](Robustness Diagram)
@snapend

@snap[west span-80]
@img[domain_events](assets/img/robustness/first-robustness-diagram.png)
@snapend

@snap[south-west template-note text-gray]
Is something wrong?
@snapend

Note:
このロバストネス図を眺めていると何か違和感を感じませんか？
20秒ほど眺めてどこが変なのか

---
@snap[north-west text-gray span-100]
@size[1.5em](Something wrong..)
@snapend

武器を装備する操作が発生していないのにバリデーションしてるの変だな？

@snap[south-west template-note text-gray]
Is it strange that you have validated even though there is no operation to equip weapons?
@snapend

Note:
戦士に武器を装備する操作が発生していないのに、武器一覧画面からバリデーション操作が繋がっているのは、
操作の流れとしておかしい

---
@snap[north-west text-gray span-100]
@size[1.5em](Robustness Analysis)
@snapend

@snap[west span-80]
@img[domain_events](assets/img/robustness/updated-robustness-diagram.png)
@snapend

@snap[south-west template-note text-gray]
@snapend

Note:
戦士に武器を装備するという操作が発生することで、バリデーションが発生するように図を書き換えてみました。  

@snap[south-west template-note text-gray]
Updated the diagram so that validation occurs when an operation to equip a warrior with a weapon occurs.
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](Update Use Case Descriptions)
@snapend

```diff
ユーザーは、自分の所有している戦士の中から1人を選んでタップする
システムは、戦士詳細画面を表示する
ユーザーは、装備ボタンをタップする
システムは、武器一覧を取得し、武器一覧画面に表示する
ユーザーは、戦士に装備したい武器を一覧から選択して、決定ボタンをタップする
+ システムは、戦士に武器を装備する
システムは、戦士のレベルが選択された武器のレベル条件以上であるかを確認する
システムは、選択した武器の属性と戦士の属性が同じであるかを確認する
- システムは、選択した武器を装備した戦士を作成する
システムは、"装備が完了しました"というメッセージを画面に表示する
```

@snap[south-west template-note text-gray]
Remove ambiguity from use case descriptions.
Discover new system behavior.
@snapend

---
@snap[north-west text-gray span-100]
@size[1.5em](Highly Cost)
@snapend

#### 全部のユースケース記述にこれをやるのは大変

@snap[south-west template-note text-gray]
Hard to do all use case descriptions.  
At first, it seems good to analyze for important domains and complex domains.
@snapend

Note:
ロバストネス分析はICONIXプロセスにおける奥義 / 真髄とされています。
しかし現実問題、全てのユースケース記述に足してロバストネス分析を行いのはとても大変だと思います

ビジネスにおいて重要なドメインや相対的にドメインの複雑性が高い場合に実施する
程度が最初はいいかなと思います
