# RDB v.s. NoSQL

RDB: Relational Database, 即關連式資料庫

NoSQL: Not Only SQL, 即"非關連式資料庫"的統稱

#### RDB的優點

* Transaction處理: 保持資料的一致性
* 以標準化為前提, 資料更新的開銷比較小\(相同的欄位基本上只有一處\)
* 可進行join等複雜的查詢

#### RDB的缺點

* 擴展不易: 因為存在join這種多表查詢的機制, 使得資料庫在擴展方面比較艱難
* 讀寫速度的瓶頸: 這種情況主要發生在數據量達到一定規模時由於關聯式資料庫的系統邏輯較為複雜, 使得其非常容易發生死鎖等的concurrency問題，進而導致其讀寫速度下滑非常嚴重
* 成本高: 看看那精美的Oracle Enterprise Lisence費用, 而且成本會隨著系統規模成長而不斷上升
* 有限的支撐容量: 現有的關聯式解決方案對於海量資料其實不太能夠支撐得住

#### NoSQL的優點

* 擴展簡單方便, 特別是水平橫向的擴展: 如Cassandra, MongoDB

  * 縱向擴展: 買更強的機器
  * 橫向擴展: 把資料分散到多台機器

* 讀寫快速高效, 多數都會映射到記憶體操作: 如Redis

* 成本低廉, 用普通機器, 分布式集群即可

* 資料模型靈活, 沒有固定的資料模型: 當今出現越來越多非結構化資料的需求

#### NoSQL缺點

* 不提供對SQL的支持: 這基本上會對使用者產生一定的學習和應用遷移成本
* 支持的特性不夠豐富: 大多數NoSQL都不支持transaction, 也可能沒有BI或報表這類東西
* 現有產品不夠成熟穩定

透過以上比較, 其實不難發現NoSQL在架構和資料模型方面做了"減法"; 而在擴展性與並發性上做了"加法".

#### 可以考慮使用NoSQL的情況

* DB schema經常發生變化: 常見例子如線上商城, 維護產品的屬性經常要增加新欄位/減少欄位, 這就表示說你的ORM/DAO層的程式和config會需要做更動, 若該table的資料量超過百萬級別, 新增欄位帶來的開銷其實是很可怕的\(想想index重建這種事情\). 這種時候就可以考慮以NoSQL作為解決方案, 以提升DB的可伸縮性.
* DB table的欄位是複雜資料類型: 對於複雜的資料類型來說, 很多NoSQL會以json-like的方式來儲存\(如MongoDB\), 且提供了原生的支持, 在效率上是比RDB高的.
* 高並發的資料庫請求: 在一些web2.0的應用中, 由於對資料一致性的要求比較低, 這時候RDB的transaction以及join等特性反而就會在性能方面扯後腿了, 當然這道理不見得所有場景都適用, 這邊有篇[文章](http://artur.ejsmont.org/blog/content/insert-performance-comparison-of-nosql-vs-sql-servers)做了些比較, 挺有趣的, 可以參考看看.
* 海量資料的分布式儲存: 不是每間公司都付得起Oracle要的錢, 所以利用NoSQL分布式儲存並且部署到廉價的硬體上, 是一種CP值相對高的解決方案, 像MongoDB的sharding已經可以應用到production上了\(~~雖然還是被炮轟得滿慘的~~\).

當然, NoSQL不可能解決所有的問題, 對一些業務模型複雜的應用來說, 如BI, ERP等, RDB可能還是比較好的方案.

#### 選擇適合的NoSQL

這其實不是個很好回答的問題, 因為根據每個團隊遇到的情境跟挑戰的不同, 會有各種不同的結論出現, 這邊就列出一些可以參考的點

* 資料結構特點: 譬如說結構化, 非結構化, 欄位是否可能更動, 有沒有LONGTEXT欄位等等
* 寫入特點: 像是insert比例, update比例, 是否經常更新資料的某一個小欄位, atomic update等等
* 查詢特點: 包含查詢的條件, 查詢熱點範圍. 如使用者訊息的查詢可能就是隨機的, 但新聞的查詢就是按照時間的, 越新的越頻繁

#### 摻在一起做撒尿牛丸

其實你也不用一定要選邊站, 因為實際場合中, 很多公司都是兩種一起用的. 試想一下你的網站有comment服務, 每個使用者都可以留下自己的評論, 那這時候圍繞著一個comment來說, 會有哪些欄位呢? 如評論id\(PK\), 評論對象的id, user id以及評論內容. 基本上, 我們不太可能會在資料庫裡面使用"where conmment.content like ?"這種方式去查評論內容, 畢竟這東西本質上大概都會是類似LONGTEXT欄位的資料類型. 所以一種可考慮的做法像是把評論id, 評論對象id, user id儲存在RDB裡面, 而評論內容存在NoSQL裡面. 這樣對RDB來說就節省了很多的IO了, 且也容易對評論內容做cache.

