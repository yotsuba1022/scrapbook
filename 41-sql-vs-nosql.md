# CAP Theorem, ACID v.s. BASE

這邊我想寫一些RDB跟NoSQL的比較, 所以會先提到一些基本觀念:

#### CAP Theorem

對於一個分布式計算系統來說, 不可能同時滿足以下三點:

* **Consistency**: 系統在執行過某項操作後仍然處於一致的狀態, 在分布式系統中, 更新操作執行成功後所有的用戶都應該讀取到最新的值, 這樣的系統被認為具有強一致性
* **Availability**: 每一個操作總是能夠在一定的時間內返回結果, 或可想成任何時間點都可讀寫\(可用性\), 即任何一個節點失效都不會影響其它節點繼續運作
* **Partition tolerance**: 系統若處於網路分區的情況下仍然可以接受請求並處理, 此處的網路分區是指由於某些原因導致網路被分成了若干個孤立區域, 且區域之間不互通. 當然, 這其實也相當於對通信的時限要求, 因為系統若不能在時限內達成資料一致性, 就意味著發生了分區的情況

所以, 我們可以根據CAP來把DB分成以下三種類型:

* **CA**: 單點集群, 滿足了一致性和可用性, 通常在可擴展性上不太強大, 如relational database, 若只有一台機器的話是不能實現CA的
* **CP**: 滿足一致性和分區容錯性, 通常性能不是特別高, 如分布式資料庫
* **AP**: 滿足可用性即分區容錯性, 通常對一致性的要求就沒那麼高, 如大多數的NoSQL

#### ACID \(For Relational Database\)

* **Atomicity**: 原子性, 即transaction中的所有操作, 要就全都成功, 不然就全都失敗
* **Consistency**: 一致性, 在transaction的開始與結束時, DB都處於一致的合法狀態, 例如從A的帳戶轉1000塊錢到B的帳戶, 則在這個交易的開始與結束, 總金額是一樣的. 此處的consistency跟CAP theorem裡的consistency是不同的東西, 別搞混了
* **Isolation**: 隔離性, 同時進行的transaction間, 互相不影響
* **Durability**: 在transaction結束時, 此操作是不可逆的, 即commit之後, 系統將保證資料不會丟失, 即便是系統crash

題外話, 其實大多數的DB廠商都體會到了DB分區的必要性, 所以引入了一種稱為**2PC**\(**Two-Phase-Commit**\)的技術來提供跨越多個DB instance的ACID保證.

#### BASE \(For Non-Relational Database\)

* **Basically Available**: 系統能夠基本運行, 一直提供服務
* **Soft-state**: 系統不要求一直保持強一致狀態
* **Eventual consistency**: 系統需要在某一時刻後達到一致性要求

BASE模型可視為傳統ACID模型的反面, 由上可知, 不同於ACID, BASE強調犧牲consistency, 從而得到availability, 資訊允許在一段時間內的不一致, 只要保證最終一致即可.

#### 



