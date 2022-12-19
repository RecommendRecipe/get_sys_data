# 1からクックパッドの環境を構築する

## loginするコマンド
`mysql --user=root --password=root`
## 既に作成したcookpadというデータベースを選択
`use cookpad;`
## cookpadデータセットのsqlファイルをmysqlに読み込み
mysql上にログインした後に以下のコマンドで読み込む  
`source "cookpad_data.sqlのパス";`

# データの前処理

## 前処理で消しとくべき記号
これらはcsv形式にしたときに区切りをわからなくさせるので要注意  
クックパッドで言えば変換数が2000件近く変わります  

","は"、"に置換しておく  
"\t","\n","\r",""","'"は削除  

## データの置換（削除）
`UPDATE "対象のテーブル" SET "対象のカラム"=REPLACE("対象のカラム", "対象の文字列", "置換後の文字列");`  
これを対象のデータ（食材名や手順などのデータ）について行う  

# [データの書き出し](https://weblabo.oscasierra.net/mysql-select-into-outfile/)

`SELECT "カラム名" FROM "テーブル名" INTO OUTFILE '/var/lib/mysql-files/"ファイル名".csv' FIELDS TERMINATED BY ',' ENCLOSED BY '"' ESCAPED BY '"' LINES TERMINATED BY '\r\n';`  

後は指定したディレクトリから書き出したcsvファイルをダウンロードしてくればいい

## ここで抽出するデータ
レシピのタイトルと分量データ <- recipes = レシピ情報の確認と分量による正規化  
レシピの食材データ <- ingredients = レシピの食材情報  
レシピの調理手順 <- steps = 機械学習の学習データ