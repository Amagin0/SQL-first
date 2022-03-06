select文
-----
- 構文
  - select 列1, 列2 ... from テーブル名;
- 全ての列を取得したい場合
  - select * from テーブル名
  
ユーザー一覧の取得(※データベース名はmydbとする)
-----
```sql
use mydb;
select * from users;
```

演習(商品一覧の取得)
----
```sql
select * from products;
```

コメント
-----
1. [ -- ] (１行コメント)  
2. [/* */]  (複数行も可)   
``` 
-- select * from users;

/* select * from users; */

/* select * from users;
select * from products; */
```

列を指定してデータを取得(今回はid, last_nameを指定)
-----
```sql
select id, last_name from users;

select
  id
  last_name
from
  users;
```

演習(商品一覧、商品名(name)と価格(price)を抽出)
-----
```sql
select name, price from products;
```

列の名前を変更
-----
```sql
select name as 商品名, price as 価格 from products;
-- as は省略可能
select name 商品名, price 価格 from products;
```

列の値に対して演算を行う
-----
- テーブルから抽出した結果をそのまま使うだけでなく、計算した結果を出力できる
```sql
select
  name as 名前,
  price as 価格,
  price * 1.08 as 税込み価格
from
  products;
```