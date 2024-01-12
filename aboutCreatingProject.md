# Fetchするためにやったこと
next.js使うとずっとfetchできなかったけど動画見ていけた：[How To Create a Node.js + Next.js project](https://www.youtube.com/watch?v=5Vxx5UkjV4s)

### 手順
1. PPGディレクトリ作成
2. cd PPG -> PPG内部でbackendディレクトリを作成
3. 同じとこで
``` terminal
 $　npx create-next-app frontend
```
する　
- typesclipt -> YES
- eslint -> yes
- tailwand -> no
- use src -> yes
- app router -> NO
- custumise -> no
- ⇨ lsするとbackendとfrontendが存在しているはず

4.   $ cd backendして
``` terminal
  $ npm init
```
する　※これだいじ！！！たぶん
- package name -> (backend)
- version -> (1.0.0)
- description -> enter
- entry point -> (index.js) server.js　<- 書き換え
- あとは全部enterでOK -> backend/ oackage.jsonができてるはず

5. ここからVSコード
##### サーバーのセットアップ

1. vscode内でterminal開いて $cd backend
2. $ touch server.js(entry pointで設定したやつ)
3. $ npm i express -> package-lock.jsonができる
4. $ npm i nodemon --save-dev <-これはあったら便利なやつ npm run devで起動できるようにするやつ　一応いれた
5. package.jsonの"scripts"の"test"の下に"start": "node server"と"dev": "nodemon server"追記
``` terminal
{
  "name": "backend",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server",
    "dev": "nodemon server"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "cors": "^2.8.5",
    "express": "^4.18.2"
  },
  "devDependencies": {
    "nodemon": "^3.0.2"
  }
}
```
になる
6. バックエンドでAPIつくる

##### クライアントのセットアップ
7. backendディレクトリ内でcorsをinstallする: 
``` terminal
$ npm i cors
```
8. server.js 内に const cors = require('cors')とapp.use(cors());追記
