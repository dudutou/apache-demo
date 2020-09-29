# apache-demo
The demo for studying apache.

## 実行環境
* Dockerがインストール済み（Docker Desktop for Mac で動作確認済み）
* port番号80/81/10080/8888が空いている

## 実行方法
* 本リポジトリをローカルへcloneしてください
* コマンド `cd apache-demo` を実行ください
* コマンド `docker-compose up -d` を実行してApacheコンテナーを生成ください。
* コンテナを削除する場合はコマンド `docker-compose down` を実行ください。

## デモ
下記のデモ方法に沿ってデモを実施ください  
| 大項目 | 小項目 | 設定値link | デモ内容 |
|:--|:--|:--|:--|
|basic | DirectoryIndex | https://github.com/ki-ri/apache-demo/blob/master/conf/my-httpd.conf#L300-L302 |ブラウザで http://localhost へアクセスしたら、indexファイル `This is a apache demo index page for ki-ri.` が表示される。 |
|basic | Listen | https://github.com/ki-ri/apache-demo/blob/master/conf/my-httpd.conf#L52-L54 |ブラウザで http://localhost:80(http://localhost:81 か http://localhost:10080) へアクセスしたら、`This is a apache demo index page for ki-ri.` が表示され、3つのポート（80/81/10080）を Listen している。 |
|basic |DocumentRoot | https://github.com/ki-ri/apache-demo/blob/master/conf/my-httpd.conf#L267-L294 |ブラウザで http://localhost/index.html へアクセスしたら、`This is a apache demo index page for ki-ri.` が表示される。 |
|basic |Directory/Options Indexes | https://github.com/ki-ri/apache-demo/blob/master/conf/basic_setting.conf#L10-L14 |ブラウザで http://localhost/sub/ へアクセスしたら、sub ディレクトリ下のファイル一覧が表示される。 |
|basic |Directory/AllowOverride/Basic_Auth | https://github.com/ki-ri/apache-demo/blob/master/conf/basic_setting.conf#L16-L22 |ブラウザで http://localhost:81/sub/sub2/hello.html へアクセスしたら、ベーシック認証が要求されて、test/test を入力したら `test of sub/sub2/hello.html` の内容が表示される。 |
|basic |Directory/Basic_Auth | https://github.com/ki-ri/apache-demo/blob/master/html/public-html/sub/sub2/.htaccess |ブラウザで http://localhost:81/auth/ へアクセスしたら、ベーシック認証が要求されて、`test/test` を入力したら `test ~/auth/index.html` の内容が表示される。 |
|basic |Directory/Location マージ順番 | https://github.com/ki-ri/apache-demo/blob/master/conf/my-httpd.conf#L593-L598<br>https://github.com/ki-ri/apache-demo/blob/master/conf/basic_setting.conf#L16-L22 |ブラウザで http://localhost:80/auth/ へアクセスしたら、ベーシック認証が**要求されず** `test ~/auth/index.html` の内容が表示される。Directory中のベーシック認証の設定がLocation中の設定で上書きされて無効になる。 |
|basic | Alias | https://github.com/ki-ri/apache-demo/blob/master/conf/basic_setting.conf#L2-L8 |ブラウザで http://localhost/doc/index.html へアクセスしたら、test ~/doc/index.html which is outside of DocumentRoot for ki-ri が表示され、DocumentRoot以外の場所のコンテンツへもアクセスできる。 |
|basic |cgi | https://github.com/ki-ri/apache-demo/blob/master/conf/basic_setting.conf#L6-L7 |ブラウザで http://localhost/doc/test.cgi へアクセスしたら、Hello, ki-ri. が表示されて、CGIが正しく実行される。 |
|basic |VirtualHost | https://github.com/ki-ri/apache-demo/blob/master/conf/rewrite-vhosts.conf |ブラウザで http://localhost:10080/server-status/ へアクセスしたら、Apache Server Status for localhost のコンテンツが表示され、VirtualHost *:10080 へ正しくアクセスできる（http://localhost:10080/server-status/  へアクセスして表示されない）。 |
|+α |mod_rewrite | https://github.com/ki-ri/apache-demo/blob/master/conf/rewrite-vhosts.conf#L12-L14 |インターナルリダイレクト：ブラウザで http://localhost:81/needLogin/internal[任意文字列] へアクセスしたら、URLがそのまま、/login.html の内容が表示される。 |
|+α |mod_rewrite | https://github.com/ki-ri/apache-demo/blob/master/conf/rewrite-vhosts.conf#L16-L18 |エクスターナルリダイレクト：ブラウザで http://localhost:81/needLogin[/internal以外の任意文字列] へアクセスしたら、URLが http://localhost:81/login.html へリダイレクトして、/login.html の内容が表示される。 |
|+α |mod_rewrite | https://github.com/ki-ri/apache-demo/blob/master/conf/rewrite-vhosts.conf#L20-L22 |リダイレクト処理で元のパスの一部データを使う：ブラウザで http://localhost:81/needRedirect/something へアクセスしたら、URLが http://localhost:81/something へリダイレクトする（something=/login.htmlとか。もし存在しないパスの場合は更に下記のようにリダイレクトする）。 |
|+α |mod_rewrite | https://github.com/ki-ri/apache-demo/blob/master/conf/rewrite-vhosts.conf#L24-L28 |存在しないパスが指定される場合にindex.htmlへリダイレクトする：ブラウザで http://localhost:81/[存在しないパス] へアクセスしたら、URLが http://localhost:81/index.html へリダイレクトして、/login.html の内容が表示される。 |
|+α |mod_proxy | https://github.com/ki-ri/apache-demo/blob/master/conf/my-httpd.conf#L598-L601 |Apache から Tomcat へのプロキシ機能：ブラウザで http://localhost/tomcat/sample/ へアクセスしたら、リクエストをApache から Tomcat へ転送して、Tomcatにデプロイしたサンプルアプリの内容が表示される。 |