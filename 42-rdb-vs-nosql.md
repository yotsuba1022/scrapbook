# RDB v.s. NoSQL

RDB: Relational Database, 即關連式資料庫

NoSQL: Not Only SQL, 即"非關連式資料庫"的統稱

Why NoSQL?  
簡單說, 是為了解決在web2.0時代出現的三高需求:

1. 對DB的高併發\(high concurrency\)讀寫之要求
2. 對海量資料的高效率儲存與存取之需求
3. 對DB的高可擴展性與高可用性的需求

而在RDB裡的一些特性, 在web2.0裡可能就沒那麼重要了, 如:

1. DB的transaction之一致性\(consistency\)
2. DB的real time讀寫
3. 複雜的SQL查詢, 特別是多表關聯查詢

NoSQL的優點:

* 擴展簡單方便, 特別是水平橫向的擴展
  * 縱向擴展: 買更強的機器
  * 橫向擴展: 把資料分散到多台機器
* 讀寫快速高效, 多數都會映射到記憶體操作
* 成本低廉, 用普通機器, 分布式集群即可
* 資料模型靈活, 沒有固定的資料模型

NoSQL缺點:

* 不提供對SQL的支持
* 現有產品不夠成熟穩定, 功能也還有待加強



