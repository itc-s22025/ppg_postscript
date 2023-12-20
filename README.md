# ppg用の追記諸々
## Docker作成関連
- りんがやったこと

1. 最初にとりあえず全体のDockerコンテナつくった: [Dockerコンテナ作成](https://github.com/itc-s22025/ppg_postscript/blob/main/aboutFirstMakingDocker.md)
2. データベースだけのコンテナつくった: [DBのDockerコンテナ作成](https://github.com/itc-s22025/ppg_postscript/blob/main/aboutDbContainer.md)
3. データベースのテーブルとかデータが作れるかテストした： [Database関連](https://github.com/itc-s22025/ppg_postscript/blob/main/aboutDb.md)


## 起動関連
ppgディレクトリ下
``` terminal 
$ npm run dev &
```
Backendディレクトリ下
``` terminal 
$ npm start &
```
Dockerコンテナをバックグラウンドで起動
``` terminal 
$ docker-compose up -d
```
Dockerコンテナ稼働中一覧確認
``` terminal 
$ docker ps
```
Dockerコンテナ停止
``` terminal 
$ docker stop (コンテナ名)
```
Dockerコンテナ再起動
``` terminal
$ docker container start (コンテナ名)
```

## メモ
- [思ったこととか](https://github.com/itc-s22025/ppg_postscript/blob/main/memo.md)
