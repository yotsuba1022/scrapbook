# Conway's Law

**"organizations which design systems ... are constrained to produce designs which are copies of the communication structures of these organizations." ... Melvin Conway, 1967.**

這是在大約50年前由這位名叫Melvin Conway的人所提出的觀點, 對於conway's law, 我的解讀是這樣:

**"想要得到什麼樣的系統, 就先要建立起什麼樣的團隊"**

我們可以從一個讓monolithic architecture演進到microservices architecture的過程來看這件事:

* 若團隊是分布式的, 而系統是monolithic的, 則在開發、測試以及部署等的溝通協調成本就大了, 可能也會在一定程度上影響到效率, 甚至出現團隊間的衝突

![](/assets/monolithic_team.png)

* 將monolithic解耦成microservices, 每個團隊都去開發、測試和發布自己的服務, 彼此之間互不干擾, 系統效率也可得到改善

這樣看下來, 可以知道組織跟系統架構之間是有種對應關係的, 若沒有對應好, 就容易出現各種各樣的問題. 若你的組織結構與文化不支持, 你也很難成功建立起高效的系統架構, 比方說在比較集中式和嚴格專注於特定職能\(開發, 業務, QA, Operations, etc\)的企業中, 要推動microservices跟DevOps就沒有那麼容易. 這類的組織職能之間都比較偏向於局部最佳化\(local optimization\).

找到對的team member, 對的客戶, 組成對的團隊, 才容易做出對的產品. 除了建立一套好的團隊文化之外, 每個參與其中的team member也要保持open-minded的心態去讓自己的視野更開闊.

