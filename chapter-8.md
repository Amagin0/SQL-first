## テーブルの足し算

集合演算子(union)
-----
- ベースとなるselectの結果に、unionの後に記載したselectの結果を足し算する
- 構文
```sql
select
  列１
from
  テーブル１
union
select 
  列２
from
  テーブル２
```

注意点
-----
- テーブル１とテーブル２で列数を合わせる必要がある
- 同じ入りにあるカラムのデータ型は一致している必要がある

練習問題１
-----
- ユーザーとアドミンユーザーを足し合わせた一覧の出力

```sql
select
  email,
  last_name,
  first_name,
  gender
from
  users
union 
-- union all...重複したものが削除されない
select
  email,
  last_name,
  first_name,
  gender
from
  admin_users;
```

句について
-----
- where, group by, havingといった句をつけることができる
- ただし、order by だけは、全体として最後に一つしか記述できない

練習問題２
-----
- usersテーブルから男性だけ、admin_usersテーブルからは女性だけ抜き出す
- unionした結果は性別順に並び替える

```sql
select
  email,
  last_name,
  first_name,
  gender
from
  users
where 
  gender = 1
union 
select
  email,
  last_name,
  first_name,
  gender
from
  admin_users
where 
  gender = 2
order by gender;
```