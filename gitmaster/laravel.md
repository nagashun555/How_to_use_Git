# larabel log
認証を実装するためのLaravel Breezeをインストールする．
以下のコマンドを実行し，必要なファイルをダウンロードする．

    composer require laravel/breeze --dev
    
下記コマンドでインストールする．

    php artisan breeze:install
       
 下記コマンドを実行し，その他必要なパッケージをインストールしてビルドする．(ここでNode.jsが動く)
 
    npm install && npm run dev

続いて，Laravel から MySQL にアクセスするための設定を行う．今回はプロジェクト作成の時点で設定されているため項目の確認のみ行う．
エディタから.envファイルを開く．.envファイルはlaratterディレクトリの直下に配置されている．

    DB_CONNECTION=mysql
    DB_HOST=mysql
    DB_PORT=3306
    DB_DATABASE=laratter
    DB_USERNAME=sail
    DB_PASSWORD=password

Model とマイグレーションファイルは一度に両方とも作成することができる．

下記コマンドは Model を作成するコマンドだが（Tweetがモデル名），-mをつけることでマイグレーションファイルも同時に作成できる．この手法を用いることで，Model 名とマイグレーションファイル内のテーブル名が自動的に対応する．

早速実行．

    php artisan make:model Tweet -m

database/migration/2021_09_23_130915_create_tweets_table.phpを開く．これがマイグレーションファイルである．
カラムを追加するため，下記にように編集する．今回はtweet，descriptionの 2 カラムを追加する．

カラムを追加するときはデータ型も設定する．tweetは文字列（string），descriptionはテキスト（text）を設定している．

idははじめから用意されているので設定不要．
【tips】

- nullable()を記述することで，入力必須でなくすることができる．他にunique()で重複を禁止することもできる．
- timestamps()はcreated_atカラムとupdated_atカラムを自動的に設定してくれる．

## マイグレーション実行
マイグレーションを実行するとテーブルが作成される．以下のコマンドを実行する．

    php artisan migrate

Laravel には「seeder」という機能があり，テスト用のデータを簡単に作成することができる．

今回はダミーデータを 10 件作成する．database/seeders/DatabaseSeeder.phpを以下のように編集する．

    public function run(
    {
      // ↓この行のコメントから外す
      \App\Models\User::factory(10)->create();
    }

下記コマンドを実行してテストユーザを作成する．

    php artisan db:seed

エラーになる場合はマイグレーションファイルの内容が間違っていることが多い．修正して以下のコマンドを実行する．

    php artisan migrate:fresh
    
## ルーティングとコントローラの作成
コントローラとルーティングを作成する．今回のコントローラ名はTweetControllerとする．

--resourceをつけることで，よく使用する処理（代表的な CRUD 処理）を一括して作成することができる．

    php artisan make:controller TweetController --resource
    
これでコントローラファイルが作成された．続いてルーティングを作成する．

ルーティングファイルはroutes以下に配置される．

routes/web.phpを以下のように編集する．

## コントローラの実装（一部）
URL にリクエストが来た場合に実行される関数はコントローラに記述される．まずはリクエストが来た場合に特定の画面を表示するよう処理を記述する．

コントローラはapp/Http/Controllers以下に配置される．

app/Http/Controllers/TweetController.phpの内容を以下のように編集する．

view()関数は指定したビューファイル（画面）を表示する．tweet.indexはtweetフォルダのindex.blade.phpの意味．ビューファイルは次項で作成する．

# 10.4 必要な画面の作成と動作確認

## 共通画面の作成
実際にブラウザ画面に表示される内容を記述する．ビューファイルは*.blade.phpの形式で作成する．これは「blade テンプレート」と呼ばれる形式であり，コントローラとのデータのやり取りなどに最適化されている．

ビューファイルはlatavel_tweet/resources/viewsディレクトリ以下に配置する．

blade テンプレートでは共通パーツと個別パーツを組み合わせて画面を構成する．例えば，アプリケーション全体を通して表示されるナビゲーションバーやエラー画面などは共通パーツである．

それぞれのパーツは「コンポーネント」と呼ばれる．

## 共通パーツ（エラー表示）の作成
入力値が不正な場合などはエラー画面を表示して対応する．

tweet の入力や編集など複数の画面で必要となるため，共通のコンポーネントとして作成する．

latavel_tweet/resources/viewsの中にcommonフォルダを作成する． commonフォルダの中にerrors.blade.phpファイルを作成する．

errors.blade.phpに以下の内容を記述する．

## 共通パーツ（ナビゲーションバー）の調整
ナビゲーションバーもアプリケーションを通じて使用するため，共通パーツとして作成する．

「tweet 一覧」と「tweet 作成」の 2 種類のリンクを作成しておく．

resources/views/layouts/navigation.blade.phpの内容を以下のように編集する．

全部で 4 箇所追記があるので注意．長いので全コピペ推奨．

## tweet 作成画面の作成
まず，resources/viewsの中にtweetフォルダを作成する．

続いて，tweetフォルダの中に以下のファイルを作成する．今後必要になる画面のファイルも合わせて作成している．

- index.blade.php（tweet 一覧画面）
- create.blade.php（tweet 作成画面）
- show.blade.php（tweet 詳細画面）
- edit.blade.php（tweet 編集画面）
ファイルを作成したらcreate.blade.phpを以下のように編集する．

【解説】

- route('tweet.store')はTweetControllerのstore()関数にデータを送ることを示している．
- @csrfはセキュリティ対策．フォームからデータを送信するときには必ず記述すること．

## 動作確認（作成画面）
編集したらブラウザからlocalhost/tweet/createにアクセスして動作確認． 下記画面になっていれば OK．

## tweet 一覧画面の作成
続いて，index.blade.phpを以下のように編集する．

こちらはエラーが発生する．

コントローラから$tweetsという名前でデータを受け取るよう記述しているが，コントローラからデータが送られていないためである．

とりあえず空のデータを送るようにapp/Http/Controllers/TweetController.phpのindex()を内容を以下のように編集する．

【解説】

- view()関数の第 2 引数に渡したいデータを配列で記述する．
- ここでは「tweets」という名前で「空の配列」を送っている．
- ビューファイル側では$tweetsとすることで渡されたデータを使うことができる．
- また，変数などのデータはvar_dump()またはdd()またはddd()で確認することができる．これらを使ってデータの構造を確認しながら進めると良いだろう．

## 動作確認
編集したらブラウザからlocalhost/tweetにアクセスして動作確認． 下記画面になっていれば OK．

# 10.5























