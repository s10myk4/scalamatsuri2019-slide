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

* 各自 意欲を持って問題の解決を図ろうとするものの...
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

https://www.eventstorming.com/

* 複雑なビジネスドメインに対して 共同 で 探究 を行うためのワークショップの 1 形式
    * 共同: 主要なステークホルダーが一堂に会する
    * 探究: 物事を理解しようとする取り組み

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

* システムを全体を把握する
* 解決すべき問題を発見する
* いま利用できる最良の情報を集める
* ベストと思われる開始地点から解決策を実行する

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

@snap[west]
<img alt="domain_events" src="assets/img/domain_events.jpg" width="25%"/>
@snapend

@snap[east]
* オレンジ色の付箋
* 動詞の過去形
* ドメインエキスパートに関係あること
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

---

@snap[north-west text-gray span-100]
@size[1.5em](議論)
@snapend

@snap[west]
<img alt="domain_events_problems" src="assets/img/domain_events_problems.jpg" width="25%"/>
@snapend

@snap[east]
ドメインについて知っていることや矛盾点、問題点をサイロを越えて議論
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

---

@snap[north-west text-gray span-100]
@size[1.5em](大局的な図の完成)
@snapend

@snap[west]
<img alt="Big_Picture_conference_scenario" src="assets/img/Big_Picture_conference_scenario.jpg" width="25%"/>
@snapend

@snap[east]
> 出典: [Introducing EventStorming](https://leanpub.com/introducing_eventstorming)
@snapend

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

時間の関係で省略しますが、その後イベントの並び替えや大きな塊の抽出、人と外部システムとの関係なども追加し、全体のフローとして整合性の取れる形を目指します。

---

@snap[north-west text-gray span-100]
@size[1.5em](焦点をあてる問題を選ぶ)
@snapend

@snap[west]
<img alt="focus_problem" src="assets/img/focus_problem.jpg" width="25%"/>
@snapend

@snap[east]
新機能: キャラクターに武器を装備できるようにする
@snapend

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

最後にいま取り組むべき問題を決定します。

ここでは他プレイヤーとの差別化、対戦時の戦略性アップについて取り組むことになりました。機能としては、キャラクターに武器を装備できるようにしようというものです。