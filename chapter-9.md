ビューとは
-----
- データそのものを保存するのではなく、データを取り出すSELECT文だけを保存する
- データベースユーザーの利便性を高める道具
- SQLの観点からみると、テーブルと同じもの
- ビューを使うと、必要なデータが複数のテーブルにまたがる場合などの複雑な集約が行いやすくなる
- よく使うSELECT文はビューにして使いまわすことが出来る

テーブルとビューの違い
-----
- テーブル
  - 実際のデータを保存
- ビュー
  - ビューの中にはselect文が保存される
  - ビュー自体はデータを持たない

ビューの制限
-----
- order by句が使えない
- ビューに対する更新は不可能ではないが制限がある  
  1. select句に`distinct`が含まれていない
  2. from句に含まれるテーブルが一つだけ
  3. `group by`句を使用していない
  4. `having`句を使用していない
- ビューとテーブルの更新は連動して行われるため、集約されたビューは更新不可。

ビューのメリット
-----
- データを保存しないので、記憶装置の容量を節約できる
- よく使うselect文をビューにしておくことで、使いまわしが出来る

ビューのデメリット
-----
- パフォーマンス低下を招く場合がある

ビュー(create view)
-----
- 構文
```sql
create view
ビュー名(<ビューの列名１>,<ビューの列名２>)
as
<select文>
```

練習問題
-----
- 都道府県別のユーザー数の取得
```sql
create view 
  prefecture_user_counts(name,count)
as
select
  p.name name,
  count(*) count
from
  users u
inner join
  prefectures p
on u.prefecture_id = p.id
group by
  u.prefecture_id;
```

使い方
-----
```sql
select
  name,
  count
from
  prefecture_user_counts;
```

ビューの削除(drop view)
-----
- 構文
```sql
drop view ビュー名;
```

練習問題
-----
- 先ほど作ったviewの削除
```sql
drop view prefecture_user_counts;
```

