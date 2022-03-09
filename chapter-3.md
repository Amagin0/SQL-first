集約関数とは
-----
- SQLでテーブルの値を集計する為に使う
- 関数(function)とは、様々な計算をパッケージ化したもの

主な集約関数
-----
- `sum(expr)`...合計値
- `avg(expr)`...平均値
- `min(expr)`...最小値
- `max(expr)`...最大値
- `count(expr)`...行数  
<br>

- `expr`...expression:式
  - exprの部分は引数(parameter)を入力する

合計値を求める sum集約関数
-----
- 構文
  - `sum(expr)`...exprの合計値を返す

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
  - `avg(expr)`...exprの平均値を返す
```sql
全商品の平均価格を求める
select avg(price) from products;
```

最小値を求める min集約関数
-----
- 構文
  - `min(expr)`...exprの最小値を返す
```sql
取り扱っている商品の価格の最小値を求める
select min(price) from products;
```

最大値を求める max集約関数
-----
- 構文
  - `max(expr)`...exprの最大値を返す
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

対象行の行数を数える count集約関数
-----
- 構文
  - `count(expr)`
    - `expr` の数をカウントする
    - `count(*)` とすると、テーブルの行数を取得できる
```sql
1) 全ユーザ数を求める
select count(*) from users;

2) 女性ユーザの数を求める(gender:1が男性, gender:2が女性)
select count(*) from users where gender = 2;
```

練習問題
-----
- 2017年1月にアクセスした、ユニークユーザー数(ECサイト登録ユーザのみ)
  - ユニークアクセスユーザーとは、決まった集計期間内にアクセスしたユーザ数
  - あるユーザーAが10回アクセスしたとしても1回と数える
```sql
select 
	count(distinct user_id)
from 
	access_logs 
where 
	request_month = '2017-01-01';
```

データをグループ化 group by
-----
- 構文
  - `select 列名 from テーブル名 group by 列名;`
    - `count` などを使ってデータを集計するときに、グループ単位で集計が行われる
    - `group by` で指定する列名によって、グループ化される
```sql
都道府県ごとのユーザ数を求める
select 
	prefecture_id, 
  count(*) 
from 
	users
group by 
	prefecture_id;
```

練習問題
-----
- 2017年の月別ユニークユーザー数を求める
```sql
select 
	request_month,
  count(distinct user_id)
from 
	access_logs
where
	request_month >= '2017-01-01'
  and request_month < '2018-01-01'
group by
	request_month;
```

集約結果をさらに絞り込む having句
-----
- `where` と同様に、条件に合致する行だけに絞り込める
- `having` は、テーブルのデータを集約した結果に対して条件式を適用する場合に利用する  

```sql
select
  列
from
  テーブル名
where
  条件式
group by
  列
having
  条件式;
```
記述順序
-----
- `select` → `from` → `where` → `group by` → `having`
- ポイントとして、`having` は `group by` の後に書く

```sql
月間ユニークユーザー数が630人以上の月を一覧で取得する
select 
	request_month,
  count(distinct user_id)
from 
	access_logs
where
	request_month >= '2017-01-01'
  and request_month < '2018-01-01'
group by
	request_month;
having
  count(distinct user_id) >= 630;
```

select文の記述順序と実行順序
-----
1. `select` ...取得行(カラム)の指定
2. `from` ...対象テーブルの指定
3. `where` ...絞り込み条件の指定
4. `group by` ...グループ化の条件を指定
5. `having` ...グループ化した後の絞り込み条件を指定
6. `order by` ...並び替え条件を指定
7. `limit` ...取得する行数の制限

記述順序と実行順序の違い
-----
- ポイント... `select`の記述順序と実行順序が異なる  
<br>

記述順序 / 実行順序
1. `select` / `from` 
2. `from` / `where`
3. `where` / `group by`
4. `group by` / `having`
5. `having` / `select`
6. `order by` / `order by`
7. `limit` / `limit`

演習問題１
-----
- 全てのアクセスログ一覧を出力
<details><summary>解答</summary>

```sql
select * from access_logs;
```
</details>  
<br>

演習問題２
-----
- アクセスログの出力は、2017年１月～６月まで
<details><summary>解答</summary>

```sql
select 
	* 
from 
	access_logs
where
	request_month >= '2017-01-01'
    and request_month < '2017-07-01';
```
</details>  
<br>

演習問題３
-----
- 演習問題２に追加で月ごとのリクエスト数を出力
<details><summary>解答</summary>

```sql
select 
  request_month,
	count(*) 
from 
	access_logs
where
	request_month >= '2017-01-01'
    and request_month < '2017-07-01'
group by
  request_month;
```
</details>  
<br>

演習問題４
-----
- さらにアクセス数が1000以上の月だけを出力
<details><summary>解答</summary>

```sql
select 
  request_month,
	count(*) 
from 
	access_logs
where
	request_month >= '2017-01-01'
    and request_month < '2017-07-01'
group by
  request_month
having
  count(*) >= 1000;
```
</details>  
<br>
