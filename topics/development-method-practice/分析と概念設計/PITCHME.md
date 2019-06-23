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
- 新たなドメインオブジェクトの発見
- ドメインオブジェクトと振る舞いの関連性

@snap[south-west template-note text-gray]
Our purpose in this phase is to disambiguate use case descriptions.
@snapend

Note:
ここではユースケース記述の健全性を検証するため分析方法を用いて、初稿のユースケース記述の曖昧さを解消し、  
取りこぼしていたドメインオブジェクトの発見やオブジェクトとシステムの振る舞いとの関係性を検証していきます。  


ICONIXプロセスは初稿のドメインモデルを不完全なものだと仮定しており、
取りこぼしていたオブジェクトがロバストネス分析中で発見されると想定しています。
初稿のユースケース記述はたいてい曖昧で不完全であり、不正確なものになりがちです
それをサポートするためにロバストネス図による分析を行います。

---
@snap[north-west text-gray span-100]
@size[1.5em](Sample Image)
@snapend

![](assets/img/robustness/robustness-sample.png)
@snap[south-east template-note]
@box[text-white rounded bg-orange box-padding text-05](Source: [ユースケース駆動開発実践ガイド 図5.31](https://www.shoeisha.co.jp/book/detail/9784798114453))
@snapend

Note:
ロバストネス図というのはこのようなものなんですが、  
ユースケース記述をオブジェクトシステムの振る舞いの関係性にフォーカスして表記されたものです。

ロバストネス分析がどういったものかを簡単にではありますが説明します。  

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

---
@snap[north-west text-gray span-100]
@size[1.5em](Rules)
@snapend

![](assets/img/robustness/robustness-rules.png)

Note:
ロバストネス分析では、資料に記したルールに基づいて図を作成することで
ユースケース記述の曖昧さを検知することを目的とします

オブジェクト同士がつながることを不正とし、間には何らかのシステムの振る舞いが関連するといった感じです。 

---
@snap[north-west text-gray span-100]
@size[1.5em](Elements Extracting)
@snapend

#### Normal Case
@ul[mylist-s](true)
- ユーザーは、@color[Green](戦士一覧画面)から@color[Red](武器)を装備したい@color[Red](戦士)を@color[Blue](選択する)
- システムは、@color[Green](戦士詳細画面)を@color[Blue](表示する)
- ユーザーは、@color[Green](装備ボタン)を@color[Blue](タップする)
- システムは、@color[Red](武器一覧)を@color[Blue](取得し)、@color[Green](武器一覧画面)に@color[Blue](表示する)
- ユーザーは、@color[Red](戦士)に装備したい@color[Red](武器)を一覧から選択して、@color[Green](決定ボタン)を@color[Blue](タップする)
- システムは、@color[Red](戦士のレベル)が選択された@color[Red](武器のレベル条件)以上であるかを@color[Blue](確認する)
- システムは、選択した@color[Red](武器の属性)と@color[Red](戦士の属性)が同じであるかを@color[Blue](確認する)
- システムは、選択した@color[Red](武器)を装備した@color[Red](戦士)を@color[Blue](作成する)
- システムは、"装備が完了しました"というメッセージを@color[Green](戦士詳細画面)に@color[Blue](表示する)
@ulend

@snap[south-west template-note text-gray]
Mark use case descriptions and extract elements.  
@color[Green](Boundary is Green),@color[Blue](Control is Blue),@color[Red](Entity is Red)
@snapend

Note:
実際にロバストネス図を作成するときには、事前にロバストネス図にマッピングするオブジェクトを抽出しておくと効率的です  
バウンダリをB、コントローラをC、エンティティをEとしてユースケース記述内に印をつけていきます

---
@snap[north-west text-gray span-100]
@size[1.5em](Elements Extracting)
@snapend

#### Abnormal Cases
@ul[mylist-s](true)
- @color[Red](戦士のレベル)が選択した@color[Red](武器のレベル条件)を満たしていない場合<br>システムは、"戦士が武器のレベル条件を満たしていないので装備できません"と@color[Green](戦士詳細画面)に@color[Blue](表示する)

- @color[Red](戦士の属性)と選択した@color[Red](武器の属性)が異なる場合<br>システムは、"戦士と武器の属性が異なるため装備できません"と@color[Green](戦士詳細画面)に@color[Blue](表示する)

- @color[Red](戦士のレベル)も@color[Red](属性)も異なる場合<br>システムは、"戦士が武器のレベル条件を満たしていないので装備できません 且つ 戦士と武器の属性が異なるため装備できません"と@color[Green](戦士詳細画面)に@color[Blue](表示する)
@ulend

@snap[south-west template-note text-gray]
Mark use case descriptions and extract elements.  
@color[Green](Boundary is Green),@color[Blue](Control is Blue),@color[Red](Entity is Red)
@snapend

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
@size[1.5em](Something wrong?)
@snapend

システムが戦士に武器を装備する操作を行っていないのに、バリデーション操作をしている

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
Updated the diagram so that validation occurs when an operation to equip a warrior with a weapon occurs.
@snapend

Note:
戦士に武器を装備するという操作を追加し、バリデーションが行われるように図を書き換えてみました。  

---
@snap[north-west text-gray span-100]
@size[1.5em](Update Use Case Descriptions)
@snapend

```diff
ユーザーは、戦士一覧画面から武器を装備したい戦士を選択する
システムは、戦士詳細画面を表示する
ユーザーは、装備ボタンをタップする
システムは、武器一覧を取得し、武器一覧画面に表示する
ユーザーは、戦士に装備したい武器を一覧から選択して、決定ボタンをタップする
+ システムは、戦士に武器を装備する
システムは、戦士のレベルが選択された武器のレベル条件以上であるかを確認する
システムは、選択した武器の属性と戦士の属性が同じであるかを確認する
- システムは、選択した武器を装備した戦士を作成する
システムは、"装備が完了しました"というメッセージを戦士詳細画面に表示する
```

@snap[south-west template-note text-gray]
Remove ambiguity from use case descriptions.
Discover new system behavior.
@snapend

---
@snap[north-west text-gray span-100]
@size[1.5em](Tips)
@snapend

#### 全てのユースケース記述にこれをやるのは大変
- 複雑性の高いドメインや要求

@snap[south-west template-note text-gray]
Hard to do all use case descriptions.  
At first, it seems good to analyze for important domains and complex domains.
@snapend

Note:
ロバストネス分析はICONIXプロセスにおける真髄とされています。
しかし現実問題、全てのユースケース記述に対してロバストネス分析を行いのはとても大変だと感じました。

ビジネスにおいて重要なドメインや相対的にドメインの複雑性が高い場合に実施する
程度が最初はいいかなと思います

---
@snap[north-west text-gray span-100]
@size[1.5em](Robustness Analysis)
@snapend

@snap[west span-80]
@img[domain_events](assets/img/robustness/updated-robustness-diagram.png)
@snapend

@snap[south-west template-note text-gray]
Disambiguate use case descriptions.  
Discover new system behavior and domain models.
@snapend


