## テーブルの結合(内部結合)
  
<br>

テーブルの正規化とは
-----
- テーブルを分けて情報の重複をなくしていく作業  
  
メリット
-----
- データの管理が容易になる
- データ容量の削減
  - 特別な意図(例えば、システムパフォーマンス向上のためなど)がなければテーブルは正規化する  
  
テーブルの結合とは
-----
- テーブル同士をある条件で結合することにより、正規化なしの状態を作り出すこと  
  
基本は正規化、しかしながら
-----
- パフォーマンスの問題が解消できない(できなくなりそうな)場合は、あえて非正規化することもある
- 実務で見かける可能性もある  
  
主キー(Primary Key, PK)
-----
- 一つの行を特定できる列のこと  
  
外部キー(Foreign Key, FK)
-----
- 他のテーブルとの関連付けに使う列のこと
- 外部キーは関連付けされた先のテーブルでは主キーになる  
  
内部結合(inner join)
-----
- 構文
``` sql
select
  テーブル名1.列名,
  テーブル名1.列名...
from
  テーブル名1
inner join
  テーブル名2
on テーブル名1.列名 = テーブル名2.列名;
```

練習問題１
-----
- 顧客一覧の取得
- 都道府県IDではなく都道府県名で表示
- 必要な列は、ユーザーID,名字,名前,都道府県名
```sql
select
  users.id,
  users.last_name,
  users.first_name,
  prefectures.name
from
  users
inner join
  prefectures
on users.prefecture_id = prefectures.id;
```

asを使用して、テーブル名を置き換える(asは省略可能)
-----
``` sql
select
  u.id,
  u.last_name,
  u.first_name,
  p.name
from
  users as u
  -- users u でもOK
inner join
  prefectures as p
  -- prefectures p でもOK
on u.prefecture_id = p.id;
```

inner join + where
-----
- 構文
```sql
select
  テーブル名1.列名,
  テーブル名1.列名...
from
  テーブル名1
inner join
  テーブル名2
on テーブル名1.列名 = テーブル名2.列名;
where
  絞り込み条件;
```

練習問題２
-----
- 練習問題１に加え、女性だけのデータを出力
```sql
select
  u.id,
  u.last_name,
  u.first_name,
  p.name
from
  users u
inner join
  prefectures p
on u.prefecture_id = p.id
where
  u.gender = 2;
```

select文の記述順序
-----
1. `select` 取得行(カラム)の指定
2. `from` 対象テーブルの指定
3. 結合処理
4. `where` 絞り込み条件の指定
5. `group by` グループ化の条件を指定
6. `having` グループ化した後の絞り込み条件を指定
7. `order by` 並び替え条件を指定
8. `limit` 取得する行数の制限

実行順序
-----
1. `from` 対象テーブルの指定
2. 結合処理
3. `where` 絞り込み条件の指定
4. `group by` グループ化の条件を指定
5. `having` グループ化した後の絞り込み条件を指定
6. `select` 取得行(カラム)の指定
7. `order by` 並び替え条件を指定
8. `limit` 取得する行数の制限

演習問題
-----
- 2017年1月の、東京都に住むユーザーの注文情報一覧の出力
- 取得カラム(注文id, 注文日時, 注文合計金額, ユーザーid, 名字, 名前)

<details><summary>解答</summary>

```sql
select
  o.id,
  o.order_time,
  o.amount,
  u.id user_id,
  u.last_name,
  u.first_name
from
  orders o
inner join
  users u
on o.user_id = u.id
where
  u.prefecture_id = 13
  and extract(year_month from o.order_time) = 201701;
  -- and o.order_time >= '2017-01-01 00:00:00'
  -- and o.order_time < '2017-02-01 00:00:00'
-- order by order_id;
```

</details>  
<br>
