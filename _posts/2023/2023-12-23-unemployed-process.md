---
title: "転職時に無職になる手続きをインターネットでする"
author: azu
layout: post
date : 2023-12-23T10:47
category: 雑記
tags:
    - 仕事

---

転職する際に空白期間がない場合は会社側で手続きがほとんど終わりますが、一旦無職を挟んで転職する場合は、保険証、年金、iDeCoなどの手続きが必要になります。

今回の[転職活動](https://efcl.info/2023/12/19/open-job-letter/)にあたって、
この手続きをするのに市役所やプリンターを使いたくないので、インターネットだけで完結できるかにトライしました。

- [Open Job Letterを公開しました | Web Scratch](https://efcl.info/2023/12/19/open-job-letter/)

## 手続きの前提条件

会社や個人によって前提が異なるので、ここでは次の前提で記事を書いています。

- 会社員: 厚生年金
- 保険証: [［ITS］関東ITソフトウェア健康保険組合](https://www.its-kenpo.or.jp/)
- iDeCo: 個人型確定拠出年金

退職して次の会社が決まってない場合は、保険証や年金などの手続きが必要になります。

- 保険証: 国民健康保険か任意継続かは好きな方で
    - 今回は任意継続を選択
- 年金: 厚生年金から国民年金第1号へ切り替える
- iDeCo: 「加入者被保険者種別変更届」を提出する

これらをできるだけ、ネットだけで完結させることを目指します。

## 必要なもの

- マイナンバーカード
    - 基礎年金番号 はマイナンバーカードがあればわかる
    - 元々持っていた保険証の記号と番号 はマイナンバーカードがあればわかる

## 手続きの概要

大まかな手続きをまとめたものは次のようになっています。

- ITSの任意継続
    - いつから: 退職日の翌日
    - 期限: 20日以内
    - 方法: [Webゆうびん｜日本郵便](https://webyubin.jpi.post.japanpost.jp/webyubin/snt/DYFR900.do)を使う
    - 取得にかかる日付: 2週間ぐらい
    - 必要なもの: 元々持っていた保険証の**記号**と**番号**
        - マイナポータルで”健康保険情報”をみることでも、保険証の**記号**と**番号**を確認できる
    - 取得できるもの: 任意継続の健康保険証
    - 参考: 
        - 任意継続の健康保険証[任意継続被保険者について | ［ITS］関東ITソフトウェア健康保険組合](https://www.its-kenpo.or.jp/hoken/nini/how_to.html)
- 厚生年金から国民年金へ切り替え
    - いつから: 退職した日
    - 期限: 14日以内
    - 取得にかかる日付: 3営業日ぐらい
    - 方法:  マイナンバーポータルから申請
        - https://www.nenkin.go.jp/denshibenri_kojin/denshibenri_kojin/shinsei_kojin.files/denshi.pdf
    - 必要なもの: マイナンバーカード
    - 申請ファイル: フォーム入力
    - 取得できるもの: 特になし(ステータスはマイナポータルで確認できる)
    - 参考: 
        - [転職・退職したときの手続き｜日本年金機構](https://www.nenkin.go.jp/service/kokunen/kanyu/20121003.html)
        - [会社を退職した時の国民年金の手続き｜日本年金機構](https://www.nenkin.go.jp/service/kokunen/kanyu/20140710-03.html)
        - [電子申請（マイナポータル）｜日本年金機構](https://www.nenkin.go.jp/denshibenri_kojin/denshibenri_kojin/mynaportal.html)
        - [退職したら国民年金への切り替えが必要！ 手続きをまとめてみた | みらいのねだん | JA共済](https://nedan.ja-kyosai.or.jp/column/20181211_aged_no20.html)
- iDeCoの加入者被保険者種別変更
    - いつから: 退職した日
    - 期限: 半年
    - 取得にかかる日付: 1週間ぐらい
    - 必要なもの: 基礎年金番号
        - マイナポータル → [ねんきんネット](https://www.nenkin.go.jp/n_net/)で**基礎年金番号**を確認できる
    - 方法: それぞれのiDeCoのサイト経由で変更の依頼をする
        - SBI: [【iDeCo】iDeCo加入中に退職しました。具体的な手続きの方法について教えてください。 : SBI証券](https://faq.sbisec.co.jp/answer/61ae91a0d30554001c752bfa)
    - 申請ファイル: 郵送されててくる書類
    - 取得できるもの: 特になし(ステータスはiDeCoのサイトで確認できる)

具体的にそれぞれでどうやったかの手順を書いていきます。

### ITSの健康保険の任意継続

健康保険は、[［ITS］関東ITソフトウェア健康保険組合](https://www.its-kenpo.or.jp/)の保険証を任意継続できます(最長 2年できる)。国民健康保険への切り替えもできますが、自分はやったことがないです。

1. [任意継続被保険者資格取得申請書](https://www.its-kenpo.or.jp/hoken/nini/how_to.html)をダウンロードする
2. ダウンロードした申請書に記入する
    - PDFへの入力が安定してる[Adobe Acrobat Reader](https://www.adobe.com/jp/acrobat/pdf-reader.html)を使う
    
    ```markdown
    brew install --cask adobe-acrobat-reader
    ```
    
    - 入力後はPDFとして保存
    - [Preview.app](http://Preview.app) などで印刷 → 1ページ目だけをPDFとして保存しなおす
    
    <aside>
    ⚠️ PDFのサイズは1MB以下とする必要があります
    </aside>
    
3. [Webゆうびん｜日本郵便](https://webyubin.jpi.post.japanpost.jp/webyubin/snt/DYFR900.do)にアクセス
    - 99円でウェブからPDFを郵送できるサービス
4. WebゆうびんでWebレターを開く
      - 特に色はいらないので白黒でOK
5. 作成したPDFをアップロードして、文字化けしてないことをプレビューで確認
6. 宛先を入力して、送付する

あとは、納付書が郵送で来るので、支払うだけ。

**支払い方法**

1. 納付書と保険証が届くのを待つ
2. 納付書に書かれている方法で支払う
    - デフォルトだとインターネットバンキングに振り込む形になっている

**ITSの任意継続のメモ**

- [Webゆうびん](https://webyubin.jpi.post.japanpost.jp/webyubin/snt/DYFR900.do)を使うことでプリンタを使ったり、郵便局に行ったりしなくて良くなります
- 申請から納付書と保険証が届くのに2週間ぐらいかかった
    - 退職日から20日以内に届出完了しないといけないので、毎回不安になる
- 任意継続の納付はインターネットバンキングでできる
    - ⚠️この時に `保険証の番号 苗字 名前` という名義で振り込む必要がある
    - これを忘れた時は電話で確認する必要があったので注意

これで最長2年間は健康保険証が任意継続できるようになります

### 厚生年金から国民年金へ切り替え

厚生年金から国民年金へ切り替えは、マイナポータルから申請できます。

⚠️ [動作環境について ｜ マイナポータル](https://img.myna.go.jp/html/dousakankyou.html)
PCのブラウザでも動作するが、結構マイナンバーカードを読み取るのでスマホでやった方が楽でした。
住所なども情報もマイナンバーカードから読み取れるので選択していくだけです。

1. [マイナポータル](https://myna.go.jp/)にログインする
2. 年金 → 年金関係 → 国民年金に加入する方・加入中の方の手続き
3. 国民年金への加入 → 資格取得(種別変更)届
4. 資格取得(種別変更)届の内容を入力する
    - マイナンバーから読み取れば、ほとんど入力する項目はない(電話番号とかぐらい)
    - 資格取得届の該当年月日は**退職日の翌日**の日にちを入力する
        - [会社を退職した時の国民年金の手続き｜日本年金機構](https://www.nenkin.go.jp/service/kokunen/kanyu/20140710-03.html)
5. 申請する

あとは、年金の納付書が郵送で来るので、支払うだけです。

**支払い方法**

1. 国民年金保険料納付書 という書類が届く
2. 領収（納付受託）済通知書 に書かれている方法で支払う
    - Pay-easy か バーコードをPayPayなどで読み込んで支払う

### iDeCoの加入者被保険者種別変更

厚生年金から国民年金に変わった場合、無職の場合は[第1号被保険者](https://www.nenkin.go.jp/service/yougo/tagyo/dai1hihokensha.html)になります。この被保険者種別によって、iDeCoの掛金の上限が異なるため、種別の変更が必要になります。

⚠️ 各自が加入している、iDeCoのサイトによって変更方法が異なる。ここでは[SBIの個人型確定拠出年金：iDeCo](https://go.sbisec.co.jp/prd/ideco/ideco_top.html)を使ってるケースを書いています。

SBIの場合

1. [【iDeCo】iDeCo加入中に退職しました。具体的な手続きの方法について教えてください。 : SBI証券](https://faq.sbisec.co.jp/answer/61ae91a0d30554001c752bfa)から選択して申請画面にいく
2. 基礎年金番号や住所を入力して申請
    - 基礎年金番号は[マイナポータル](https://myna.go.jp/) → [ねんきんネット](https://www.nenkin.go.jp/n_net/)で確認できる
3. 郵送された書類に入力して返送
    - 切手とかは特にいらないのでポストに入れるだけ

**郵送される書類のメモ**

会社員から無職の場合は、第2号被保険者から第1号被保険者(or 第3号の扶養)になる。
上限金額が異なるので、掛金を変更する場合は、書類の金額も変更する。

- 2 → 1 への変更
- 23000円: 会社員における個人型確定拠出年金と同じにする場合は23000円のままにする

参考: 

- https://www.ideco-koushiki.jp/library/pdf/simplified_chart.pdf
- [年金の被保険者には、1号・2号・3号の種類がありますが、どう違うのです|よくある質問|吉岡町ホームページ](https://www.town.yoshioka.gunma.jp/faq/hoken-nenkin/kokumin-nenkin/001905.html)
- [失業したらiDeCoの落とし穴にハマった件 - 35歳からの中二病エンジニア](https://aikawame.hateblo.jp/entry/2021/10/29/%E5%A4%B1%E6%A5%AD%E3%81%97%E3%81%9F%E3%82%89iDeCo%E3%81%AE%E8%90%BD%E3%81%A8%E3%81%97%E7%A9%B4%E3%81%AB%E3%83%8F%E3%83%9E%E3%81%A3%E3%81%9F%E4%BB%B6)
    - 国民年金の納付免除などを申請している場合はiDeCoの手続きは異なります
    - 自分は特にこういった免除の申請はしてないです

## まとめ

[![退職後の手続きの流れ](https://mermaid.ink/img/pako:eNqVk8tOwkAUhl-FzFpfgIUb2ZhoYtTETTfVjkqUS2pZGGPS6YgCileMAkGUQCSSiAREFJSHOUwvb-HYQaMYb00XzZ___2bO6TnraD6kYORFi6ocXvKMT0lBD38cXbfJvWd4eMQzNjMN5KbXbpub-1YzbTUzwvJVd-2zeA6MLSD8rYNxDOTKStXtyo6ZbTmX52AcOfTO0Ylg_NHsgnvdnJN-tMsdIGWrkey1z7gLyC6rJYDsC97PHhcDNAmUAo0BPQDKT63yU81U1Yyf8Ht8qZ7tZaxUnj00nO1DIDtgxFn2ybw9fFNaLLYNRsLMdoHERPpfkf6deK1FoAmgOaAdMLpAKyIq-iHAv9pc2He9-Uc_noHkgVQn5TWr3gajAPQU6PWPffL78GiIzwNLXLBoydajdqHS_xt61CrfsFiJFeNmtiGSf7a78PdxeB2NmiEQA-JH4-fCB8QBYjfFBwwNoQBWA7Jf4Zuw_hqTkLaEA1hCXv6pyOqyhKTgBvfJES00vRacR15NjeAhFAkrsoZ9fpkvUAB5F-SVVa5ixa-F1AmxWu6GbbwAQaDpwg?type=png)](https://mermaid.live/edit#pako:eNqVk8tOwkAUhl-FzFpfgIUb2ZhoYtTETTfVjkqUS2pZGGPS6YgCileMAkGUQCSSiAREFJSHOUwvb-HYQaMYb00XzZ___2bO6TnraD6kYORFi6ocXvKMT0lBD38cXbfJvWd4eMQzNjMN5KbXbpub-1YzbTUzwvJVd-2zeA6MLSD8rYNxDOTKStXtyo6ZbTmX52AcOfTO0Ylg_NHsgnvdnJN-tMsdIGWrkey1z7gLyC6rJYDsC97PHhcDNAmUAo0BPQDKT63yU81U1Yyf8Ht8qZ7tZaxUnj00nO1DIDtgxFn2ybw9fFNaLLYNRsLMdoHERPpfkf6deK1FoAmgOaAdMLpAKyIq-iHAv9pc2He9-Uc_noHkgVQn5TWr3gajAPQU6PWPffL78GiIzwNLXLBoydajdqHS_xt61CrfsFiJFeNmtiGSf7a78PdxeB2NmiEQA-JH4-fCB8QBYjfFBwwNoQBWA7Jf4Zuw_hqTkLaEA1hCXv6pyOqyhKTgBvfJES00vRacR15NjeAhFAkrsoZ9fpkvUAB5F-SVVa5ixa-F1AmxWu6GbbwAQaDpwg)

- マイナンバーカードがあれば、保険証や基礎年金番号などの情報がわかるので便利です
    - [ねんきんネット](https://www.nenkin.go.jp/n_net/)では、今までの年金をどう収めたかの情報も全部見れます
- [Webゆうびん｜日本郵便](https://webyubin.jpi.post.japanpost.jp/webyubin/snt/DYFR900.do)を使うことで、プリンタが必要なく郵便局に行かなくても、任意継続の手続きが進められたので便利でした
    - コンビニでプリントアウトするのがめちゃくちゃ面倒なので、これが解決できるのが良かった
    - [ネットdeゆうびん](https://about.yubin.app/)も似たサービスです
- 厚生年金から国民年金へ切り替えは、[マイナポータル](https://myna.go.jp/)だけで完結してたので一番スムーズ
- iDeCoの加入者被保険者種別変更は、郵送されてきた書類を手書きしてポストに出す必要があったので、一番手間がかかった
    - 特に本人確認のコピーとか印鑑は不要で、書類を書くだけなので許容範囲

結論的には、iDeCo以外は家の中で手続きが完結できたので、だいぶ楽になったなと思いました。
