# Statement, PreparedStatement and CallableStatement

在JDBC的世界裡, 我們常常會透過Statement/PreparedStatement來對DB進行一些操作, 這篇主要是想記錄一下這兩者之間的差異\(CallableStatement我沒用過, 以後有機會再補吧\).

### Statement

* 通常用於執行靜態的SQL語句並且獲得SQL產生之結果, 通常會看到以下三種執行SQL語句的方法:
  * executeUpdate\(String sql\): 執行SQL insert/update/delete語句, 回傳受影響資料的數目或是0
  * executeQuery\(String sql\): 執行回傳單個ResultSet的SQL語句, 回傳類型就是ResultSet
  * execute\(String sql\): 執行可以回傳多個結果的SQL語句, 回傳類型是boolean, 若回傳的是更新的數目, 就回傳false, 若是ResultSet, 則回傳true

### PreparedStatement

* 相較於Statement, PreparedStatement是一種pre-compiled語句, 可以使用預留位置\("?"\)替SQL帶入變數值, 講白一點, 就是允許你使用參數化查詢.
* PreparedStatement基本上是java.sql package下的一個介面, 用來執行SQL語句, 常見的使用起手式如下:
  * 透過DriverManager取得connection
  * 透過呼叫connection.preparedStatement\(sql\)獲得PreparedStatement物件
  * DB會對sql進行pre-compile處理\(前提是JDBC要支援\), 之後這個PreparedStatement被預先編譯好後, 就可以在未來的查詢中重複利用了, 這在需要多次查詢的時候, 基本上是比Statement物件生成的的查詢速度要來得快的, 以下是一個簡單的範例:
  * 在上面的範例中, 若還是使用PreparedStatement進行同樣的查詢, 儘管參數值不同, 譬如帶入其他銀行的名稱作為參數值, DB還是會去呼叫之前compiler已經compile過的執行語句
* 基本上, 在實際工作場合中, 應該都是使用PreparedStatement的場合多於Statement, 以下是PreparedStatement的一些優勢:

  * 動態參數化查詢: 可以使用帶參數的sql語句, 通過使用相同的sql搭配不同的參數來做查詢, 總比建立一個不同的語句要來得好. 這個部分有另一個名詞叫做"[同構\(isomorphism\)](https://zh.wikipedia.org/wiki/同构)", 基本上PreparedStatement也是在解決同構問題.
  * 比Statement快: 因為PreparedStatement會被pre-compile在DBMS中, 使用時一般來說會比普通的查詢更快, 做的工作也更少\(因為DB對這個語句的分析/編譯/最佳化已經在第一次查詢之前就完成了\). 這也是為什麼在production環境中, 使用PreparedStatement會比Statement更好的原因之一 --- 不要讓你的DB不開心或是有太多額外的負擔, 不然你遲早會倒大楣.
  * 另外要注意的一點就是: 為了獲得性能上的優勢, 應該盡量使用參數化sql而不是使用字串連接的方式, 以下是兩個範例:
    * SQL1: 用字串連接的方式
      ```java
      String loanType = getLoanType();
      PreparedStatement prestmt = conn.prepareStatement("select banks from billing where billing_type = " + billingType);
      ```
    * SQL2: 參數化sql
      ```java
      PreparedStatement prestmt = conn.prepareStatement("select banks from billing where billing_type = ?");
      prestmt.setString(1, billingType);
      ```
  * 可以防止SQL Injection攻擊:

    * 以下是一個很老梗的case,  某個網站驗證登入的SQL可能長這副樣子:

    * ```java
      sql = "SELECT * FROM users WHERE name = '" + userName + "' and password = '"+ passWord +"';";
      ```
    * 然後故意填入這些東西:

    * ```java
      userName = "1' OR '1' = '1";
      passWord = "1' OR '1' = '1";
      ```
    * 這樣sql就會變成:

    * ```java
      sql = "SELECT * FROM users WHERE name = '1' OR '1'='1' and password = '1' OR '1'='1';";
      ```
    * 這表示where條件恆為true, 就等於下了這條:

    * ```java
      sql = "SELECT * FROM users";
      ```
    * 這樣就可能可以做到無帳密登入, 或是人家更壞一點, 在裡面綁上"DROP TABLE xxx"之類的東西...

    * 不過, 在使用PreparedStatement的參數化查詢時, 基本上可以避免大多數的SQL Injection. 在使用參數化查詢的情況下, DB不會將參數視為SQL指令的一部分來處理, 而是會於DB對SQL的compile結束後, 才套用參數運作, 故儘管含有破壞性指令, 也不會被DB執行.

    * 另外要補充的是, 前面那個塞入惡意指令的方式\(包含單引號字元的那個\), 可以透過字元取代來避免攻擊, 譬如:

    * ```java
      sql = "SELECT * FROM users WHERE name = '" + userName + "';";
      ```
    * 傳入的字串可能是:

    * ```java
      userName = " 1' OR 1=1";
      ```
    * 然後我們可以把一個單引號替換成兩個單引號:

    * ```java
      userName = " 1'' OR 1=1";
      ```
    * 結果就會變這樣:

    * ```java
      sql = "SELECT * FROM users WHERE name = '1'' OR 1=1';";
      ```
    * 所以DB會去找name = "1'' OR 1=1"的紀錄, 從而避免了攻擊.

  * 比起散亂的字串連結查詢, PreparedStatement可讀性更高, 也比較安全.

* 雖然這樣看起來, PreparedStatement好像很不錯, 但其還是有一定的限制存在, 例如為了防止SQL Injection, 其不允許一個"?"參數裡包含多個值, 若你的查詢子句裡面使用到了"in\(xxx,xxx...\)"這種東西的話, 就有點麻煩了, 譬如下面這段SQL就不會回傳結果:

  * ```java
    sql = "SELECT * FROM billing WHERE billing_type IN (?)";
    preparedSatement.setString(1, "'credit card', 'cash', 'apple pay'");
    ```
  * 解法可以參考這篇: [點我](https://stackoverflow.com/questions/178479/preparedstatement-in-clause-alternatives)

* PreparedStatement跟java.sql.Connection是相關聯的, 記得用完PreparedStatement之後再關閉connection, 不然PreparedStatement就不能用了.

* 預留位置"?"的索引位置是從1開始不是從0開始, 你如果寫0會拋出"java.sql.SQLException invalid column index"這種exception.

### CallableStatement

* 主要用於Stored Procedure



