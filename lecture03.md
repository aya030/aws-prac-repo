# AWSフルコース 第3回課題

 ## AP サーバーについて
 - AP サーバーの名前とバージョンを確認
     - Puma version: 5.6.5 (ruby 3.1.2-p20) ("Birdie's Version")
 ![apサーバー](https://user-images.githubusercontent.com/101713134/236299484-4347e680-95fe-4025-b5b9-8304814317f8.png)

 - AP サーバーを終了させた場合、引き続きアクセスできますか?
     - できない
 ![APサーバー止めた時](https://user-images.githubusercontent.com/101713134/236300764-bc08ff23-7bdf-424f-9ed0-c81d46bd1d3b.png)


 ## DBサーバーについて
 - サンプルアプリケーションで使った DB サーバー(DB エンジン)の名前と、今 Cloud9で動作しているバージョン
     - mysql Ver 8.0.33 for Linux on x86_64 (MySQL Community Server - GPL)
 ![DBサーバー](https://user-images.githubusercontent.com/101713134/236301540-77dcfd9f-98ba-48cb-a66f-9bcef4431965.png)

 - DB サーバーを終了させた場合、引き続きアクセスできますか?
     - できない
 ![DB起動止めた時](https://user-images.githubusercontent.com/101713134/236301852-ad6dbb00-6ccd-421d-a526-19675814a723.png)


 ## Rails の構成管理ツール名
 - Bundler


 ## 今回の課題で学んだこと
 ### 環境構築の手順について
 1. Cloud9にgithub上の[raisetech-live8-sample-appl](https://github.com/yuta-ushijima/raisetech-live8-sample-appl)をクローンする
       `git clone サンプルアプリのアドレス`
2. サンプルアプリのバージョンに合わせる
    - アプリのバージョンを調べる `vendor/.ruby-version`(3.1.2)
    - 現状のRubyのバージョンを調べる `ruby -v`(2.6.3)
    - rvmを最新バージョンに更新する
       `rvm get head`
       `rvm reload`
    - rvmを用いてインストールできるRubyリストを表示する
      `rvm list known`
    - 3.1.2をインストールする
       `rvm install 3.1.2`
    - インストールされているか確認する
       `rvm list`
    - バージョンを変更する
       `rvm --default use 3.1.2`
3. MySQLのセットアップ
    - MySQLのインストール
 　　　 - 初期パスワードの入手
    - 初期パスワードの変更
    - 変更したパスワードを`config/database.yml`に記載
    - SQLの起動確認 `mysql -u root -p` 
4. Gitエディターを永続的にnanoからvimに変更
    - .bashrc側を修正する `vim ~/.bashrc`
    - 20行目`config --global core.editor nano`をコメントアウト
    - 21行目`git config --global core.editor 'vim -c "set fenc=utf-8"'`を追加
5. アプリに必要なライブラリをインストール
    - `gem install bundler:2.3.14`
    - `bundle install`
6. DBを作成・起動
    - database.ymlを読み込みファイルに基づいてデータベースを作成
       `bundle exec rails db:create`
    - エラー① Can't connect to local MySQL server through socket '/tmp/mysql.sock'(2)
       - `database.yml`のsocket記載箇所に`/var/lib/mysql/mysql.sock'に変更
       - `db/migrate`の中にあるファイルに基づいてデータベースにテーブルを作成
          `bundle exec rails db:migrate`
7. サーバー起動
    - `Procfile.dev`の3000を8080に変更 `web: bin/rails server -p 8080 -b 0.0.0.0`
    - `bundle exec rails server -b 0.0.0.0
    - 表示
 ![起動](https://user-images.githubusercontent.com/101713134/236383116-d396c7e8-29b1-4752-839c-e7d1f864b935.png)
       - エラー②　BlockedHost
 ![エラーBlockedHost](https://user-images.githubusercontent.com/101713134/236379843-6511c615-0df8-407a-b6d5-b5b34a1e08ae.png)
       - `config/environments/development.rb`のend直前に`config.hosts << "××.amazonaws.com"`を追加する
MacBook-Pro:aws-practice-repo ayane$ vim lecture03.md

# AWSフルコース 第3回課題

 ## AP サーバーについて
 - AP サーバーの名前とバージョンを確認
     - Puma version: 5.6.5 (ruby 3.1.2-p20) ("Birdie's Version")
 ![apサーバー](https://user-images.githubusercontent.com/101713134/236299484-4347e680-95fe-4025-b5b9-8304814317f8.png)

 - AP サーバーを終了させた場合、引き続きアクセスできますか?
     - できない
 ![APサーバー止めた時](https://user-images.githubusercontent.com/101713134/236300764-bc08ff23-7bdf-424f-9ed0-c81d46bd1d3b.png)


 ## DBサーバーについて
 - サンプルアプリケーションで使った DB サーバー(DB エンジン)の名前と、今 Cloud9>で動作しているバージョン
     - mysql Ver 8.0.33 for Linux on x86_64 (MySQL Community Server - GPL)
 ![DBサーバー](https://user-images.githubusercontent.com/101713134/236301540-77dcfd9f-98ba-48cb-a66f-9bcef4431965.png)

 - DB サーバーを終了させた場合、引き続きアクセスできますか?
     - できない

