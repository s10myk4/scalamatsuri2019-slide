### 要求

---

![development-flow](assets/img/developmemt-flow.png)

---

@snap[north-west text-gray span-100]
@size[1.5em](要求)
@snapend

#### ここでも問題領域

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](要求)
@snapend

#### ここでも期待する成果物

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](想定する要求:ユースケース)
@snapend

### ユーザーは、戦士の装備する武器を変更することができる

@snap[south-west template-note text-gray]
Customer requested specification  

The user can change the weapon equipped to the warrior.
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](要求)
@snapend

#### どんな手法を用いて要求を明確化するかを示す
#### 顧客の要求を具体的に理解していく流れを例を用いて段階的に示す

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](要求)
@snapend

#### 私たちが機能を実装する上で考えていることは？

- その機能が実現したいこと
- ユーザーがどのようにその機能を利用するのか
- どのように作るか

etc...

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](要求)
@snapend

#### どのように作るかを概念的に整理する

#### ->振る舞い要求を明らかにする

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](要求)
@snapend

### ユースケース記述

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](ユースケース記述とは)
@snapend

### TODO

@snap[south-west template-note text-gray]
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](ユースケース記述)
@snapend

#### 正常系(晴れの日コース)
```
ユーザーは、自分の所有している戦士の中から1人を選んでタップする
システムは、戦士の詳細画面を表示する
ユーザーは、武器装備ボタンをタップする
システムは、武器一覧を取得し、武器装備画面に表示する
ユーザーは、戦士に装備したい武器を一覧から選択して、タップする
システムは、選択された武器が装備するために必要なレベル以上であるかを確認する
システムは、選択した武器の属性が戦士の属性が同じであるかを確認する
システムは、選択した武器を装備した戦士を作成し、保存する
システムは、"武器を変更しました"というメッセージを画面に表示する
```

@snap[south-west template-note text-gray]
UseCase Description NormalCase
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](ユースケース記述)
@snapend

#### 異常系(雨の日コース)
```
・戦士のレベルが選択した武器を装備するのに必要なレベルを未満である場合
システムは、"この武器を装備するための条件を満たしていません"というメッセージを武器装備画面に表示する

・戦士の属性と選択した武器の属性が異なる場合
システムは、"この武器を装備するための条件を満たしていません"というメッセージを武器装備画面に表示する

・指定した戦士が存在しない場合
システムは、"対象の戦士が存在しません"というメッセージを戦士一覧画面に表示する
```

@snap[south-west template-note text-gray]
UseCase Description AbnormalCase
@snapend

---

@snap[north-west text-gray span-100]
@size[1.5em](UseCase Description)
@snapend

#### NormalCase
```
The user taps one person out of warriors owned by the user.
The system displays the detailed screen of the warrior.
The user taps the weapon equipped button.
The system acquires the weapon list and displays it on the weapon equipment screen.
The user selects a weapon that he wants to equip warrior from the list and taps.
The system checks whether the selected weapon is at or above the level required for equipping it.
The system checks whether the attribute of the selected weapon is the same as the attribute of the warrior.
The system creates and stores warriors equipped with selected weapons.
The system displays a message "weapon changed" on the screen.
```
---

@snap[north-west text-gray span-100]
@size[1.5em](UseCase Description)
@snapend

#### AbnormalCase
  
```
・If the level of the warrior is less than the level required to equip the selected weapon
The system displays a message "We do not meet the conditions for equipping this weapon" on the weapon equipment screen.

・When the attribute of the warrior and the attribute of the selected weapon are different
The system displays a message "We do not meet the conditions for equipping this weapon" on the weapon equipment screen.

・If the designated warrior does not exist
The system displays a message "target warrior does not exist" on the warrior list screen.
```

---

@snap[north-west text-gray span-100]
@size[1.5em](ユースケース記述のポイント)
@snapend

### 簡単にユースケース記述する上でのポイントを説明する
### 詳細は本か私のスライドを参照してねw

@snap[south-west template-note text-gray]
@snapend

