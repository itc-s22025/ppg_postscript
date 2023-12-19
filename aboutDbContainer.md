# Database用のDockerコンテナ
- コンテナは削除(STOP?)すると中のデータが消えるので、永続的に保持したいデータはデータボリュームの中に保管する
- コマンド： $ docker volume create (ボリューム名)
1. なので、とりあえずボリューム作成しておく
``` terminal
$ docker volume create db_data
```

2. docker-compose.ymlを編集
``` terminal
# docker-compose.yml
# services: dbとvolumesを追記

version: '3'

services:
  db:
    container_name: database
    image: postgres:15.3
    volumes:
      - db_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_DATABASE=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
      - POSTGRES_ROOT_PASSWORD=root

  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3001:3001"
    restart: always

volumes:
  db_data:
```
- postgreSQLのポート番号が5432らしい
- ports部分...ホストマシンのポート番号：コンテナのポート番号
- ppgフロントのポート：3000, app-docker間のポート：3001, Backendポート：3002にしてる？かも

3. docker-compose upでビルドする？？
``` terminal
$ docker-compose up
```
4. ↑がなんか終わったぽかったらctrl+Cで中断してバックグラウンドでやりなおした
``` terminal
$ docker-compose up -d
```

## Dockerコンテナの中に入る
- DB操作のときは多分コンテナ内部に入ってDB操作する

``` terminal 
$ docker exec -it database(※DBコンテナの名前) /bin/sh
```
- postgresのコマンド：psql
- 現時点ではdocker-compose.ymlのenvironmentでusernameをpostgresにしているので⇩
``` terminal
# psql -U postgres
```
コンテナからでるときは
``` terminal 
# exit
```
コンテナの停止
``` terminal 
$ docker-compose stop database(※コンテナ名)
```
ていうかBackend/DataBase内に作成したinit.jsてなに？
