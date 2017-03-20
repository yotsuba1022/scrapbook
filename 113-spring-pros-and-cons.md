# Spring - Pros and Cons

Spring現在確實是Java圈子中的當紅框架, 但沒有東西是完美的, 我想這邊就整理一下自己認為的pros/cons.

Pros

* Component跟container之間的解耦
* Component跟dependency之間的解耦
* 提供了一種管理物件的機制, 把可以把中間層有效地組織起來, 就像膠水一樣
* 有利於培養code on interface的習慣
* 目的之一是為了寫出易於測試的程式
* 非侵入性\(non-intrusive\), 讓你的application對Spring的依賴盡可能的減到最小程度
* 較為輕量級的解決方案
* IoC將建立物件的職責從程式中抽離到了框架中, 如setter/constructor injection

Cons

* IoC, AOP這類的技術, 立意都是想將原本位於application中的hard-code邏輯抽出並且移至配置文件中\(或其他形式\), 大部分人都覺得這是在提高可維護性, 但若從以下幾點來看的話, 也許是缺點, 尤其是當你面對一個陌生的系統, 或是專案人員變動頻繁的情形:

  * 中斷了application的邏輯, 使程式變得不完整, 不直觀. 單從source無法完全把握application的所有行為

  * 將原本應該寫成code的邏輯變成了configuration, 可能會增加出錯的機會與負擔

  * 有時候你會發現維護程式竟然比維護config file還要輕鬆

* 有一種說法叫"overbean", 就是你什麼東西都要把它變成bean, 太過頭反而會有點亂



