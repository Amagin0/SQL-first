集約関数とは
-----
- SQLでテーブルの値を集計する為に使う
- 関数(function)とは、様々な計算をパッケージ化したもの

主な集約関数
-----
- sum(expr)...合計値
- avg(expr)...平均値
- min(expr)...最小値
- max(expr)...最大値
- count(expr)...行数  
<br>
- expr...expression:式
  - exprの部分は引数(parameter)を入力する

合計値を求める sum集約関数
-----
- 構文
  - sum(expr)...exprの合計値を返す

```sql
2017年1月の合計売上金額を求める
1)orderテーブル全体を表示
select * from orders;

2)2017年1月の売り上げを表示
select
	*
from
	orders
where
	order_time >= '2017-01-01 00:00:00'
	and order_time < '2017-02-01 00:00:00';

3)合計金額を求める
select
	sum(amount)
from
	orders
where
	order_time >= '2017-01-01 00:00:00'
	and order_time < '2017-02-01 00:00:00';
```

平均値を求める avg集約関数
-----
- 構文
  - avg(expr)...exprの平均値を返す
```sql
全商品の平均価格を求める
select avg(price) from products;
```

最小値を求める min集約関数
-----
- 構文
  - min(expr)...exprの最小値を返す
```sql
取り扱っている商品の価格の最小値を求める
select min(price) from products;
```

最大値を求める max集約関数
-----
- 構文
  - max(expr)...exprの最大値を返す
```sql
取り扱っている商品の価格の最大値を求める
select max(price) from products;
```

集約関数におけるnullの扱い
-----
- 集約関数では、基本的に **nullは無視** される
- nullを許可する場合、0とnullの違いや、nullと空文字の違いを意識する必要がある。
- nullの変わりに数値:0や、文字列:''空文字が使えないのか検討してみる
```sql
10
null
20
avg()　
=>15
-- 3つあるので平均10になると思うが、nullは分母に含まれない
```
