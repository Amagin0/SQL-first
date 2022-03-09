## 色々な算術演算  
<br>

SQLで使える主な算術演算子
----
- `+`:足し算
- `-`:引き算
- `*`:掛け算
- `/`:割り算
- `%`:余り
```sql
/* 足し算 */
select 10 + 3;
-- 13

/* 引き算 */
select 10 - 3;
-- 7

/* 掛け算 */
select 10 * 3;
-- 30

/* 割り算 */
select 10 / 3;
-- 3.3333

/* 余り */
select 10 % 3;
-- 1
```

nullを含む演算
-----
- nullを含んだ **計算結果はnullになる** ので注意
```sql
select 10 + null;
select 10 - null;
select 10 * null;
select 10 / null;
select 10 % null;
→ 全て null となる
```

絶対値の取得
-----
```sql
/* 10の絶対値 */
select abs(10);
-- 10

/* -10の絶対値 */
select abs(-10);
-- 10

/* 0の絶対値 */
select abs(0);
-- 0
```

四捨五入  round関数
-----
- 構文
  - `round(対象の数値, 丸めの桁数)`
```sql
/* 小数第一位で四捨五入 */
round(10.555, 0)
-- 11

/* 小数第二位で四捨五入 */
round(10.555, 1)
-- 11.6

/* 小数第三位で四捨五入 */
round(10.555, 2)
-- 11.56
```

練習問題
-----
- 商品価格一覧を作成するときに税込み価格を出力
- 小数第一位で四捨五入する

```sql
select id,name,round(price * 1.08, 0) from products;
```  
<br>

文字列の演算
-----
- 文字列連結 `a || b` ... 文字列aと文字列bを連結
- ただし、MySQL, SQL Server2012以降では、concat関数を仕様
  - `concat(文字列1, 文字列2, 文字列3...)`

練習問題
-----
- ユーザ一覧を山田 太郎さんというように、名字+スペース+名前+さんのフォーマットで出力
```sql
select concat(last_name, ' ', first_name, 'さん') from users;
```  
<br>

演習問題１
-----
- メルマガ送信用のリスト作成
- 出力項目は、
  - 宛名「名字 + さん」
  - メールアドレス
    - 例)中村さん、nakamura@example.com
- 女性だけに送信したい

<details><summary>解答</summary>

```sql
select 
  concat(last_name, 'さん'),
  email,
from
  users
where
  gender = 2;

```
</details>  
<br>

主な日付と時刻の関数や演算子
-----
- 現在の日付 ...`current_date`
- 現在の時刻 ...`current_timestamp`
- n日後の日付 ...`d + n`
- n日前の日付 ...`d - n`
- x時間後の時刻 ... `interval 'x hour'`
- x時間前の時刻 ... `- interval 'x hour'`
- `extract` ... 日付や時刻の特定の部分(年や月)までを取り出す

```sql
/* 現在の日付 */
select current_date();

/* 現在の時刻 */
select current_timestamp();

/* 3日後の日付 */
select current_date() + interval 3 day;
-- 月またぎの処理を考慮

/* 3日前の日付 */
select current_date() - interval 3 day;
-- 月またぎの処理を考慮

/* 6時間後の時刻 */
select current_timestamp() + interval 6 hour;

/* 6時間前の時刻 */
select current_timestamp() - interval 6 hour;

/* extract ... 日付や時刻の特定の部分(年や月)までを取り出す */
/* ordersテーブルから注文日時(order_timeカラム)が、2017年1月のレコードを取得する */
select * from orders where extract(year_month from order_time) = 201701;

/* ordersテーブルから注文日時(order_timeカラム)が、2017年のレコードを取得する */
select * from orders where extract(year from order_time) = 2017;

/* ordersテーブルから注文日時(order_timeカラム)が、1月のレコードを取得する */
select * from orders where extract(month from order_time) = 1;
```
