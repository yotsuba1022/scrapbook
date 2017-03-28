# Conway's Law

"organizations which design systems ... are constrained to produce designs which are copies of the communication structures of these organizations." ... Melvin Conway, 1967.

這是在大約50年前由這位名叫Melvin Conway的人所提出的觀點, 對於conway's law, 我的解讀是這樣:

"想要得到什麼樣的系統, 就先要建立起什麼樣的團隊"

我們可以從一個讓monolithic architecture演進到microservices architecture的過程來看這件事:

* 若團隊是分布式的, 而系統是monolithic的, 則在開發、測試以及部署等的溝通協調成本就大了, 可能也會在一定程度上影響到效率, 甚至出現團隊間的衝突  
  ![](/assets/monolithic_team.png)

* 將monolithic解耦成microservices, 每個團隊都去開發、測試和發布自己的服務, 彼此之間互不干擾, 系統效率也可得到改善  
  ![](/assets/microservices_team.png)  



