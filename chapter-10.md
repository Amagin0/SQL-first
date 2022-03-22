サブクエリとは
-----
- ある問い合わせの結果に基づいて、異なる問い合わせを行う仕組み
- 複雑な問い合わせが出来る
- where句の中で使うことが多い
- where句以外にも、select句、from句、having句など様々な場所で利用できる

サブクエリを使うと
-----
- 日々の業務改善のデータ分析に役立つデータが、データベースから直接SQLで取り出せる
  - 全商品の平均単価より、高い商品を取得
  - 商品別の平均販売数量よりも、多く売れている日を取得
  - 商品カテゴリごとに、平均単価を取得
  - 2017年12月に、商品を購入して"いない"ユーザーを取得　など

サブクエリ(where句)
-----
- 構文
```sql
select
  列名
from
  テーブル名
where
  列名 演算子( select 列名 from テーブル名２ ...);
```

復習：not in 演算子
-----
- ある値が値セットないに含まれて **"いない"** かどうか
- 例) idが1か2か3ではない行を取得
```sql
select * from products where id not in (1,2,3);
```

練習問題
-----
- 2017年12月に、商品を購入していないユーザー一覧の取得
- 必要な情報は、user_id, last_name, email
```sql
select
  id,
  last_name,
  email
from
  users 
where id not in (
  select
    user_id
  from
    orders
  where
    order_time >= '2017-12-01 00:00:00'
    and order_time < '2018-01-01 00:00:00');
```

演習問題
----
- 2017年12月に、商品を購入したユーザー一覧の取得
- 必要な情報は、user_id, last_name, email

<details><summary>解答</summary>

```sql
select
  id,
  last_name,
  email
from
  users
where id in (
  select
    user_id
  from
    orders
  where
    order_time >= '2017-12-01 00:00:00'
    and order_time < '2018-01-01 00:00:00');
```
</details>  
<br>

スカラ・サブクエリ
-----
- 必ず１行１列だけの戻り値を返す、サブクエリのこと
  - **スカラ** とは **単一** のという意味
  - 絶対にサブクエリが複数行を返さないようにする

練習問題１
-----
- 全商品の平均単価よりも、単価が高い商品の一覧の取得
```sql
select
  *
from
  products
where 
  price > 
    (
      select
        avg(price)
      from
        products
    );
```
練習問題２
-----
- 練習問題１に加え、商品単価の高い順に並び替え
- 商品単価が同じ場合は、登録順(id昇順)に並び替え
```sql
select
  *
from
  products
where 
  price > 
    (
      select
        avg(price)
      from
        products
    )
order by
  price desc, id asc;
  -- ascは省略可能
```
