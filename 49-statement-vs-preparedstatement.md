# Statement, PreparedStatement and CallableStatement

在JDBC的世界裡, 我們常常會透過Statement/PreparedStatement來對DB進行一些操作, 這篇主要是想記錄一下這兩者之間的差異.

### Statement

* 通常用於執行靜態的SQL語句並且獲得SQL產生之結果, 通常會看到以下三種執行SQL語句的方法:
  * executeUpdate\(String sql\)
  * executeQuery\(String sql\)
  * execute\(String sql\)

### PreparedStatement

* 相較於Statement, PreparedStatement是一種pre-compiled語句, 可以使用預留位置替SQL帶入變數值, 講白一點, 就是允許你使用參數化查詢.

### CallableStatement

* 主要用於Stored Procedure





