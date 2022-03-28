データベースの作成
-----
- 構文
```sql
create database データベース名;
```

命名ルール
-----
### データベースオブジェクトに使ってよい文字
- 半角のアルファベット...a,b,cなど
- 半角の数字...0,1,2など
- アンダースコア... _
  - ただし、名前の最初は半角のアルファベットとする

具体例
-----
- 〇 users
- 〇 mydb2
- 〇 products_categories
- ✕ _users
- ✕ 1users

テーブルの作成
-----
- `books`という名前のテーブルを作成
- 列
  - id...行のid
  - title...本のタイトルを格納

```sql
use book_store;
-- 使用するデータベースの選択

show tables;
-- 全テーブルの確認

create table books(id int not null auto_increment primary key, title varchar(255) not null);

show columns from books;
-- 作成したカラムの確認
```
- 列名 `id`
  - `int` : 整数型
  - `not null` : `null`を許可しない
  - `auto_increment` : `id`を自動的にふる
  - `primary key` : 主キーの設定
- 列名 `title`
  - `varchar(255)` : 最大255文字の可変長文字列型
  - `not null` : `null`を許可しない

列の追加
-----
- 書籍を管理する`books`テーブルに、価格を管理する列を追加
  - 列名：`price`、データ型：`int`

```sql
alter table books add price int after id;
```

列名の変更
-----
- 構文
```sql
alter table テーブル名 change 旧列名 新列名 データ型;
```

例題
-----
- `price`を`unit_price`に名前を変更
```sql
alter table books change price unit_price int;
```

列の削除
-----
- 構文
```sql
alter table テーブル名 drop 列名;
```

例題
-----
- `unit_price`を削除
```sql
alter table books drop unit_price;
```

テーブルの削除
-----
- 構文
```sql
drop table テーブル名;
```

例題
-----
- `books`テーブルを削除
```sql
drop table books;
```

データベースの削除
-----
- 構文
```sql
drop database データベース名;
```

例題
-----
- `book_store`データベースを削除する
```sql
drop database book_store;
```

操作の注意点
-----
### 注意事項：`alter table`, `drop table`, `drop database`など
- 操作は基本的には取り消せない
  - 特に、実務において本番環境を操作するときは、サービスをメンテナンスモードにして、バックアップを取ってから、`alter table`などを実行するのが安全
- 想定外に時間がかかりシステムトラブルになる場合もある
  - テスト環境で`alter table`のテストをして問題点を洗い出してから、本番環境で実行するのがおすすめ