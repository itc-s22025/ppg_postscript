# memo
- サーバー型DBを用いたprismaの使用方法調べる(テキストにのってるのはローカルDB)
- prisma使うならDB作成しないほうがいいかも？prisma使ってモデルから作成したほうがいいのかも
- フロント・バック・DB用いたアプリケーションの作成手順確認
- Poolてなに(postgres)
- [PosgreWeb](https://postgresweb.com/)
  

### フロントエンドとバックエンドの連携
⇨ RestfulAPIを使用して連携させる
- ビューエンジン(テンプレートエンジン)...フロントが用意されていない場合につかうやつ？
  
1. APIの設計:
- バックエンド（Node.jsとExpress）で、必要なエンドポイント（APIエンドポイント）を設計
- エンドポイント: クライアント（Next.js）がデータを要求したり送信したりするためのURL・エンドポイント等
2. APIの実装:
- バックエンドで、設計したAPIエンドポイントに対応するルートやコントローラーを実装
- バックエンドではデータベースとのやりとりやビジネスロジックを処理し、結果をJSON形式で返す
3. Fetch APIまたはライブラリの使用:
- フロントエンド（Next.js）でfetch APIやAxiosなどのライブラリを使用して、バックエンドのAPIエンドポイントにリクエストを送信
- リクエストの中には、データを取得するための情報（例：GETリクエスト）やデータを送信するための本文（例：POSTリクエスト）を含む
4. データの処理:
- フロントエンドで、APIから受け取ったデータを処理して必要な形に整形
- Reactコンポーネント内で、状態やプロパティとしてデータを保持し、それをもとにUIを構築
5. データの表示:
- Reactコンポーネントで処理したデータを使ってWebページ上にUIをレンダリング

#### フロントのサンプルコード
``` terminal
// pages/index.js

import React, { useState, useEffect } from 'react';

const Home = () => {
  const [data, setData] = useState([]);

  useEffect(() => {
    // バックエンドのAPIエンドポイントにリクエストを送信
    fetch('/api/data')
      // .then ... 返ってきたレスポンスをjson形式に直して
      .then(response => response.json())
      // .them ... それを更にsetDataにわたしてる？
      .then(data => setData(data));
  }, []);

  return (
    <div>
      <h1>Webアプリ</h1>
      <ul>
        {data.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
};

export default Home;
```
#### バックのサンプルコード
``` terminal
// server.js
//fetch API(front)を使用して/api/dataエンドポイントにGETリクエストを送信

const express = require('express');
const app = express();
const port = 3001;

// サンプルデータ
const sampleData = [
  { id: 1, name: 'Item 1' },
  { id: 2, name: 'Item 2' },
  { id: 3, name: 'Item 3' },
];

// APIエンドポイントの実装
app.get('/api/data', (req, res) => {
  // sampleDataをjson形式でレスポンス
  res.json(sampleData);
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```
##### APIエンドポイント：クライアントが特定の機能やデータにアクセスするための入り口
- クライアントはHTTPリクエスト（GET、POST、PUT、DELETEなど）を使用してAPIエンドポイントにリクエストを送信、応答を受け取る
- APIエンドポイントはリクエストに基づいて適切な処理を行い、クライアントにレスポンスを返します
##### Httpリクエスト
- HTTPリクエスト：リクエストライン、ヘッダ、メッセージボディからなる
- リクエストラインの例：POST /test.html HTTP/1.1\r\n(test.htmlにHTTP/1.1でPOSTするよ！)
- ヘッダ：オリジンとかコンテントの長さとかかかれてる
- メッセージボディの例：q=test&submitTest=%E6%A4%9C%E7%B4%A2(受け渡されるパラメータの値とか)
##### Fetch APIてなに
- ブラウザ上のJavaScriptからHTTPネットワークリクエストを行うためのインターフェース
- リクエストやレスポンスといったプロトコルを操作する要素にアクセスするためのJSインターフェース
- HTTPリクエストを発行するAPI
- sample⇩
``` terminal
(await fetch("https://qiita.com/api/v2/items")).json();
```
⇨"https://qiita.com/..." にAPIリクエストを送信している
- つまり``` fetch('/api/data')```はプロジェクト内のapi/data/にAPIリクエストを送信している
- そういえばリアクトってapiフォルダがデフォルトであるらしいけどバージョン変わってるからわかんない
###### つまりバックエンド側では``` app.get() ```でルーティング(エンドポイントとレスポンスの定義)、フロント側ではそのエンドポイントに``` fetch() ```でアクセスしてフロントに表示するための処理をかくってこと？


## promiseについて
axios.post()するとPromiseオブジェクト(Promise{pending})が返ってきた
##### Promiseオブジェクト...非同期処理の完了もしくは失敗の結果及びその結果の値を返す
- 待機(Pending) ...初期状態。成功も失敗もしていない
- 履行(fulfilled) ...処理が成功して完了したことを意味
- 拒否(rejected) ...処理が失敗したことを意味
待機➔履行or拒否状態になる➔.then()処理➔returnが返ってきて再度待機状態になる？➔.then()/.catch()
- 確認したらpromiseStateはfulgilledだったので、二度目のpending状態かも
- つまり、returnで返ってきた値をどうするか.then()で処理しなおせばいいってことかも

## async/ awaitについて
##### asyncとは
- 非同期関数を定義する関数宣言のこと async function sample()とすることで、ここから非同期関数ですよ〜て宣言していることになる
- async関数が呼び出されるとpromiseを返す
- async関数が値をreturnすると、Promiseは戻り値をresolveする
- async関数が例外や値をthrowした場合はその値をrejectする

##### awaitとは
- async関数内でPromiseの結果が返されるまで待機(処理を一時停止)する演算子　Promiseが処理している間await演算子が指定された関数は実行されない
- 結果が返されたら実行する

