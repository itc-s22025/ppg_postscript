# Database関連
1. コンテナに入る
``` terminal
$ docker exec -it database /bin/sh
```
2. コンテナ内でデータベースにログイン
``` terminal
$ psql -U postgres
```
- -U ... userの指定 docker-compose.ymlのenvironmentで作成したやつ
- -d ... database名の指定　ここではやってない　指定しない場合のデフォルト：OSのユーザー名
- -h ... ホストの指定　ここではやってない　デフォルト: localhost(127.0.0.1)
- -p ... ポート番号の指定　ここではやってない　デフォルト：5432
- environmentでpassword指定しなければ、本来ならここでパスワード要求されるっぽい(たぶんそのときつくる)
- ログアウトしたいときは
``` terminal
\q
```
### テスト用のDBつくってみる
1. postgres=#になったら
``` terminal
create database test;
```
CREATE TESTてでてくるはず
- createできたか確認したいとき：
``` terminal
\l
```

2. テーブル作成してみる
``` terminal 
create table users(id varchar(20), password varchar(40), username varchar(20) primary key(id));
```
- createできたか確認したいとき：
``` terminal
\d
```
※create table userにするとシンタックスエラーになる(userて名前のテーブルは作成できないらしい)

3. データをインサート(作成)してみる
``` terminal
insert into users(id, password, username) values('user1', 'password1', 'ユーザー1');
```
確認⇩
``` terminal 
select * from users;

//こうなる
  id   | password  | username  
-------+-----------+-----------
 user1 | password1 | ユーザー1
(1 row)
```

## 確認コマンドとか
postgresログイン後、ユーザ権限とか確認したいとき
``` terminal 
select * from pg_user;
```
