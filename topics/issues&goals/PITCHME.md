## Issues
---
@snap[north-west text-gray span-100]
@size[1.5em](Common issues)
@snapend

- 負債が積もる
    - 緊急で重要なモノが常に優先され、負債は積もる
    
- 改修やリリースのコストが逓増

@snap[south-west template-note text-gray]
@snapend

Note:
何年か運用されているプロダクトの開発では、これらのような課題と全く関係ないよ！って場合の方が少ないのではないのでしょうか？
私もいくつかのプロダクト開発に関わってきましたが、度合いは違うにせよこのようなよくある課題と向き合わなくてはならないことが
多かったように思っています。

リリース間に合わせないといけないから一旦リリースして、あとできれいにしようとチームで話しても、次の機能追加/改修/バグ対応と
次々と優先順位の高い要求が上がってきて、コードをきれいにする事まで中々手が回らなく、負債が積もるスパイラルに入り、
改修やリリースに関するコストが逓増し、コントロールすることが難しくなっていました。

TODO

---
## 課題を掘り下げる

---
@snap[north-west text-gray span-100]
@size[1.5em](既存仕様の把握)
@snapend

@ul[west]()
* 既存の仕様を読み解くのに思ったより時間がかかる
    * 自分が書いたコードより他の人が書いたコードを読んで既存仕様を拾ったりする方が多い
    * 影響範囲が曖昧な場合、関係がないことを手当たり次第に確認する必要がある(考古学)
@ulend

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](コードレビュー)
@snapend

@ul[west]()
* コードレビューで要件やモデリングについての議論が発散する
* 個々に要求/要件に対する認識、理解が異なる
@ulend

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](モデリング/設計)
@snapend

@ul[west]()
* 個人の要件定義などのスキルに依存して、要件の漏れによるバグ
* 属人的なモデリング
* エンジニアが創造したモデルの氾
@ulend

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](プロダクト開発を取り巻く課題)
@snapend

Note:
ここまで、主に本セッションに特に関連すると思われるプロダクト開発におけるよくあるだろうと思われる課題について話してきましたが、

できる限り小さい取り組みとしてプロダクト開発に適用し、個々の課題に対してどのように取り組めばいいのかの1つの例として参考にしていただければ
という思い出で資料を作成しました。

???

このセッションのゴールをよｐり明確にし、アプローチの全体像について話したあとに、
個々の手法などの話をしていきます

---

### 流行りのアーキテクチャは全てを解決してくれるのか？

---
@snap[north-west text-gray span-100]
@size[1.5em](設計・アーキテクチャの目的とは？)
@snapend

@ul[west]()
* 求められるシステムを構築・保守するために必要な人材を最小限に抑えることである
@ulend

@snap[south-east template-note]
@box[text-white rounded bg-orange box-padding text-05](Source: [Clean Architecture 達人に学ぶソフトウェアの構造と設計](https://www.kadokawa.co.jp/product/301806000678/))
@snapend

Note:

今回このセッションでは、要件定義や設計、アーキテクチャなどプロダクト開発の広範な部分について扱って行きますが、
まずは多くの部分で重要な観点となるアーキテクチャや設計の目的に触れておきたいと思います。

みなさんは、どのようなことを目的、期待して設計、アーキテクチャについて考えていますか？

?????

ボブおじさんの言葉によれば、
求められるシステムを構築・保守するために必要な人材を最小限に抑えることである
設計の品質は、顧客のニーズを満たすために必要な労力で計測できる。
必要な労力が少なく、システムのライフタイム全体で低く保たれているならば、その設計は優れている。
逆に、リリースごとに労力が増えるなら、その設計は優れていない。

---
## Goals

---
@snap[north-west text-gray span-100]
@size[1.5em](目指している状態/モチベーション)
@snapend

@ul[west](true)
* 日々の機能追加/改修/運用におけるコストをコントロールしたい(逓増を防ぐ)
    * 既存の仕様を把握するためのコストを極力下げたい
    * 属人的な要件定義、モデリングや設計を排除し、且つソフトウェアの機能要求の品質を担保したい
    * 要件に対して凝集度の高い設計を実現し、改修に対する変更箇所を局所化したい
@ulend

---
@snap[north-west text-gray span-100]
@size[1.5em](アプローチの全体像)
@snapend

@snap[west]
![development-flow](assets/img/development-flow.png)

* 開発における問題(関心)領域を分割し、領域ごとの課題に対して小さく検証できるようにする
@snapend

---
@snap[north-west text-gray span-100]
@size[1.5em](アプローチの全体像)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-focus2.png)

ICONIXプロセスを用いて、要件分析、ドメインモデルの探索を行う
@snapend

Note: 


---
@snap[north-west text-gray span-100]
@size[1.5em](アプローチの全体像)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-focus4.png)

ソースコードが要求分析による概念モデルを踏襲し、要件の変化を追従す
@snapend

Note:

????

このあとに、図で示したように開発における問題(関心)領域ごとにフェーズを分割して、
問題や課題に対して、どのようなアプローチを用いて行くかという話をしていきます

---
@snap[north-west text-gray span-100]
@size[1.5em](このセッションから得られること)
@snapend

@ul[west](true)
* 要求の分析から関連する業務をモデリングする方法
* 曖昧な要求から要件を明確にする方法
* 要件や業務領域に対して凝集度の高いコンポーネントの設計方法
@ulend

Note:

---
