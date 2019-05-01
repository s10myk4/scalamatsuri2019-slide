## ビジネス背景の理解と要求の整理

---

@snap[north-west text-gray span-100]
@size[1.5em](ビジネス背景の理解と要求の整理)
@snapend

@snap[west]
@ul[](false)
* 目的
    * 環境の把握
    * 取り組むべき要求の整理
    * 組織の自己組織化 
* アウトプット: 課題/要求の共有
    * 主要なステークホルダー**全員**!
@ulend
@snapend

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

まず最初のステップとして、ビジネス背景の理解と要求の整理を行います。

ステークホルダー全員がプロダクトを取り巻く環境全体を把握し、その中でも今取り組むべき問題について合意形成を図ります。

サイロ内での局所的な解決策ではなく全体として取り組むべき問題が共有されることで、組織全体の自己組織化が促されることを期待します。

---

@snap[north-west text-gray span-100]
@size[1.5em](Event Storming)
@snapend

@snap[west]
`https://www.eventstorming.com/`
<hr/>
@ul[](false)
* 複雑なビジネスドメインに対して、
  `共同`で`探究`を行うためのワークショップ
    * 共同: 主要なステークホルダーが一堂に会する
    * 探究: 物事を理解しようとする取り組み
@ulend
@snapend

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

@snap[west]
@ul[](false)
* システム全体の把握
* 解決すべき問題を発見
* いま利用できる最良の情報を収集
* ベストと思われる開始地点から解決策を実行
@ulend
@snapend

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

Event Storming では、各参加者の専門性・立場を越えて以下について探究します。 

* システム全体の把握
* 解決すべき問題を発見
* いま利用できる最良の情報を収集
* ベストと思われる開始地点から解決策を実行 

---

@snap[north-west text-gray span-100]
@size[1.5em](Event Storming の大まかな流れ)
@snapend

@snap[west]
@ol[](false)
* Domain Event の書き出し
* Domain Event の議論
* 焦点をあてる問題を選ぶ
@olend
@snapend

@snap[south-west template-note text-gray]
TODO: 英語字幕
@snapend

Note:

Event Storming の大まかな流れは以下です。

* Domain Event の書き出し
* Domain Event の議論
* 焦点をあてる問題を選ぶ

実際には Domain Event の整理や Event が実行されるトリガの追加といったことも行いますが、本セッションではこちらのみ紹介します。

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
たとえば発注という名詞が出てきたとして、販売や梱包、配達や請求といったそれぞれの分野、つまりコンテキストを持つドメインエキスパートによっては意味するところが違うかもしれまえん。

イベントという動的なものに注目することで、サイロ間の境界を明確にします。

また、時系列に沿って Event を洗い出すことでビジネス全体の流れを表現することを強制します。

---

@snap[north-west text-gray span-100]
@size[1.5em](Domain Event の議論)
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
扱うテーマにもよりますが、最終的にはこのように巨大なものとなります。

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
