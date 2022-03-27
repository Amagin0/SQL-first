応用問題１
-----
- 全期間での平均客単価を求める
- 小数第一位で四捨五入

<details><summary>解答</summary>

```sql
select
  round(avg(amount)) avg
from
  orders;
```
</details>  
<br>

応用問題２
-----
- 月別での平均客単価を求める
- 小数第一位で四捨五入

<details><summary>解答</summary>

```sql
select
  date_format(order_time, '%Y%m') order_year_month,
  round(avg(amount)) avg_customer_spend
from
  orders
group by
  date_format(order_time, '%Y%m')
order by
  order_year_month;
```
</details>  
<br>

応用問題３
-----
- 都道府県別平均客単価を求める
- 必要な列(都道府県ID、都道府県名、平均客単価(小数第一位で四捨五入))
- 都道府県ID昇順

<details><summary>解答</summary>

```sql
select
  p.id prefecture_id,
  p.name,
  round(avg(amount)) avg_customer_spend
from
  orders o
inner join
  users u
  on o.user_id = u.id
inner join
  prefectures p
  on u.prefecture_id = p.id
group by
  p.id
order by
  p.id;
```
</details>  
<br>

応用問題４
-----
- 都道府県別、月別平均客単価を求める
- 必要な列(都道府県ID、都道府県名、年月、平均客単価(小数第一位で四捨五入))
- 都道府県ID昇順、年月昇順

<details><summary>解答</summary>

```sql
select
  p.id prefecture_id,
  p.name,
  date_format(o.order_time, '%Y%m') order_year_month,
  round(avg(amount)) avg_customer_spend
from
  orders o
inner join
  users u
  on o.user_id = u.id
inner join
  prefectures p
  on u.prefecture_id = p.id
group by
  p.id, order_year_month
order by
  p.id, order_year_month;
```
</details>  
