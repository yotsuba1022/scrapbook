# Statement, PreparedStatement and CallableStatement

在JDBC的世界裡, 我們常常會透過Statement/PreparedStatement來對DB進行一些操作, 這篇主要是想記錄一下這兩者之間的差異.

### Statement

* 通常用於執行靜態的SQL語句並且獲得SQL產生之結果, 通常會看到以下三種執行SQL語句的方法:
  * executeUpdate\(String sql\)
  * executeQuery\(String sql\)
  * execute\(String sql\)

### PreparedStatement

* 相較於Statement, PreparedStatement是一種pre-compiled語句, 可以使用預留位置替SQL帶入變數值, 講白一點, 就是允許你使用參數化查詢.
* PreparedStatement基本上是java.sql package下的一個介面, 用來執行SQL語句, 常見的使用起手式如下:
  * 透過DriverManager取得connection
  * 透過呼叫connection.preparedStatement\(sql\)獲得PreparedStatement物件
  * DB會對sql進行pre-compile處理\(前提是JDBC要支援\), 之後這個PreparedStatement被預先編譯好後, 就可以在未來的查詢中重複利用了, 這在需要多次查詢的時候, 基本上是比Statement物件生成的的查詢速度要來得快的, 以下是一個簡單的範例:
  * 在上面的範例中, 若還是使用PreparedStatement進行同樣的查詢, 儘管參數值不同, 譬如帶入其他銀行的名稱作為參數值, DB還是會去呼叫之前compiler已經compile過的執行語句
* 基本上, 在實際工作場合中, 應該都是使用PreparedStatement的場合多於Statement, 以下是PreparedStatement的一些優勢:
  * 動態參數化查詢: 可以使用帶參數的sql語句, 通過使用相同的sql搭配不同的參數來做查詢, 總比建立一個不同的語句要來得好. 這個部分有另一個名詞叫做"[同構\(isomorphism\)](https://zh.wikipedia.org/wiki/%E5%90%8C%E6%9E%84)", 基本上PreparedStatement也是在解決同構問題.
  * 比Statement快: 因為PreparedStatement會被pre-compile在DBMS中, 使用時一般來說會比普通的查詢更快, 做的工作也更少\(因為DB對這個語句的分析/編譯/最佳化已經在第一次查詢之前就完成了\). 這也是為什麼在production環境中, 使用PreparedStatement會比Statement更好的原因之一 --- 不要讓你的DB不開心或是有太多額外的負擔, 不然你遲早會倒大楣.
  * 另外要注意的一點就是: 為了獲得性能上的優勢, 應該盡量使用參數化sql而不是使用字串連接的方式, 以下是兩個範例:
    * SQL1: 用字串連接的方式
      ```java
      String loanType = getLoanType();
      PreparedStatement prestmt = conn.prepareStatement("select banks from billing where billing_type=" + billingType);
      ```
    * SQL2: 參數化sql
      ```java
      PreparedStatement prestmt = conn.prepareStatement("select banks from billing where billing_type=?");
      prestmt.setString(1, billingType);
      ```
  * 
* 
### CallableStatement

* 主要用於Stored Procedure



