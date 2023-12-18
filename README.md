# ppg用の追記諸々
## Docker作成関連
- りんがやったこと
### バックエンド周辺

1. PPG以下に一応Dockerディレクトリ作成
``` terminal
$ mkdir Docker
```
ついでにnodeもインストールしとく
``` terminal 
$ sudo apt update -y
$ curl https://get.volta.sh | bash
$ exec $SHELL -l
$ volta install node
$ volta install npm
```
nextもインストールしとく(Dockerディレクトリ)
nextあるか確認
```terminal 
$ npm list -g next
```
なければ
``` terminal
$ npm install -g next
```
依存関係をインストール(Dockerディレクトリ)
``` terminal 
$ npm install
```

2. Backendディレクトリ作成
(※でもExpress使うなら$npx express-generator Backendとかでフォルダごと作成したほうが良かったのかも)
``` terminal 
$ cd Docker
$ mkdir Backend
```
3. Backendの初期化(express, postgreSQLのインストール)
``` terminal
$ npm init -y
$ npm install express pg
```
4. app.jsの作成
``` terminal
$ vi app.js
```
app.jsの中身

``` terminal
const express = require('express');
const { Pool } = require('pg');

const app = express();
const port = 3002;

const pool = new Pool({
  user: 'your_postgresql_user',
  host: 'localhost',
  database: 'your_postgresql_database',
  password: 'your_postgresql_password',
  port: 5432,
});

app.get('/api/items', (req, res) => {
  pool.query('SELECT * FROM items', (err, result) => {
    if (err) {
      res.status(500).json({ error: err.message });
      return;
    }
    res.json(result.rows);
  });
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```
5. package.jsonの変更

``` terminal 
/* backend/package.json */
{
  // ...
  "scripts": {
    "start": "node app.js",
    // ...
  },
  // ...
}
```


### データベース周辺

1. Backendディレクトリに移動してDataBaseディレクトリ作成
``` terminal 
$ cd Backend
$ mkdir DataBase
```
2. DataBaseディレクトリに移動してinit.jsを作成
``` terminal 
$ vi init.js
```
init.jsの中身(今のとこ)

``` terminal
/* Backend/Database/init.js */

const { Pool } = require('pg');
const path = require('path');

const pool = new Pool({
  user: 'your_postgresql_user',
  host: 'localhost',
  database: 'your_postgresql_database',
  password: 'your_postgresql_password',
  port: 5432,
});

pool.query(
  'CREATE TABLE IF NOT EXISTS items (id SERIAL PRIMARY KEY, name VARCHAR(255))',
  (err, result) => {
    if (err) {
      console.error('Error creating table:', err);
      pool.end();
      return;
    }

    // データの挿入
    const insertQuery = 'INSERT INTO items (name) VALUES ($1), ($2)';
    const values = ['Item 1', 'Item 2'];

    pool.query(insertQuery, values, (err, result) => {
      if (err) {
        console.error('Error inserting data:', err);
      }
      pool.end();
    });
  }
);
```

### フロントエンド周辺

1. Dockerディレクトリ内でppgのデータをgit clone
``` terminal 
$ git clone (ppgのURL)
```
lsすると、Docker以下にBackendとppgディレクトリが存在しているはず


### Dockerコンテナを構築するためにDockerfile作成
 
1. Dockerディレクトリ内に作成
``` terminal 
$ vim Dockerfile
```
Docerfileの中身
``` terminal 
/* Docker/Dockerfile */
# フロントエンドのビルドステージ
FROM node:alpine as ppg
WORKDIR /app
COPY ppg/package*.json ./
RUN npm install
COPY ppg .
RUN npm run build

# バックエンドの実行ステージ
FROM node:alpine
WORKDIR /app
COPY Backend/package*.json ./
RUN npm install
COPY Backend .
COPY --from=ppg /app/.next /app/public

# データベースの初期化
RUN node DataBase/init.js

EXPOSE 3001
CMD ["npm", "start"]
```
Docker/.dockerignoreを作成、記入
``` terminal 
node_modules
build
```
Docker/docker-compose.ymlを作成、記入
``` terminal
# docker-compose.yml
version: '3'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3001:3001"
    restart: always
```
#### Dockerコンテナ起動
``` terminal 
$ docker-compose up -d
```

## 起動関連
ppgディレクトリ
``` terminal 
$ npm run dev
```
Backendディレクトリ
``` terminal 
$ npm start
```
