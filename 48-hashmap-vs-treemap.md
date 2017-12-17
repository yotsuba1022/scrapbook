# HashMap v.s. TreeMap

在工作上, 若需要使用到key-value pair的資料結構, 我們常常第一個想到的就是HashMap, 然而, 若想要擁有排序的功能的話, 就可以考慮下TreeMap了, 但由於底層資料結構特性的關係, TreeMap\(based on R-B tree\)基本上是比HashMap\(based on hash table\)要來得慢的.

### HashMap

* 基於hash table來實作
* 常見操作之時間複雜度為O\(1\) \(get and put\)

### TreeMap

* 基於R-B tree\(紅黑樹\)來實作
* 常見操作之時間複雜度為O\(logn\) \(containsKey, get, put and remove\)



