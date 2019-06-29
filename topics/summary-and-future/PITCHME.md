@snap[north-west text-gray span-100]
@size[1.5em](Recap)
@snapend

@snap[west]
![development-flow](assets/img/development-flow.png)
@snapend

Note:
TODO

全体の話の流れとしては
プロダクト開発における一部の主要な課題を取り上げて、
プロダクト開発におけるフローごとに問題領域を分割、課題を分類し、
それぞれのフローでどのように課題に取り組むかという形式で話を進めた

- 個々の課題に対して、小さなアプローチを検証することをできるようにフェーズごとの目的、成果物を明確にした

- 課題に対してどのようなアプローチで取り組もうとしているかを振り返る

---
@snap[north-west text-gray span-100]
@size[1.5em](Recap)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-focus1.png)
@snapend

Note:

今回は省略しましたが、要求の抽出、妥当性を検証、つまりビジネス自体についてどう関わっていくかについても重要なフェーズです。

---
@snap[north-west text-gray span-100]
@size[1.5em](Recap)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-focus2.png)
@snapend

Note:

要求分析では、要求を概念レベルで整理することを目的としていました。

要求の全体像やその中でのシステムの振る舞い/対話を明らかにし、ソフトウェアとしての要件を求めました。

アウトプットはユースケース分析やドメインモデルでした。

---
@snap[north-west text-gray span-100]
@size[1.5em](Recap)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-focus3.png)
@snapend

Note:

次の概念設計ではユースケース記述のあいまいさを解消することで、取りこぼしていたドメインオブジェクトの発見や、オブジェクトとシステムのふるまいとの関係性を検証しました。

手法としてはロバストネス分析を用いました。

---
@snap[north-west text-gray span-100]
@size[1.5em](Recap)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-focus4.png)
@snapend

Note:

詳細設計ではユースケース記述やドメインモデルをもとに、コードに落とし込むことを行いました。

実装に入る前にこのフェーズを行うことで、メンバ間での解釈が一致しているかが検知できます。

また、ユースケース記述やドメインモデルをいかに凝集度の高いコンポーネントとするかについても重要視しました。

---
@snap[north-west text-gray span-100]
@size[1.5em](Recap)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-focus5.png)
@snapend

Note:

これまでのフェーズをもとに実装に入っていきます。

凝集度の高いコンポーネントを利用することで、保守性の高い実装が実現できることを実感いただけたのではないかと思います。

---
@snap[north-west text-gray span-100]
@size[1.5em](Recap)
@snapend

@snap[west]
あくまで手法を検証した上で、  
日々のプロダクト開発における課題に対して問題領域を分解して適用しやすいアプローチを探った  
一例として参考にしてもらえたら嬉しいです
@snapend

