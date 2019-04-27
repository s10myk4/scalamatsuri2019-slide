## Sample Theme

対象とする題材

---

@snap[north-west text-gray span-100]
@size[1.5em](題材と背景)
@snapend

* オンラインRPG: Clean Quest
* われわれは Clean Quest の開発者
* 利用数が伸び悩み。次の一手を打ちたい

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

われわれはとあるゲーム開発会社の開発者であり、オンラインRPG Clean Quest を開発しています。

Clean Quest ではユーザーが自分のキャラクターを作成・操作し、ダンジョン探索や他のプレイヤーと対戦することができます。

ただ、近年全体の利用者数が伸び悩み、なんとか次の一手を打ちたい、というのが会社全体の意向としてあります。 

---

@snap[north-west text-gray span-100]
@size[1.5em](全体の理解)
@snapend

* 各自意欲的に問題の解決を図ろうとはしている
* 取り組むべき問題の観点がサイロに閉じている
* 全体の把握が必要そうだ

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

毎週そこかしこで現状を打破するための会議がセッティングされ、喧々諤々とした議論が行われるのですが、なかなかこれというものが出てきません。

ある会議中では XXX が問題だといい、別の会議中ではまた別の YYY がやり玉に上がるなど、
それぞれが各自の専門性(サイロ)から意見を述べるものの、本当の問題が何なのか、どこから着手すべきかが見えにくくなってしまっているようです。

これでは話が進まないと誰もが思い始めた結果、まずは現状を把握するために、各部署から主要なメンバが集まりワークショップを開くことになったのでした。

---

@snap[north-west text-gray span-100]
@size[1.5em](Event Storming)
@snapend

`https://www.eventstorming.com/`

@ul[](false)
* 複雑なビジネスドメインに対して、
  `共同`で`探究`を行うためのワークショップ
    * 共同: 主要なステークホルダーが一堂に会する
    * 探究: 物事を理解しようとする取り組み
@ulend

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

ここでは手法の 1 つとして Event Storming を紹介します。

Event Storming とは、複雑なビジネスドメインに対して共同で探究を行うためのワークショップの 1 形式です。

---

@snap[north-west text-gray span-100]
@size[1.5em](Event Storming で行うこと)
@snapend

* システムを全体を把握
* 解決すべき問題を発見
* いま利用できる最良の情報を収集
* ベストと思われる開始地点から解決策を実行

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

Event Storming では、それぞれの専門性・立場を越えて以下について探究します。 

* システムを全体を把握する
* 解決すべき問題を発見する
* いま利用できる最良の情報を集める
* ベストと思われる開始地点から解決策を実行する 

---

@snap[north-west text-gray span-100]
@size[1.5em](Domain Event の書き出し)
@snapend

@snap[west span-50]
@img[domain_events](assets/img/domain_events.jpg)
@snapend

@snap[east span-50]
@ul[](false)
* オレンジ色の付箋
* 動詞の過去形
* ドメインエキスパートに関係あること
@ulend
@snapend

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

Event Storming では、Domain Event を以下のような形式で書き出します。

* オレンジ色の付箋
* 動詞の過去形
* ドメインエキスパートに関係あること

ただし付箋の色については一貫していれば任意です。ここではオレンジ色とします。

名詞やを中心にするのではなくなぜイベントについて注目するかというと、名詞は静的なものであり、コンテキストごとに矛盾が発生しがちだからです。
たとえば Order という名詞が出てきたとして、販売や梱包、配達や請求といったそれぞれの分野、つまりコンテキストを持つドメインエキスパートによっては意味するところが違うかもしれまえん。

イベントという動的なものに注目することで、サイロ間の境界を明確にします。

---

@snap[north-west text-gray span-100]
@size[1.5em](ドメインイベントの議論)
@snapend

@snap[west span-50]
@img[domain_events_problems](assets/img/domain_events_problems.jpg)
@snapend

@snap[east span-50]
@ul[](false)
* 知っていること
* 矛盾点/問題点
* サイロを越えて議論
@ulend
@snapend

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

ひととおり Domain Event が出つくしたら、全体を俯瞰し、様々な議論を行います。

たとえばあるマーケターは、新規登録数が減っていることを問題視するかもしれませんし、
プランナーは予算の関係で戦士以外のキャラクターを用意することができない、ということが問題だと思っているかもしれません。
既存ユーザーのアンケート結果からは対戦に差別化要素がなく、戦略性もあまりないという声も挙げられており、飽きられてきているのではという課題も挙げられました。

その他、ここでは載せていない関係各所間の認識の不一致なども出てきたことでしょう。うちのサイロでは問題ない、というやつです。

---?image=assets/img/Big_Picture_conference_scenario.jpg

@snap[south-east template-note]
@box[text-white rounded bg-orange box-padding text-05](Source: [Introducing EventStorming](https://leanpub.com/introducing_eventstorming))
@snapend

Note:

時間の関係で省略しますが、その後イベントの並び替えや大きな塊の抽出、人と外部システムとの関係なども追加し、全体のフローとして整合性の取れる形を目指します。

Event Storming について詳細を知りたい方は、考案者の Alberto Brandolini 氏(@ziobrando) の著作である Introducing EventStorming を参照してみてください。  

---

@snap[north-west text-gray span-100]
@size[1.5em](焦点をあてる問題を選ぶ)
@snapend

@snap[west span-50]
@img[focus_problem](assets/img/focus_problem.jpg)
@snapend

@snap[east span-50]
新機能: キャラクターに武器を装備できるようにする
@snapend

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

最後にいま取り組むべき問題を決定します。

ここでは他プレイヤーとの差別化、対戦時の戦略性アップについて取り組むことになりました。機能としては、キャラクターに武器を装備できるようにしようというものです。