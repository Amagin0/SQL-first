新規データを追加する(insert into)
-----
- 構文
- 注意点：列リストと、values句の値リストは、列数が一致している必要がある
```sql
insert into
  テーブル名(列1, 列2, 列3...)
values
  (値1, 値2, 値3...);
```
例
-----
```sql
insert into
  products(name, price)
values
  ('新商品A', 1000);
```

列を省略してinsert
-----
- values句に列の定義順、カンマ区切りで値を設定
- ただし、列を省略するには条件がある
  - テーブルの全列に対して、値を指定する必要がある
```sql
insert
  products
values
  (1002, '新商品B', 2000);
```

複数行を一度にinsertする
-----
- 構文
```sql
insert into
  テーブル名(列1, 列2, 列3...)
values
  (値1, 値2, 値3...),
  (値1, 値2, 値3...),
  (値1, 値2, 値3...);
-- Oracleでは使えない
```

例
-----
```sql
insert into
  products(name, price)
values
  ('新商品C', 3000),
  ('新商品D', 4000),
  ('新商品E', 5000);
```

レコードの更新(update)
-----
- 構文
```sql
update テーブル名 set 列1 = 値1, [列2 = 値2...]
[where 条件式];
```

例
-----
- 全商品10％引き
```sql
-- set sql_safe_updates = 0;
-- セーフティが掛けられている場合
update products set price = price * 0.9;
```

特定の条件に合致するデータを更新
-----
- 商品idが3の商品名を、「SQL入門」に変更
```sql
update products set name = 'SQL入門' where id = 3;

/* 複数のものを同時更新 */
update products set name = 'SQL入門1',price = 1000 where id = 3;
```

更新条件にサブクエリを使う
-----
- 累計販売数が10を超えている商品は価格を5％UPする
```sql
update
  products
set
  price = price * 1.05
where
  id in
  (
  select
    product_id
  from
    order_details
  group by
    product_id
  having
    sum(product_qty) >= 10
  );
```

行の削除(delete)
-----
- 構文
```sql
delete from テーブル名 [where 削除条件];
```

練習問題
-----
- 商品とカテゴリのひも付きを削除する
```sql
delete from products_categories;
```

注意点
-----
- `delete`で削除したデータは、基本的に元に戻せない
- 大量のデータを`delete`するときに予想外に時間がかかる場合がある

条件を指定して行の削除
-----
- 商品ID 1001の削除
```sql
delete from products where id = 1001;
```

注意点
-----
- where句を指定し忘れると、productsテーブル全体が削除対象となる

削除条件にサブクエリを使う
-----
- １つも売れていない商品の削除
- ※DB2では動作しない
```sql

delete
from
  products
where
  id not in
  (
  select
    product_id
  from
    order_details
  group by
    product_id
  );
```