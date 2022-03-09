並び替え order by
-----
- 構文
  - `order by 列名や式 並び順, ...`
- 補足:並び順の指定
  - `asc`  ... 昇順(ascending)　※デフォルト。指定しない場合はascになる
  - `desc` ... 降順(descending)

```sql
1)商品一覧を価格が高い順に並び替える
select * from products order by price desc;

2)商品一覧を価格が低い順に並び替える
select * from products order by price asc;
-- ascは省略可能
```

order byで並び順を指定しないと、、、
-----
- どんな並び順になるかわからない
- 規則性はあるが、将来のバージョンアップで同じ並び順になることは保証されていない
- 取得するレコードの並び順が重要な場合は、**`order by`を使って、明示的に並び順を指定する**

<br>

複数の並び替え条件を指定する
-----
- カンマ区切りで複数の並び替え条件を指定できる

練習問題
-----
- 商品一覧を価格が高い順に並び替える
- 価格が同じ場合は登録順に並び替える  
※登録順...products.idが小さい順とする  
```sql
select * from products order by price desc, id asc;
-- ascは省略可能
```

日本語の商品名並び替え
-----
- 商品名で並び替えたいとき、アルファベットの商品名であればアルファベット順に並び変わる
- 日本語(全角文字)については、文字コード順になるので期待通り並び替えられない為、工夫が必要
- 代替案として、ヨミガナを入れる列を新規に作り、それを使って並び替える(検証は必要)  
<br>

演習問題１
-----
- ユーザ一覧を出力
- 生年月日が古い順に並べ、同じ場合は都道府県ID順に並べる
<details><summary>解答</summary>

```sql
select
  *
from
  users
order by
  birthday,
  prefecture_id;
```
</details>  
<br>
