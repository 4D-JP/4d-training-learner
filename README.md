無料ビギナー・セミナー（東京・大阪）

###シラバス

* オリエンテーション
* カード型データベースを作成する
* ユーザーインタフェースを作成する	
* リレーショナルデータベースを作成する
* コンポーネントを作成する
* ウィジェットを作成する
 
---

###凡例

❓情報

📜リファレンス（ランゲージ/デザイン）

🔷ランゲージ（4D）

🔶ランゲージ（SQL）

----

🎯データベースを作成する

テキストデータをインポートする

* テーブルを作成する
* ヘッダー > 列名をフィールド名にする
* テーブル名: Contact
* エンコーディング: Windows-31J
* 生年月日フィールドを日付型に設定する

テストデータ作成ツール: http://yamagata.int21h.jp/tool/testdata/

---

❓文字コード

🔷[CONVERT FROM TEXT](http://doc.4d.com/4dv15r/help/command/ja/page1011.html)

⚠️15R4, 15R5の64bitsを使用してはいけない（インポート画面が未完成）

---

テーブル名（慣例）

* テーブル名は（ふさわしければ）単数形の名詞にする
* 最初の1文字だけアッパケース（大文字）にする
 
❓命名規則

📜[ランゲージリファレンス > プログラミング言語の構成要素 > 識別子](http://doc.4d.com/4dv15r/Identifiers.300-2936710.ja.html)

❓自由に表示名を設定することができる

🔷[SET TABLE TITLES](http://doc.4d.com/4dv15r/help/command/ja/page601.html)

🔷[SET FIELD TITLES](http://doc.4d.com/4dv15r/help/command/ja/page602.html)

📜[デザインリファレンス > フォームの作成 > スタティックテキスト中で参照を使用する](http://doc.4d.com/4dv15r/Using-references-in-static-text.300-2880256.ja.html)

---

デザインモードでデータを表示する

* 「テーブル」ツールをクリックする
* フォームの右端にスプリッターを配置する（一番最後のフィールド「住所ふりがな」を表示するため）

❓自動フォーム作成オプション

📜[デザインリファレンス > 環境設定 > 一般ページ] (http://doc.4d.com/4dv15r/General-Page.300-2880274.ja.html)

---

ショートカットでセレクション操作

* Control/command/+: すべてのレコードをカレントセレクションに
* Control/command/-: ハイライトされているレコードをカレントセレクションに
* Enter (Function+return): 確定

❓カレントセレクション

📜[デザインリファレンス > レコードの管理 > レコードの表示と選択](http://doc.4d.com/4dv15r/Displaying-and-selecting-records.300-2880415.ja.html)

❓リストフォーム

📜[デザインリファレンス > 一覧フォームとレポート > 一覧フォーム](http://doc.4d.com/4dv15r/Output-forms.300-2964166.ja.html)

---

検索

* 完全一致
* 全角と半角
* ひらがなとカタカナ
* ワイルドカード
* メールアドレスの検索（全角＠）
* 文字列の途中に含まれる＠はワイルドカードとして扱わない

❓フォームによるクエリ

📜[デザインリファレンス > レコードの検索 > フォームによるクエリ](http://doc.4d.com/4dv15r/Query-by-example.300-2964241.ja.html)

❓ワイルドカード

📜[デザインリファレンス > レコードの検索 >クエリエディター](http://doc.4d.com/4dv15r/Query-editor.300-2964242.ja.html)

📜[デザインリファレンス > データベース設定 > データベース/データストレージページ](http://doc.4d.com/4dv15r/DatabaseData-storage-page.300-2880293.ja.html)

---

🎯ユーザーインタフェースを作成する

フォーム

* ``On Load``, ``On Unload``を残してすべてのフォームイベントを無効にする
* Control/command+クリックでトグルできる
* 連結メニューバーを「メニューバー#1」に設定する
* 水平/垂直マージンを``20``にする

リストボックス

* データソース: カレントセレクション
* 変数名を空にする
* オブジェクト名: Contact.List
* 行の高さ: ピクセル -> 行
* イベント: すべて解除（リストボックス/列）
* エリプシスで省略する: 無効（列）
* テンプレートとして使用（リストボックス）
* 選択モード: 単一
* 横方向サイズ変更: 拡大（リストボックス）
* 縦方向サイズ変更: 拡大（リストボックス）
* データソース: [Contact]名前（列）
* タイトル: "名前"（ヘッダー）
* サイズ変更可: 無効（列）

* 2列目を追加する
* データソース: [Contact]名前ふりがな（列）
* タイトル: "ふりがな"（ヘッダー）
* サイズ変更可: 無効（列）

* 1列目の「行フォントカラー」に式を入力する
 
```
Choose([Contact]性別="男";0x003333FF;0x00FF3333)
```

🔷[Choose](http://doc.4d.com/4dv15r/help/command/ja/page955.html)

🔷[Form event](http://doc.4d.com/4dv15r/help/command/ja/page388.html)

---

検索エリア

* テキスト入力
* 変数名を空にする
* オブジェクト名: Input.SearchCriteria
* 横方向サイズ変更: 拡大
* 縦方向サイズ変更: なし
* イベント: ``On Data Change``, ``On Load``
 
```
$event:=Form event

Case of 
: ($event=On Load)

ALL RECORDS([Contact])

: ($event=On Data Change)

$q:=Replace string(Self->;"@";"")

$criteria:="@"+$q+"@"

QUERY([Contact];[Contact]名前=$criteria;*)
QUERY([Contact]; | ;[Contact]名前フリガナ=$criteria)

End case 

GOTO OBJECT(*;OBJECT Get name(Object current))

OBJECT SET ENABLED(*;"Contact.Field.@";Is record loaded([Contact]))
```

---

クエリパス

* テキスト入力
* 変数名を空にする
* オブジェクト名「Message.QueryPath」
* 横方向サイズ変更: 拡大
* 縦方向サイズ変更: 移動
* イベント: すべて解除
* 入力可: 無効
* フォーカス可: 有効
* 複数行: あり
* ワードラップ: あり
 
```
DESCRIBE QUERY EXECUTION(True)
```

```
DESCRIBE QUERY EXECUTION(False)
```

```
OBJECT Get pointer(Object named;"Message.QueryPath")->:=Get last query path(Description in text format)
```

---

検索エリア（改良）

```
$event:=Form event

Case of 
: ($event=On Load)

ALL RECORDS([Contact])

: ($event=On Data Change)

$q:=Self->
$q:=Replace string($q;"@";"")
$q:=Replace string($q;" ";"")

DESCRIBE QUERY EXECUTION(True)

GET TEXT KEYWORDS($q;$words)

  //ゆるめの条件
$criteria1:="@"
For ($i;1;Size of array($words))
$criteria1:=$criteria1+$words{$i}+"@"
End for 

  //ふつうの条件
$criteria2:="@"+$q+"@"

Case of 
  //全件
: (Length($q)=0)
QUERY([Contact];[Contact]名前="@")

  //電話番号
: (Match regex("[0-9-]+";$q))
QUERY([Contact];[Contact]電話番号=$q;*)
QUERY([Contact]; | ;[Contact]携帯番号=$q)

//メール
: (Match regex("[^@]+@[^@]+";Self->))

$criteria:=Replace string(Self->;"@";"＠";*)
QUERY([Contact];[Contact]メール=$criteria)

  //ふつうの条件のみ
: (Size of array($words)=1)
$criteria:="@"+$q+"@"
If (Match regex("[[:Hiragana:][:Katakana:]]+";$q))
QUERY([Contact];[Contact]名前フリガナ=$criteria)
Else 
QUERY([Contact];[Contact]名前=$criteria)
End if 

Else 
  //ゆるめの条件＋ふつうの条件
If (Match regex("[[:Hiragana:][:Katakana:]]+";$q))
QUERY BY FORMULA([Contact];[Contact]名前フリガナ=$criteria1 & \
Replace string([Contact]名前フリガナ;" ";"")=$criteria2)
Else 
QUERY BY FORMULA([Contact];[Contact]名前=$criteria1 & \
Replace string([Contact]名前;" ";"")=$criteria2)
End if 

End case 

DESCRIBE QUERY EXECUTION(False)

OBJECT Get pointer(Object named;"Message.QueryPath")->:=Get last query path(Description in text format)

End case 

GOTO OBJECT(*;OBJECT Get name(Object current))

OBJECT SET ENABLED(*;"Contact.Field.@";Is record loaded([Contact]))
```

---

スプリッター

* 変数名を空にする
* 上: 0
* 下: 高さいっぱい
* 以降のオブジェクトを移動: 無効
* 縦方向サイズ変更: 拡大
* 横方向サイズ変更: なし
* 1ポイント左側に透明な四角形を配置する（ストッパーの役目）

---

フィールド

* 詳細フォームからオブジェクトをコピーする
* 横方向サイズ変更: なし
* オブジェクト名を「Contact.Field.」で始める
* 生年月日フィールドは「ヌルのときブランクにする」

```
$event:=Form event

Case of 
: ($event=On Selection Change)

LISTBOX GET CELL POSITION(*;OBJECT Get name(Object current);$col;$row)
LISTBOX GET TABLE SOURCE(*;OBJECT Get name(Object current);$number;$name;$set)

If ($row#0)
GOTO SELECTED RECORD(Table($number)->;$row)
Else 
UNLOAD RECORD(Table($number)->)
End if 

OBJECT SET ENABLED(*;"Contact.Field.@";Is record loaded(Table($number)->))

End case 
```

---

レコード更新

* フィールド入力エリアの``On Data Change``イベントを有効にする
* フォームの``On Data Change``イベントを有効にする
 
```
$event:=Form event

Case of 
: ($event=On Data Change)

$source:=OBJECT Get data source(*;OBJECT Get name(Object with focus))

If (Not(Nil($source)))
If (Not(Is a variable($source)))
$table:=Table(Table($source))
If (Not(Nil($table)))
If (Is record loaded($table->))
If (Modified record($table->))
SAVE RECORD($table->)
REDRAW(OBJECT Get pointer(Object named;"Contact.List")->)
End if 
End if 
End if 
End if 
End if 

End case 
```

---

🎯リレーショナルデータベースを作成する

カード型からリレーション型にデータベースを作り変える

* CSVデータの読み込み
* テーブルの作成（SQLで「位置参照Ｂ」を作成）

---

国土交通省データ

http://nlftp.mlit.go.jp/isj/

位置参照情報: 国土交通省

---

正規表現

🔷[Match regex](http://doc.4d.com/4dv15r/help/command/ja/page1019.html)

🔷[Document to text](http://doc.4d.com/4dv15r/help/command/ja/page1236.html)

---

SQL

📜[SQLリファレンス > 4DでSQLを使用する > 4Dと4D SQLエンジン統合の原則](http://doc.4d.com/4Dv15R5/4D/15-R5/Principles-for-integrating-4D-and-the-4D-SQL-engine.300-2977352.ja.html)

📜[SQLリファレンス > 4DでSQLを使用する > システムテーブル](http://doc.4d.com/4Dv15R5/4D/15-R5/System-Tables.300-2977353.ja.html)

🔶[CREATE TABLE](http://doc.4d.com/4Dv15R5/4D/15-R5/CREATE-TABLE.300-2977377.ja.html)

🔶[EXECUTE IMMEDIATE](http://doc.4d.com/4Dv15R5/4D/15-R5/EXECUTE-IMMEDIATE.300-2977373.ja.html)

🔶[INSERT](http://doc.4d.com/4Dv15R5/4D/15-R5/INSERT.300-2977380.ja.html)

---

テーブルの調整

* フィールドタイプの変更（テキストから数値に）

🌀文字数は入力制御のためだけなので設定しなくても良い

⚠️実際にレコードを更新するまでデータは変わらない

---
 
 リレートテーブルの作成
 
<img width="840" alt="v2" src="https://cloud.githubusercontent.com/assets/10509075/18534442/1c1e384a-7b26-11e6-82cc-d12e99d895c7.png">

```
$tableName:="都道府県"

ARRAY TEXT($fieldNames;3)
ARRAY TEXT($fieldTypes;3)

$fieldNames{1}:="ID"
$fieldTypes{1}:="INT32 PRIMARY KEY"

$fieldNames{2}:="都道府県コード"
$fieldTypes{2}:="VARCHAR(2)"

$fieldNames{3}:="都道府県名"
$fieldTypes{3}:="VARCHAR(4)"

$sql:="CREATE TABLE IF NOT EXISTS ["+$tableName+"]\r("
For ($i;1;Size of array($fieldNames))
If ($i#1)
$sql:=$sql+",\r"
End if 
$sql:=$sql+"["+$fieldNames{$i}+"] "+$fieldTypes{$i}
End for 
$sql:=$sql+"\r);"

C_LONGINT($tableId)

Begin SQL
EXECUTE IMMEDIATE :$sql;
SELECT TABLE_ID 
FROM _USER_TABLES
WHERE TABLE_NAME LIKE :$tableName
LIMIT 1
INTO :$tableId;
End SQL

$sql:="ALTER TABLE ["+$tableName+"] MODIFY ["+$fieldNames{1}+"] ENABLE AUTO_INCREMENT"

Begin SQL
EXECUTE IMMEDIATE :$sql;
End SQL
```

```
$tableName:="市区町村"

ARRAY TEXT($fieldNames;4)
ARRAY TEXT($fieldTypes;4)

$fieldNames{1}:="ID"
$fieldTypes{1}:="INT32 PRIMARY KEY"

$fieldNames{2}:="都道府県"
$fieldTypes{2}:="INT32"

$fieldNames{3}:="市区町村コード"
$fieldTypes{3}:="VARCHAR(5)"

$fieldNames{4}:="市区町村名"
$fieldTypes{4}:="VARCHAR(10)"

$sql:="CREATE TABLE IF NOT EXISTS ["+$tableName+"]\r("
For ($i;1;Size of array($fieldNames))
If ($i#1)
$sql:=$sql+",\r"
End if 
$sql:=$sql+"["+$fieldNames{$i}+"] "+$fieldTypes{$i}
End for 
$sql:=$sql+"\r);"

C_LONGINT($tableId)

Begin SQL
EXECUTE IMMEDIATE :$sql;
SELECT TABLE_ID 
FROM _USER_TABLES
WHERE TABLE_NAME LIKE :$tableName
LIMIT 1
INTO :$tableId;
End SQL

$sql:="ALTER TABLE ["+$tableName+"] MODIFY ["+$fieldNames{1}+"] ENABLE AUTO_INCREMENT;\r"
$sql:=$sql+"CREATE INDEX ["+$tableName+"."+$fieldNames{3}+"] ON ["+$tableName+"] (["+$fieldNames{3}+"]);"

Begin SQL
EXECUTE IMMEDIATE :$sql;
End SQL
```

```
$tableName:="大字町丁目"

ARRAY TEXT($fieldNames;4)
ARRAY TEXT($fieldTypes;4)

$fieldNames{1}:="ID"
$fieldTypes{1}:="INT32 PRIMARY KEY"

$fieldNames{2}:="市区町村"
$fieldTypes{2}:="INT32"

$fieldNames{3}:="大字町丁目コード"
$fieldTypes{3}:="VARCHAR(12)"

$fieldNames{4}:="大字町丁目名"
$fieldTypes{4}:="VARCHAR(18)"

$sql:="CREATE TABLE IF NOT EXISTS ["+$tableName+"]\r("
For ($i;1;Size of array($fieldNames))
If ($i#1)
$sql:=$sql+",\r"
End if 
$sql:=$sql+"["+$fieldNames{$i}+"] "+$fieldTypes{$i}
End for 
$sql:=$sql+"\r);"

C_LONGINT($tableId)

Begin SQL
EXECUTE IMMEDIATE :$sql;
SELECT TABLE_ID 
FROM _USER_TABLES
WHERE TABLE_NAME LIKE :$tableName
LIMIT 1
INTO :$tableId;
End SQL

$sql:="ALTER TABLE ["+$tableName+"] MODIFY ["+$fieldNames{1}+"] ENABLE AUTO_INCREMENT;\r"
$sql:=$sql+"CREATE INDEX ["+$tableName+"."+$fieldNames{3}+"] ON ["+$tableName+"] (["+$fieldNames{3}+"]);"

Begin SQL
EXECUTE IMMEDIATE :$sql;
End SQL
```

---

* 検索用に「住所」フィールドを追加
 
```
ALL RECORDS([位置参照情報Ｂ])

For ($i;1;Records in selection([位置参照情報Ｂ]))

QUERY([都道府県];[都道府県]都道府県コード=[位置参照情報Ｂ]都道府県コード)

If (Not(Is record loaded([都道府県])))
CREATE RECORD([都道府県])
[都道府県]都道府県コード:=[位置参照情報Ｂ]都道府県コード
[都道府県]都道府県名:=[位置参照情報Ｂ]都道府県名
SAVE RECORD([都道府県])
End if 

QUERY([市区町村];[市区町村]市区町村コード=[位置参照情報Ｂ]市区町村コード)

If (Not(Is record loaded([市区町村])))
CREATE RECORD([市区町村])
[市区町村]都道府県:=[都道府県]ID
[市区町村]市区町村コード:=[位置参照情報Ｂ]市区町村コード
[市区町村]市区町村名:=[位置参照情報Ｂ]市区町村名
SAVE RECORD([市区町村])
End if 

QUERY([大字町丁目];[大字町丁目]大字町丁目コード=[位置参照情報Ｂ]大字町丁目コード)

If (Not(Is record loaded([大字町丁目])))
CREATE RECORD([大字町丁目])
[大字町丁目]市区町村:=[市区町村]ID
[大字町丁目]大字町丁目コード:=[位置参照情報Ｂ]大字町丁目コード
[大字町丁目]大字町丁目名:=[位置参照情報Ｂ]大字町丁目名
SAVE RECORD([大字町丁目])
End if 

[位置参照情報Ｂ]大字町丁目:=[大字町丁目]ID
[位置参照情報Ｂ]住所:=[位置参照情報Ｂ]都道府県名+[位置参照情報Ｂ]市区町村名+[位置参照情報Ｂ]大字町丁目名

[位置参照情報Ｂ]都道府県コード:=""
[位置参照情報Ｂ]都道府県名:=""
[位置参照情報Ｂ]市区町村コード:=""
[位置参照情報Ｂ]市区町村名:=""
[位置参照情報Ｂ]大字町丁目コード:=""
[位置参照情報Ｂ]大字町丁目名:=""

SAVE RECORD([位置参照情報Ｂ])
NEXT RECORD([位置参照情報Ｂ])

End for 

ALERT("DONE")
```

---

リレーションとクエリ

🌀フォーミュラクエリはリレーションが必要ない

```
QUERY BY FORMULA([位置参照情報Ｂ];[都道府県]都道府県名="東京都" & \
(([位置参照情報Ｂ]市区町村コード=[市区町村]市区町村コード)\
 & ([市区町村]都道府県コード=[都道府県]都道府県コード)))
```

📜[デザインリファレンス > データベースストラクチャーの作成 > リレーションの作成と変更](http://doc.4d.com/4dv15r/Creating-and-modifying-relations.300-2964258.ja.html)

🔷[QUERY BY FORMULA](http://doc.4d.com/4dv15r/help/command/ja/page48.html)

🌀クエリは自動リレーションが必要ない

```
QUERY([位置参照情報Ｂ];[都道府県]都道府県名="東京都")
```

📜[ランゲージリファレンス > リレーション > リレーションについて](http://doc.4d.com/4dv15r/About-Relations.300-2937247.ja.html)

🔷[QUERY](http://doc.4d.com/4dv15r/help/command/ja/page277.html)

---

リレート1フィールドをリスト画面に表示

* 割り当てるフィールド
* ソースフィールド
* 自動N対1オプション

🌀ユーザーインタフェース（フォーム）は自動リレーションが必要

📜[デザインリファレンス > アクティブオブジェクトを使用する > フィールドおよび変数オブジェクト](http://doc.4d.com/4dv15r/Field-and-variable-objects.300-2964137.ja.html)

📜[デザインリファレンス > データベースストラクチャーの作成 > リレーションのタイプ](http://doc.4d.com/4dv15r/Types-of-relations.300-2964243.ja.html)

📜[デザインリファレンス > データベースストラクチャーの作成 > リレーションプロパティ](http://doc.4d.com/4dv15r/Relation-properties.300-2964252.ja.html)

---

🎯コンポーネントを作成する

📜[デザインリファレンス > 4Dコンポーネントの開発とインストール > コンポーネントとホストデータベースの相互作用](http://doc.4d.com/4dv15r/Interaction-between-components-and-host-databases.300-2964175.ja.html)

---

エクスターナルデータベース

🌀通常の方法で作成したデータベースが使用できる

🔶[USE DATABASE](http://doc.4d.com/4dv15r/USE-DATABASE.300-2977361.ja.html)

🔶[CREATE DATABASE](http://doc.4d.com/4dv15r/CREATE-DATABASE.300-2977366.ja.html)

⚠️パスワードを設定してはいけない

---

🎯ウィジェットを作成する

📜[デザインリファレンス > サブフォームとウィジェット > ページサブフォーム](http://doc.4d.com/4dv15r/Page-subforms.300-2964164.ja.html)

⚠️フォームの``On Data Change``イベントが有効でなければならない

⚠️ページ2以降にウィジェットを配置することはできない

---
