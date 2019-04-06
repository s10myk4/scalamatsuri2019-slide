## Issues

--- 

### 優れたアーキテクチャは全てを解決するのか？

@snap[south-west template-note text-gray]
TODO: 日本語字幕
@snapend

---

### 私たちが実現したいことは何なのか？

@snap[south-west template-note text-gray]
TODO: 日本語字幕
@snapend

=======
@snap[north-west text-gray span-100]
@size[1.5em](プロダクト開発における問題)
@snapend

* 仕様未定義があとから見つかる
* 改修コスト増 -> リリース頻度低
* チームのサイロ化
* 信用できないドキュメント
* 評判のアーキテクチャ

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

次に、先ほど述べた理想の実現の前に起こりうる問題についても触れます。

---

@snap[north-west text-gray span-100]
@size[1.5em](仕様未定義があとから見つかる)
@snapend

* すべてを見通すことは不可能
    * でも、今わかることは明らかにはしたい

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

未来にわたってすべてを見通すことはできません。解決しようとする問題について理解があいまいになってしまうことは多かれ少なかれあります。

しかし、現時点における解決方法のスナップショットとして、仕様未定義の部分を極力減らせる手法はないものでしょうか。

---

@snap[north-west text-gray span-100]
@size[1.5em](改修コスト増 -> リリース頻度低)
@snapend

TODO: 茹でガエルの絵?

なかなか価値を届けられないもどかしさ

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

プロダクト開発をしていて、スケジュールに余裕があることの方が少ないかもしれません。

どうしても今はいったんこのままで、あとできれいにしようと緊急ではないものは後回しにされてきたことがどれほどあったでしょうか。

そのような 1 つ 1 つは小さなことの積み重ねによって、機能追加やバグ修正のコストが増加し、リリースがなかなかできないということも経験されたことはあるのではないでしょうか。

---

@snap[north-west text-gray span-100]
@size[1.5em](チームのサイロ化)
@snapend

TODO: サイロの絵

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

プロダクトが大きくなり関係者が多くなればなるほど、特定の問題に特化した集団、サイロが生まれます。これは避けられないことです。

しかし、問題によってはサイロをまたがって、力をあわせて解決する必要があります。もしかしたらどのサイロにも属さない問題もどこかに潜んでいるかもしれません。

各々のサイロに閉じた解決策では対処できない問題に対してはどう向き合えばいいのでしょうか。

---

@snap[north-west text-gray span-100]
@size[1.5em](信用できないドキュメント)
@snapend

TODO: ドキュメントを見て困惑している人の絵

@snap[south-west template-note text-gray]
TODO: Untrusted documents
@snapend

Note:

誰しも最初は新人です。既存メンバの交わしている言葉がよくわからなかったり、巨大なソースコードやインフラストラクチャに取り組む際に、頼りになるドキュメントがあると助かります。

もちろん既存メンバであっても記憶力には限界があります。あとで見直したときに混乱が起きるようなドキュメントは困ります。

しかし、ドキュメントが更新されるかどうかは人次第です。いつもマメに更新してくれるあのメンバも、常にそうとは限りません。

---

@snap[north-west text-gray span-100]
@size[1.5em](評判のアーキテクチャ)
@snapend

そのアーキテクチャ、フィットしていますか?

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

アーキテクチャの議論はつきないものです。しかしともすれば、ちまたでは優れたとされているアーキテクチャが、いついかなる状況に対してもフィットするかはわかりません。

> もちろん興味であったり今後のための実験として、あえてオーバーエンジニアリングしてみることは否定しません。

納得感をもってアーキテクチャの検討をするために下準備がいることでしょう。
