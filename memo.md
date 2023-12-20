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
      .then(response => response.json())
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
  res.json(sampleData);
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```
