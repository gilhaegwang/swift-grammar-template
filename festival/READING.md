# 読解ノート：自分のアプリのコード3箇所

> **氏名：** 26cm0109 キルへカン  
> **AI利用レベル：** 1（質問のみ。説明は自分の言葉で書く）  
> **最終更新：** 2026-10-26

生成AIに書いてもらったコードでも、自分で書いたコードでもかまいません。自分のアプリの中から3箇所を選んで、次の3つの問いに答えてください。分からないところは、生成AIに「この行は何をしていますか」「この行を消すとどうなりますか」と聞いてかまいません。ただし、書くのは自分の言葉です。

> 💡 担任の先生の指示で、項目が増えることがあります。

---

## 箇所1：名前が空欄のときの処理

（例：品名を追加するボタンの処理）

```swift
// 該当部分のコードを抜粋して貼る（5〜15行くらい）
// RankingStore.swift
static func normalizedName(_ rawName: String) -> String {
    let cleaned = rawName.components(separatedBy: .controlCharacters)
        .joined()
        .trimmingCharacters(in: .whitespacesAndNewlines)
    return cleaned.isEmpty ? "ななし" : cleaned
}
```

**何をしているか：**
（この部分が果たしている役割を説明する）

名前の前後の空白や改行を取り除きます。空欄なら「ななし」にします。

**なぜこう書くのか：**
（別の書き方ではなく、この書き方が選ばれている理由を説明する。分からなければ「分からない」と書いてよい）

何も入力しなくても、ランキングに名前を表示できるようにするためです。

**もしこう書かなかったら：**
（この部分を消したり変えたりすると何が起きるか。実際に試した結果があればここに書く）

最後を `return cleaned` に変えると、空欄でも「ななし」になりません。

---

## 箇所2：ランキングを並べる処理

```swift
// コードを貼る
// RankingStore.swift
private static func sorted(_ values: [RankingEntry]) -> [RankingEntry] {
    values.sorted {
        if $0.scoreCentiseconds != $1.scoreCentiseconds {
            return $0.scoreCentiseconds > $1.scoreCentiseconds
        }
        return $0.registrationOrder < $1.registrationOrder
    }
}
```

**何をしているか：**

生存時間が長い順に並べます。同じ時間なら、先に登録した記録を上にします。

**なぜこう書くのか：**

長く生き残った人を上位にするためです。`$0` と `$1` は、比べる2件の記録です。

**もしこう書かなかったら：**

時間を比べる `>` を `<` に変えると、短い記録が上になります。

---

## 箇所3：入力した名前を消す処理

```swift
// コードを貼る
// ResultView.swift
private func resetInput() {
    nameIsFocused = false
    draftName = ""
    feedback = nil
    showSaveError = false
    pendingDestination = .retry
}
```

**何をしているか：**

入力した名前や案内文を消し、キーボードを閉じます。

**なぜこう書くのか：**

次の人が遊ぶときに、前の人の名前を残さないためです。

**もしこう書かなかったら：**

`draftName = ""` を消すと、この関数を呼んでも名前が変数に残ります。

---

## 自分ではまだ説明できない箇所（正直に）

（アプリの中で「動いているけれど何をしているか分からない」ところを書く。ここに書いたものは、後期の文法の授業で分かるようになります。）

JSONEncoderとJSONDecoderを使う、記録の保存と読み込みの部分がまだ難しいです。
sortedの中の `$0` と `$1` の書き方も、自分ではうまく説明できません。
