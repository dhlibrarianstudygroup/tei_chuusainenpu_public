# 抽斎年譜マークアップ方針

### 文構造
!["サンプル"](pictures/chusai_sample.PNG)

- 年次ごとに`<div>`で囲む
- 各ブロック冒頭の年代と抽斎の年齢は`<head>`で囲む。その際、インデントの升目の数をstyle属性に記述する。つまり、年代は3マス行頭が下がっているので、`<head style="#m3">`とマークアップする。
- 黒の本文は`<p>`で囲む。その際、インデントの升目の数を`@style`に記述する。例えば2マス行頭が下がっているようであれば、`<p style="#m2">`とマークアップする。
改行位置には空タグ`<lb/>`を入れる。
- 朱筆の注記は`<note>`で囲む。`@style`には朱筆であることを示す`color:red`とインデントの情報をセミコロン（;）で繋いで記述する。例えば、インデントが10マスある朱筆は`<note style="color:red;#m10”>`のようになる。
改行位置には空タグ`<lb/>`を入れる。
- 注が欄外にある場合はその位置情報と、注が指す本文を参照させる。具体的には`<note style="color:red" place="top" corresp="#note03_01">`のように、位置情報を`@place`に記述。対応する本文は`<p @xml:id="noteコマ番号_連番">`などとして、注は`@corresp`で参照させる。

- `@place`の値について  
TEIガイドラインによると下記の使い分けが推奨されている  
[https://tei-c.org/release/doc/tei-p5-doc/ja/html/ref-att.placement.html](https://tei-c.org/release/doc/tei-p5-doc/ja/html/ref-att.placement.html)

~~~
top：当該頁の上の余白
bottom：当該頁の下の余白
margin：余白（左left, 右right, 左右both）
opposite：反対側，つまり，向かいのページ。
overleaf：当該葉の裏面。
above：当該行の上。
right：右側、つまり、縦書きテキストや図の右側。
below：当該行の下。
left：左側、つまり、縦書きテキストや図の左側。
end：末尾（たとえば章や巻の）。
inline：テキスト本文中
inspace：あらかじめ用意された箇所（たとえば過去の書写者が空けておいたなど）。
~~~



- つまり、文構造としては、下記のようにマークアップできる。

```xml
<div>
    <head style="#m3">年代</head>
    <head>抽斎の年齢</head>
    <note style="color:red;#m10">原稿用紙枠内の朱筆注記</note>
    <p style="#m5" xml:id="note01_01">
       <lb/>本文1行目
       <lb/>本文2行目
       <lb/>本文3行目
    </p>
    <note style="color:red" place="注記の場所" corresp="note01_01">原稿用紙枠外の朱筆注記</note>
</div>
```

### 時間情報

- 見出しや本文中の時間情報は`<date>`でマークアップする。`@when`には西暦に変換した時間情報、`@when-custom`には和暦の時間情報を記述する。その際、算用数字を用いること。
- 和暦を西暦に変換するときに便利：[https://eco.mtk.nao.ac.jp/cgi-bin/koyomi/caldb.cgi](https://eco.mtk.nao.ac.jp/cgi-bin/koyomi/caldb.cgi)
- 例1：安政四年丁巳

```xml
<date when="1857">安政四年丁巳</date>
```

- 例2：（安政四年の）二月十四日

```xml
<date when="1857-03-09" when-custom="安政4年2月14日">二月十四日</date>
```

### 人物情報

- 本文の人物情報は`<persName>`でマークアップする。`<back>`,`<listPerson>`に人物情報のリストを作成し、本文中の`<persName>`の`@corresp`で対応させる。
- 人物情報のリストでは、下記の情報も記述できる。
~~~
<surname>：姓
<forename>：名
<idno type="VIAF">：VIAFなどのID
<birth>：生年月日
<death>：死亡年月日
<note>：注記
~~~
- 例：

```xml
<body>
	<p>
	<persName corresp="#渋江抽斎">抽斎</persName>はハ下戸にて、酒一滴をも口に執らざりしが、
	</p>
</body>
<back>
    <listPerson>
        <person xml:id="渋江抽斎">
            <persName>
                <surname>渋江</surname><forename>抽斎</forename>
            </persName>
            <idno type="VIAF">https://viaf.org/viaf/18576420</idno>
            <birth>文化2年11月8日</birth>
            <death>安政5年8月29日</death>
        </person>
```

### 割注
- `<note>`で囲み、`@rend="割注"`とする。割注内の改行箇所には空タグ`<milestone unit="wbr"/>`を入れる。

- 例：
```xml
<note rend="割注">翌年君公帰府<milestone unit="wbr"/>せず翌々年に</note>
<note rend="割注">至りて始めて<milestone unit="wbr"/>帰府せる也</note>
と為りて寒氣に堪えざりし
```

### ルビ
- ルビが付されている文字を`<rb>`、ルビを`<rt>`でマークアップし、それをさらに`<ruby>`で囲む。
- 例：
```xml
<persName corresp="#渋江保"><ruby><rb>成善</rb><rt>しげよし</rt></ruby>(保)</persName>生る、
```

### 注釈の対応
本文は`@xml:id="noteコマ数_連番"`、注は`<note corresp="noteコマ数_連番">`とし、両者を対応させる。
- 例：
```xml   
<lb/><p xml:id="note05_01" style="#m1">七月二十八日、父允成、藩主より一粒金丹製法を傳授せ
<lb/>らる、</p>

<note style="color:red" corresp="#note05_01" place="margin">是れハ、
<lb/>文化十一
<lb/>年の
<lb/>部に入るべきを、誤て此処へ入れたり</note>
```

### 『渋江抽斎』との対応
- 抽斎年譜のTEI/XMLファイルの方に開始箇所に`<anchor xml:id="cnコマ数_連番_s"/>`、終了箇所に`<anchor xml:id="cnコマ数_連番_e"/>`の空タグを入れる。青空文庫の『渋江抽斎』テキストファイルでは`<anchor corresp="#cnコマ数_連番_s"/>`、<anchor corresp="#cnコマ数_連番_e"/>とし、`@corresp`で参照させる。
  （注釈のxml:idも同様だが、コマ数ではなく、`<pb>`の番号と一致させた方が作業はしやすかったと思われる）
- 例：  

『抽斎年譜』
```xml
<p style="#m1"><anchor xml:id="cn03_01_s"/>十一月八日、江戸神田辨慶橋に生る時に父允成四十二歳。
<lb/>母<ruby><rb>縫</rb><rt>ヌヒ</rt></ruby>子三十一歳。<anchor xml:id="cn03_01_e"/></p>
```
『渋江抽斎』
```xml
<anchor corresp="#cn03_01_s"/>抽斎は文化二年十一月八日に、神田弁慶橋に生れたと<ruby><rb>保</rb><rt>たもつ</rt></ruby>さんがいう。これは母<ruby><rb>五百</rb><rt>いお</rt></ruby>の話を記憶しているのであろう。父<ruby><rb>允成</rb><rt>ただしげ</rt></ruby>は四十二歳、母<ruby><rb>縫</rb><rt>ぬい</rt></ruby>は三十一歳の時である。<anchor corresp="#cn03_01_e"/>
```
- 『日本近代文学大系 12』（角川書店、1974年）の補註が参考になる。
- NDLデジコレの送信サービスにて閲覧可能（抽斎年譜の補註はは264コマ目〜）。「年譜」で全文検索をかけるとよい。 
https://dl.ndl.go.jp/pid/12503198/1/264

### 森鴎外の書入れ
森鴎外による書入れは`<note>`で囲み、`@hand="#ougai"`とする。
```xml
<lb/>文化九年壬申
<lb/><note hand="#ougai">九月十一日允成母渋江早太祖母死ニ付忌引、十九日忌御免、日記</note>
```
