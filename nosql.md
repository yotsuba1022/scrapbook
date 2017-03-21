# NoSQL

NoSQL: **Not Only SQL**, 即"非關連式資料庫"的統稱

Why NoSQL?  
簡單說, 是為了解決在web2.0時代出現的三高需求:

1. 對DB的高並發\(high concurrency\)讀寫之要求, 或是說對low latency讀寫速度的要求: 應用程式快速地回覆可以提昇用戶的滿意度
2. 對海量資料的高效率儲存與存取之需求: 對於搜尋這樣的大型應用程式而言, 可能會用到PB級別的資料和能對應百萬級別以上的流量
3. 對DB的高可擴展性與高可用性的需求: 能更簡單的部署跟管理

而在RDB裡的一些特性, 在web2.0裡可能就沒那麼重要了, 如:

1. DB的transaction之一致性\(consistency\)
2. DB的real time讀寫
3. 複雜的SQL查詢, 特別是多表關聯查詢



