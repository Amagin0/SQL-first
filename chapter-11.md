条件分岐(case式)
-----
- case式を使うと、SQLで **場合分け** を記述することが出来る
- 場合分けのことを、**条件分岐** という
- 成績表を例にとると
  - 90点以上だったら、Aと出力
  - 70点以上だったら、Bと出力
  - 60点以上だったら、Cと出力
  - 60点未満だったら、Dと出力

case式
-----
- 構文
```sql
case
  when 条件式１ then 値１ -- 条件式１がtrue(真)ならば値１
  [when 条件式２　then 値２...]
  [else 値３]
end
-- []の部分は省略可能
```
具体例
-----
```sql
case
  when x >= 10 then 'A'
  when x >= 5 then 'B'
  else 'C'
end

-- xは変数
-- x = 10 ならば、case式の結果は'A'
```

練習問題１
-----
- ユーザーの累計注文数でランク分け
- 5回以上：A、２回以上：B、１回：C、０回は出力しない
- 必要情報はユーザーID、累計注文回数、ユーザーランク
```sql
select
  u.id user_id,
  count(*) num,
  case
	when count(*) >= 5 then 'A'
    when count(*) >= 2 then 'B'
    else 'C'
  end user_rank
from
  users u
inner join
  orders o
on u.id = o.user_id
group by u.id;
```
練習問題２
-----
- 練習問題１に追加で、ランク順に並び替え
```sql
select
  u.id user_id,
  count(*) num,
  case
	when count(*) >= 5 then 'A'
    when count(*) >= 2 then 'B'
    else 'C'
  end user_rank
from
  users u
inner join
  orders o
on u.id = o.user_id
group by u.id
order by user_rank;
```
取得した値nullを０に置き換える
-----
```sql
select
  p.id,
  p.name,
  case
    when sum(od.product_qty) is null then 0
    else sum(od.product_qty)
  end num
from
  products p
left outer join
  order_details od
on p.id = od.product_id
group by p.id
```

演習問題
-----
- 全商品を累計販売個数でランク分け(ランク順に並び替える)
- 20個以上：A、10個以上：B、10個未満：C
- 必要な列は、商品ID、商品名、販売個数、ランク

<details><summary>解答</summary>

```sql
select
  p.id,
  p.name,
  sum(product_qty),
  case
    when sum(product_qty) >= 20 then 'A'
    when sum(product_qty) >= 10 then 'B'
    else 'C'
  end product_rank
from
  products p
left outer join
  order_details od
on p.id = od.product_id
group by 
  p.id
order by
  product_rank;
```
</details>  