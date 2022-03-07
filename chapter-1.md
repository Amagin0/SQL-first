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

where文
-----
- 構文
  - select 列 from テーブル名 where 条件;

価格が9800円以上の商品一覧、列は名前と価格。ヘッダー名はname, price
-----
```sql
select name, price from products where price >=9800;
```

比較演算子
-----
- 「=」 ... 等しい
- 「<>」,「!=」 ... 等しくない
- 「>」 ... より大きい
- 「<」 ... より小さい 
- 「>=」... 以上
- 「<=」... 以下
- 「in」... ある値が値セット内に含まれているかどうか
- 「not in」 ... 値が値セット内に含まれていないかどうか
- 「is null」 ... 値がnull
- 「is not null」 ... 値がnullではない
- 「like」 ... パターンマッチング(あいまい検索)
- 「between ... and ...」 ... 値が値の範囲内に含まれているか
```sql
-- 1) productsテーブルから全件取得
select * from products;

-- 2) idが1の行を取得
select * from products where id = 1;

-- 3) 名前が「商品0003」の行を取得
select * from products where name = '商品0003';

-- 4) priceが1000より大きい行を取得
select * from products where price > 1000;

-- 5) priceが1000より小さい行を取得
select * from products where price < 1000;

-- 6) priceが100でない行を取得
select * from products where price != 100;
select * from products where price <> 100;

-- 7) idが1か2か3の行を取得
select * from products where id in(1,2,3);

-- 8) idが1か2か3ではない行を取得
select * from products where id not in(1,2,3);

-- 9) priceがnullではない行を取得
select * from products where price is not null;

-- 10) priceがnullの行を取得
select * from products where price is null;

-- 11) priceが1000から1900の行を取得
select * from products where price between 1000 and 1900;

-- 12) priceが1000から1900の行を取得(andを使っても書ける)
select * from products where price >= 1000 and price <= 1900;

-- 13) 価格が1000円または2000円の行を取得
select * from products where price = 1000 or price = 2000;
```

パターンマッチングによる絞り込み
-----
- 構文
  - select 列 from テーブル名 where 列名 like ワイルドカード文字;

ワイルドカード文字とは
-----
- ワイルドカード文字で文字列のパターンを指定する
  - '%' ... 0文字以上の任意の文字列
  - '_' ... 任意の1文字
- 例  
  1. '中%' → '中'で始まる文字列
  2. '%中%' → '中'を含む文字列
  3. '%中' → '中'で終わる文字列
  4. '__中' → 何かしらの２文字から始まり、'中'で終わる文字列
```sql
-- 1) 中から始まる名字のユーザ一覧
select * from users where last_name like '中%';

-- 2) 中を含む名字のユーザ一覧
select * from users where last_name like '%中%';

-- 3) 子で終わる名前のユーザ一覧
select * from users where first_name like '%子';

-- 4) 子で終わる３文字の名前のユーザ一覧
select * from users where first_name like '__子';
```

limit句
-----
- 構文
  - select 列 from テーブル名 limit [オフセット,]最大取得件数;
```sql
-- データを10件取得
select * from products limit 10;

-- 明示的に最初からデータを取得
select * from products limit 0, 10;

-- 11番目から10件取得
select * from products limit 10, 10;
```

演習
-----
- 男性ユーザの一覧を取得
- 取得する件数は10件
- 取得する行は、id, last_name, gender
```sql
select id, last_name, gender from users where gender = 1 limit 10;
```
