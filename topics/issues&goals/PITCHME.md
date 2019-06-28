## Issues
---
@snap[north-west text-gray span-100]
@size[1.5em](Common issues)
@snapend

@snap[west span-50]
@img[focus_problem](assets/img/deadline.png)
@snapend

@ul[east span-45](false)
- 負債が積もる
    - 緊急で重要なモノが常に優先
- 改修やリリースのコストが逓増
@ulend

@snap[south-west template-note text-gray]
- Debt accumulates.  
- Increase the cost of repair and release. 
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

@snap[south-west template-note text-gray]
Delve into the issues.
@snapend

---
@snap[north-west text-gray span-100]
@size[1.5em](Understanding of Existing Spes.)
@snapend

@ul[west]()
* 既存の仕様を読み解くのに思ったより時間がかかる
* 自分が書いたコードより他の人が書いたコードを読んで既存仕様を拾ったりする方が多い
* 影響範囲が曖昧な場合、関係がないことを手当たり次第に確認する必要がある
@ulend

Note:
既存仕様の把握

---
@snap[north-west text-gray span-100]
@size[1.5em](Code Review)
@snapend

@ul[west]()
* コードレビューで要件やモデリングについての議論が発散する
* 個々に要求/要件に対する認識、理解が異なる
@ulend

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](Modeling & Design)
@snapend

@ul[west]()
* 個人の要件定義などのスキル依存
* 要件の漏れ、未定義によるバグ
* 属人的なモデリング
@ulend

Note:

---
@snap[north-west text-gray span-100]
@size[1.5em](Issues surrounding Development)
@snapend

@snap[]()
@img[Oooops](assets/img/stress-man.jpg)
<!-- 障害や負債の蓄積を起こさないために、どう考えて取り組むべきなのか？ -->
@snapend

Note:
ここまで、主に本セッションに特に関連すると思われるプロダクト開発におけるよくあるだろうと思われる課題について話してきましたが、

???
障害や負債の蓄積を起こさないために、どう考えて取り組むべきなのか？

できる限り小さい取り組みとしてプロダクト開発に適用し、個々の課題に対してどのように取り組めばいいのかの1つの例として参考にしていただければ
という思い出で資料を作成しました。

???

このセッションのゴールをよｐり明確にし、アプローチの全体像について話したあとに、
個々の手法などの話をしていきます

---
@snap[north-west text-gray span-100]
@size[1.5em](Goals)
@snapend

@ul[west](true)
* 日々の機能追加/改修/運用におけるコストをコントロールしたい(逓増を防ぐ)
    * 既存の仕様を把握するためのコストを極力下げたい
    * 属人的な要件定義、モデリングや設計を排除し、且つソフトウェアの内部品質を担保したい
    * 要件に対して凝集度の高い設計を実現し、改修に対する変更箇所を局所化したい
@ulend

@snap[south-west template-note text-gray]
@snapend

Note:
目指している状態/モチベーション

---
@snap[north-west text-gray span-100]
@size[1.5em](Talking About)
@snapend

@ul[west](true)
* 要求の分析から関連する業務をモデリングする方法
* 曖昧な要求から要件を明確にする方法
* 要件や業務領域に対して凝集度の高いコンポーネントの設計方法
@ulend

@snap[south-west template-note text-gray]
@snapend

Note:
このセッション話すこと

---
@snap[north-west text-gray span-100]
@size[1.5em](Overview of development flow)
@snapend

@snap[west]
![development-flow](assets/img/development-flow.png)
　開発における問題(関心)領域を分割し、  
フェーズごとの課題に対して小さく検証
@snapend

@snap[south-west template-note text-gray]
Divide the problem area in development flow,  
Verify small for each phase issues.
@snapend

---
@snap[north-west text-gray span-100]
@size[1.5em](Overview of development flow)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-intro1.png)
ICONIXプロセスを用いて要件分析、ドメインモデルの探索を行う
@snapend

@snap[south-west template-note text-gray]
Analyze requirements and search domain models using the ICONIX process.
@snapend

---
@snap[north-west text-gray span-100]
@size[1.5em](Overview of development flow)
@snapend

@snap[west]
![development-flow](assets/img/development-flow-intro2.png)
　ソースコードが要求分析による概念モデルを踏襲し、  
要件の変化を追従
@snapend

@snap[south-west template-note text-gray]
Code follows a conceptual design with requirements analysis and follows changes in requirements.
@snapend

Note:

????

このあとに、図で示したように開発における問題(関心)領域ごとにフェーズを分割して、
問題や課題に対して、どのようなアプローチを用いて行くかという話をしていきます

