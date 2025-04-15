---
title: "JDBCの更新可能カーソルは自動コミットを無効にしないといけない"
emoji: "🔄"
type: "tech"
topics:
  - "Java"
  - "JDBC"
  - "DB2"
published: true
published_at: "2025-04-15 18:00"
---

JDBC で `ResultSet.deleteRow()` を使ったところ、`SQLCODE=-508`（カーソルが行に位置していない）というエラーが発生しました。  
原因は、**自動コミットが有効なまま更新可能カーソルを使っていたこと**でした。  
エラーコードから、カーソルとレコードの不整合かと思いましたが、実際には違ったため、注意しておくべき観点としてメモ。

## 結論：`autoCommit = false` にする

更新可能カーソルで更新や削除は、以下の流れで行います。

- `SELECT ... FOR UPDATE` でロック取得  
- `rs.next()` でカーソルを行に位置付け  
- `rs.deleteRow()` や `rs.updateRow()` を実行  

ところが、接続が自動コミット (`autoCommit=true`) のままだと、JDBCドライバが即時コミットを行い、カーソルが維持されなくなってしまうようです。結果として `deleteRow()` 時に `-508` エラーが出ました。

## エラー内容

```plaintext
com.ibm.db2.jcc.am.SqlException: The cursor specified in the UPDATE or DELETE statement is not positioned on a row.
SQLCODE=-508, SQLSTATE=24504, DRIVER=4.25.13
```

※DB2 11.5 / JDBC (jcc) ドライバ

## 再現コードとテーブル

```sql
CREATE TABLE TSAMPLE (
  AAA VARCHAR(10),
  BBB VARCHAR(10)
);

INSERT INTO TSAMPLE (AAA, BBB) VALUES ('001', 'DEF');
```

```java
PreparedStatement ps = connection.prepareStatement(
    "SELECT AAA, BBB FROM TSAMPLE WHERE AAA = ? FOR UPDATE",
    ResultSet.TYPE_SCROLL_INSENSITIVE,
    ResultSet.CONCUR_UPDATABLE
);
ps.setString(1, "001");
ResultSet rs = ps.executeQuery();
if (rs.next()) {
    rs.deleteRow();  // ← autoCommit=true だとここで -508 エラー
}
```

## 修正後コード

```java
Connection conn = dataSource.getConnection();
conn.setAutoCommit(false);  // ← これが必要

PreparedStatement ps = conn.prepareStatement(
    "SELECT AAA, BBB FROM TSAMPLE WHERE AAA = ? FOR UPDATE",
    ResultSet.TYPE_SCROLL_INSENSITIVE,
    ResultSet.CONCUR_UPDATABLE
);
ps.setString(1, "001");
ResultSet rs = ps.executeQuery();
if (rs.next()) {
    rs.deleteRow();  // 成功
}
conn.commit(); // 明示的にコミットしないとロールバックされる点にも注意
```

これでエラー解消しました。
