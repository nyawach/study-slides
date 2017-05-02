# Grid Layout Module

## :princess: 

---

アジェンダ

- Grid Layout Module #とは
- とりあえずみてみる
- よく使うプロパティ
- 調べた/使った所感
- 検証メモ

---

# Grid Layout Module#とは

---

## Grid Layout Module

[CSS Grid Layout Module Level 1](https://www.w3.org/TR/2016/CR-css-grid-1-20160929/)

`display: grid` からなる、グリッドレイアウトに特化したプロパティ群。

---

## 何ができる？

グリッドレイアウト。

ブログとかでよく見る系のレイアウト。

https://www.kayac.com/

---

```jade
.top_news__arranged--left
  .top_news__inner
  //- ...
.top_news__arranged--right
  .top_news__inner
  //- ...
```
↓

```
.top_news__arranged
  .top_news__inner--l
  .top_news__inner
  //- ...
```

みたいに書ける可能性も！？

---

## flexboxとどう違うの？

- flexbox: 単一方向(横向き or 縦向きどちらか一方のみ)
- Grid Layout: 縦横方向(row/column)

---

## とりあえずみてみる

- [Code Pen](http://codepen.io/be-into/pen/RVPERo)
- [Code Pen(他人の)](http://codepen.io/stacy/pen/ObmjeZ)

---

# プロパティ

---

## 親要素につける

- `display: grid`
- `grid-template-(rows/columns)`: レイアウトのサイズ指定
- `grid-(row/column)-gap (or grid-gap)`: 余白
- `grid-template-areas`: 名前空間の指定(**ヤバイ**)
- `grid-auto-flow`: 自動配列の向き指定
- `grid-auto-(rows/columns)`: はみ出た要素のサイズ指定

---

## 子要素につける

- `grid-(row/column)`
- `grid-area`(**ヤバイ**)

---

## `grid-template-areas` / `grid-area`

[Code Pen](http://codepen.io/be-into/pen/RVPERo)

```
.grid-container {
  grid-template-row: 100px 1fr;
  grid-template-column: 1fr 100px;
  grid-template-area:
    "header icon"
    "main main"
    "footer footer";
}
header { grid-area: header; }
main { grid-area: main; }
i.icon { grid-area: icon; }
footer { grid-area: footer; }
```

---

## `grid-template-areas` / `grid-area` の何がヤバイか

メディアクエリでボックスの名前空間さえいじれば、置き換えてくれる
(前のスライドのマークアップのは↓だけでいい感じに配置してくれる)

```pug
.grid-container
  header
  main
  i.icon
  footer
```

---

# 調べた/使った所感

---

## テーブルレイアウトっぽい

IE6/HTMLメールの遺産  
同じ感覚で組むと割りと簡単に理解できそう  
(colとかrowとか使ってるからってるから？)  
(そもそもテーブルレイアウトを知ってる人前提ではある)

---

## 良いところ

- DOMの入れ子がスマホのみ対応レベルに浅くなる
- SPAとの相性が良い(DOMの構造を大幅に変えなくて良いため)
- 情報量の多い・変化しやすいサイト(ギャラリーサイトとか)でGrid Layoutする時などは便利

---

### DOMの入れ子がスマホのみ対応レベルに浅くなる

- SPのマークアップ/CSS作成
- PCをgrid layoutでデザイン反映

すると、タグのネストが最低限で済みそう

---

## 懸念点など

- 検索エンジンのインデックスのレンダリングで表示されてない...？
- 最新ブラウザしか使えない(アップデートされてないchromeとかでも無理)
- アニメーションに向いてない(切り替わり時に〜とかできない)
- IE/Edgeの対応
- Masonry Grid Layoutは無理そう
- 親要素が子要素のwidth/height情報を保持するのがなんとも言えない気持ちになる

---

## 実際に使えるのか？

[Can I Use...](http://caniuse.com/#feat=css-grid)

先週Chrome/FireFox/Safariが対応した(なおIE/Edge(ry  
スマホは全滅 :innocent:   
=> そもそもスマホでGrid Layoutすることないので無問題

---

## IE/ Edgeはどうすれば...

一昔前の仕様で実装自体はされている(IE11までならなんとかなる)

=> mixinを作ってラッピングする必要あり

[CSS Grid Layoutをガッツリ使った所感](http://qiita.com/clockmaker/items/2a6ba69ef6e452844adf#%E3%83%99%E3%83%B3%E3%83%80%E3%83%BC%E3%83%97%E3%83%AC%E3%83%95%E3%82%A3%E3%83%83%E3%82%AF%E3%82%B9%E3%82%92%E4%BB%98%E3%81%91%E3%82%8B%E3%81%A0%E3%81%91%E3%81%A7%E3%81%AF%E5%8D%81%E5%88%86%E3%81%A8%E3%81%AF%E8%A8%80%E3%81%88%E3%81%AA%E3%81%84)

---

### 検証メモ

---

### 親要素

CSS-Animationで `grid-template-area` はダメそう
transtionもダメそう

---

#### 子要素

(min-/max-)width指定 => 効く(margin指定すれば中央配置などに出来るが、親要素の指定幅よりはみ出る場合は左揃え
`margin: auto` で、指定幅内で天地中央(子要素にwidth/height指定いらない)

---

## 使ってる事例

- [Beautifl - Flash Gallery](http://beautifl.net/)

これを検証すれば対応端末把握できるはず(時間なかったため未検証)

---

## 参考

- [CSS Grid Layout を極める！（基礎編）](http://qiita.com/kura07/items/e633b35e33e43240d363)
- [CSS Grid Layout を極める！（場面別編）](http://qiita.com/kura07/items/486c19045aab8090d6d9)
- [これからのグリッドレイアウト -  Grid Layout Moduleの概要 | CodeGrid](https://app.codegrid.net/entry/display-grid-1)
- [CSS Grid Layoutをガッツリ使った所感](http://qiita.com/clockmaker/items/2a6ba69ef6e452844adf)

---

# Fin.

## :princess:
