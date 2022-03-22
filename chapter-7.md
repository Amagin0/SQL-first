## テーブルの結合(外部結合)

外部結合とは
-----
- 片方のテーブルの情報が全て出力されるテーブルの結合
- 外部結合は欠落のあるデータを取り扱う結合(nullが存在する)

外部結合(left outer join)
-----
- 構文
```sql
select
  テーブル名1.列名,
  テーブル名1.列名...
from
  テーブル名1
left outer join
  テーブル名2
on テーブル名1.列名 = テーブル名2.列名;
```

left outer join / right outer join
-----
- `left outer join` ...左側(from句で最初に書いたテーブル)をマスタとする
- `right outer join` ...右側(from句で後に書いたテーブル)をマスタとする

例文
-----
```sql
select
  u.last_name last_name,
  u.id user_id,
  o.user_id order_user_id,
  o.id order_id
from
  users u
left outer join
-- left joinと略せる
  orders o
on u.id = o.user_id
order by u.id;
```

練習問題１
-----
- すべての商品について、販売個数一覧の出力
```sql
select
  p.id,
  p.name,
  sum(od.product_qty) num
from
  products p
left join
  order_details od
on p.id = od.product_id
group by p.id;
```

## 多対多の関係を含む結合
  
練習問題
-----
- 商品ID=3に紐づく商品カテゴリ名をすべて取得

```sql
select
  p.id product_id,
  p.name product_name,
  c.name category_name
from
  products p
inner join 
  products_categories pc
on p.id = pc.product_id
inner join
  categories c
on pc.category_id = c.id
where
  p.id = 3;
```