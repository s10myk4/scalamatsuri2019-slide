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
@snap[north-west text-gray span-100]
@size[1.5em](具体的な課題に掘り下げる)
@snapend

TODO 図を作成する

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
@size[1.5em](ゴール設定)
@snapend

日々の機能追加/改修/運用におけるコストをコントロールしたい(逓増を防ぐ)
@ul[west]()
* 既存の仕様を把握するためのコストを極力下げたい
* 属人的な要件定義、モデリングや設計を排除し、且つソフトウェアの機能要求の品質を担保したい
* 要件に対して凝集度の高い設計を実現し、改修に対する変更箇所を局所化したい
@ulend

---
@snap[north-west text-gray span-100]
@size[1.5em](具体的アプローチについて)
@snapend

- 開発における問題領域を分割し、領域ごとの課題に対して小さく検証できるようにする

- ICONIXプロセスを用いて、要件分析、ドメインモデルの探索を行う

- ソースコードが要求分析による概念モデルを踏襲し、要件の変化を追従す

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](このセッションから得られること)
@snapend

@ul[west]()
* 要求の分析から、関連する業務をモデリングする方法
* 曖昧な要求から、要件を明確にする方法
* 要件や業務領域に対して凝集度の高いコンポーネントの設計方法
@ulend

Note:
