
# nuxt-univ-app1-heroku

> nuxt.js universal Heroku Flow

## Nuxtアプリを作成

``` bash
# install dependencies
$ npx create-nuxt-app nuxt-univ-app1-heroku
? Project name nuxt-univ-app1-heroku
? Project description nuxt.js universal Heroku Flow
? Use a custom server framework express
? Choose features to install Axios
? Use a custom UI framework none
? Use a custom test framework none
? Choose rendering mode Universal
? Author name hiramatsu
? Choose a package manager npm
$ cd nuxt-univ-app1-heroku
# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, checkout [Nuxt.js docs](https://nuxtjs.org).

## Heroku へデプロイするための設定を行う。
1.pakege.json
package.json 内の heroku-postbuild スクリプトを使って、Heroku に npm run build を実行するよう伝えます。
```
"scripts": {
  "dev": "nuxt",
  "build": "nuxt build",
  "start": "nuxt start",
  "heroku-postbuild": "npm run build"
}
```
2. Procfile
Heroku はアプリの dyno によって実行されるコマンドを指定する Procfile (ファイル拡張子を付けずにファイル名を Procfile という名前にします）を使用します。Procfile を起動するのはとてもシンプルで、以下の行を含める必要があります
```
web: npm run start
```

## GitHub リポジトリの作成
1. GitHub ログイン後のトップページから、Repositories の New ボタンをクリックします。
2. Create a new repository の画面に遷移するので、リポジトリ名、ライセンス等を入力。Initialize this repository with a READMEはチェックせず画面下のほうにある Create repository ボタンをクリックします。
 


## プロジェクトを GitHub に Push する
1. git add -A
2. git commit -m "first commit"
3. git remote add origin https://github.com/hiramatsuYoshiaki/プロジェクト名
4. git push -u origin master

## herokuアプリを作成
ex:https://qiita.com/sho7650/items/ebd87c5dc2c4c7abb8f0 
1. Heroku へログインして、Heroku ダッシュボード画面へ入ります。
2. Heroku 上でアプリケーションを動かすためには、Heroku アプリケーションが必要です。感覚としては、空の箱を一つ準備して、ソースコードをそこへ放り込むと、Webアプリケーションが動く、というイメージです。まず、この空の箱を準備します。アプリケーション一覧の画面になりますので、ここで右上の「New」をクリックしてプルダウンメニューからCreate new appをクリックします。 
3. Heroku アプリケーションを作成する画面になります、App name は Herokuアプリケーションへアクセスするときの URLの一部になります。ここで指定した App name が、https://<App name>.herokuapp.com としてアクセスできるようになります。ドメイン名の一分になるので、世界中で唯一の名前であることが必須となります。入力しながら、使えるかどうかお試しください。面倒な場合は空欄にしておけば、自動的にアプリケーション名が割り振られます。 

## ディプロイ設定
1. Heroku ダッシュボードの「setting」タブに移動してください。
2. config varsに入力しADDします。
```
key:host  value: 0.0.0.0 
key:NPM_CONFIG_PRODUCTION value:false
key:NODE_ENV value:production 
```

## GitHub と連携しよう。
1. Heroku ダッシュボードの「Deploy」タブに移動してください。
2. 下部の Deployment method という欄の、GitHubアイコンの「GitHub」をクリックします。 
3. 「Connect to GitHub」ボタンをクリックします。 
4. GitHubでの認可画面が出てきますので、「Authorize heroku」ボタンをクリックします。 
5. 連携するリポジトリを入力しConnect」ボタンをクリックします。 
6. 連携したいブランチを選択して、「diploy branch」ボタンをクリックして、手動ディプロイをします。Buildプロセスが開始されます。異常なくDeployできれば、次のように「Your app was successfully deployed.」と表示され、「View」ボタンが現れます。 
7. ブラウザ画面が表示されれば Deploy成功です。 

## GitHub と自動連携。
1. 「Enable Automatic Deploys」ボタンをクリックします。GitHubの対象のブランチにソースコードが変更され、コミットされたら、自動的にHerokuへDeployします。
2. 「Wait for CI to pass before deploy」は、Deployの前に Heroku CIが動き、自動的にソースコード内で設定されたユニットテストが実行されます。そのテストコードがすべてパスしたら、Deployするフローとなります。






























## herokuアプリを作成
1.Herokuでアプリケーションを作成します。 
 
これは、Herokuがソースコードを受け取る準備をするためのものです。 
アプリを作成すると、git remote（と呼ばれるheroku）も作成され、 
ローカルのgitリポジトリに関連付けられます。 
```
$ cd nuxt-univ-app1-heroku
$ heroku create nuxt-univ-app1-heroku
Creating nuxt-univ-app1-heroku... done, stack is heroku-18
http://nuxt-univ-app1-heroku.herokuapp.com/ | https://git.heroku.com/nuxt-univ-app1-heroku.git
Git remote heroku added
```
2. npm run build を実行できるようにするために、Heroku にプロジェクトの devDependencies をインストールすることを伝える必要があります 

```
$ heroku config:set NPM_CONFIG_PRODUCTION=false
```

3. アプリケーションにホスト 0.0.0.0 を listen させプロダクションモードで起動します 
```
$ heroku config:set HOST=0.0.0.0
$ heroku config:set NODE_ENV=production
```

4. package.json 内の heroku-postbuild スクリプトを使って、Heroku に npm run build を実行するよう伝えます
```
"scripts": {
  "dev": "nuxt",
  "build": "nuxt build",
  "start": "nuxt start",
  "heroku-postbuild": "npm run build"//追記
}
```

5. ルートディレクトリにProcfile (ファイル拡張子を付けずにファイル名を Procfile という名前にします）を新規作成し、procfileに、Herokuで使われるアプリの dyno によってアプリケーションを起動するために実行するコマンドを記入します。

```
web: npm run start
```

4. Heroku に git push します
```
$ git add .
$ git commit
$ git push heroku master
```

5. Heroku でホストされていることを確認　
```
$ heroku open
```


## Node.jsを使ってHerokuを始めよう 
https://devcenter.heroku.com/articles/getting-started-with-nodejs

## url
https://dashboard.heroku.com/apps/nuxt-univ-app1-heroku


## heroku
