# Shard v.s. Replica

在MongoDB裡, 對於資料集的處理, 有很多種方式, 這邊只是簡單記錄一下shard與replica的差異.

* Replica Set: 又稱副本集, 即指一組server的cluster, 其中有一個主server\(primary\), 用於處理user的request; 其餘為備份server\(secondary\), 用於保存primary的資料副本. 若primary掛了, 會自動將一個secondary升級為新的primary, 進而保證服務的運作及可用性.
* Shard: 所謂的sharding \(Horizontal scaling\), 指的就是把資料拆分, 將其分散到不同機器上的過程. MongoDB支持auto sharding, 對application來說, 好像始終都和單一個server互動一般.

由以上敘述可知: **Replication是讓多台server擁有相同的資料副本, 而Sharding是指每個shard都擁有整個資料集的一個子集, 且互相為不同的資料, 多個shards的資料整合起來構成整個資料集.**



