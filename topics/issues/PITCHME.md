## Issues

開発における課題

Note:

次に、先ほど述べた理想の実現の前に起こりうる問題についても触れます。

---

@snap[north-west text-gray span-100]
@size[1.5em](Common issues)
@snapend

* Found oversight
* More maintenance cost, less release frequency
* Organization silos
* Code review cost
* Untrusted documents

@snap[south-west template-note text-gray]
There are some common issues with the software development process.
@snapend

Note:

ここでは以下のようなテーマを挙げたいと思います。

ひとつひとつ見ていきましょう。

---

@snap[north-west text-gray span-100]
@size[1.5em](Found oversight)
@snapend

* 要求・要件の見落とし
* すべてを見通すことは不可能
    * でも、今わかることは明らかにはしたい

@snap[south-west template-note text-gray]
You can not see everything from the very beginning.
@snapend

Note:

未来にわたってすべてを見通すことはできません。解決しようとする問題について理解があいまいになってしまうことは多かれ少なかれあります。

しかし、現時点における解決方法のスナップショットとして、仕様未定義の部分を極力減らせる手法はないものでしょうか。

---

@snap[north-west text-gray span-100]
@size[1.5em](More maint. cost, less release FQ)
@snapend

@snap[west]
なかなか価値を届けられないもどかしさ
@snapend

@snap[south-west template-note text-gray]
Things never do get cleaned up later and it causes more maintenance cost, less release frequency. 
@snapend

Note:

プロダクト開発をしていて、スケジュールに余裕があることの方が少ないかもしれません。

どうしても今はいったんこのままで、あとできれいにしようと緊急ではないものは後回しにされてきたことがどれほどあったでしょうか。

そのような 1 つ 1 つは小さなことの積み重ねによって、機能追加やバグ修正のコストが増加し、リリースがなかなかできないということも経験されたことはあるのではないでしょうか。

---

@snap[north-west text-gray span-100]
@size[1.5em](Organization silos)
@snapend

@snap[west span-80]
@img[domain_events](assets/img/business_flow_crossing_silos.png)
@snapend

@snap[south-west template-note text-gray]
Silos are inevitable; however, complex business flows are crossing the silos.
@snapend

Note:

プロダクトが大きくなり関係者が多くなればなるほど、特定の問題に特化した集団、サイロが生まれます。これは避けられないことです。分業のために組織やチーム再編というのはよくあることだと思います。

しかし、解決したい問題によってはサイロをまたがって、力をあわせる必要があります。1 つのサイロだけ見ていると、知覚されない、どのサイロにも属さないような課題が潜んでいるかもしれません。

各々のサイロに閉じた解決策では対処できない問題に対してはどう向き合えばいいのでしょうか。

---

@snap[north-west text-gray span-100]
@size[1.5em](Code review cost)
@snapend

@snap[west snap-50]
コードレビューで設計について議論し続けてしまう 
@snapend

@snap[south-west template-note text-gray]
TODO: english sub
@snapend

Note:

コードレビューで設計ミスや漏れが検知されること、それ自体はよいことです。しかし、常にその検知がコードレビューのタイミングであることを期待するとチームが疲弊します。

コードレビューで数十のコメントがつく/つけたといった経験はおありでしょうか? これはおうおうにして、設計についての認識齟齬が原因であることが経験上多いです。

例えばその振る舞いはこのモデルのものではない、といったものです。お互いの認識が最初から違い、違うことが判明するのがコードレビューで初めてとなると、非常にコミュニケーションに消耗しますし、リリースが遠のきます。

実装に入る前に、設計についてメンバー間で同期を取れる仕組みがほしいですね。

---

@snap[north-west text-gray span-100]
@size[1.5em](Untrusted documents)
@snapend

@snap[west snap-50]
@img[domain_events](assets/img/untrasuted_documents.png)
@snapend

@snap[south-west template-note text-gray]
Documents are not always up to date and continuous updating them is a hard thing. 
@snapend

Note:

誰しも最初は新人です。既存メンバの交わしている言葉がよくわからなかったり、巨大なソースコードやインフラストラクチャに取り組む際に、頼りになるドキュメントがあると助かります。

もちろん既存メンバであっても記憶力には限界があります。あとで見直したときに混乱が起きるようなドキュメントは困ります。

しかし、ドキュメントが更新されるかどうかは人次第です。いつもマメに更新してくれるあのメンバも、常にそうとは限りません。

---
@snap[north-west text-gray span-100]
@size[1.5em](What you can get from this session)
@snapend

@ul[](true)
- プロダクトの探求

- 設計手法
    - 要求/ドメインの分析
    - ドメインモデルの抽出
    - 凝集度の高いコンポーネント設計
@ulend
