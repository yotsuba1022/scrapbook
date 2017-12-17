# HashMap v.s. TreeMap

在工作上, 若需要使用到key-value pair的資料結構, 我們常常第一個想到的就是HashMap, 然而, 若想要擁有排序的功能的話, 就可以考慮下TreeMap了, 但由於底層資料結構特性的關係, TreeMap\(based on R-B tree\)基本上是比HashMap\(based on hash table\)要來得慢的.

### HashMap

* 基於hash table來實作
* 常見操作之時間複雜度為O\(1\) \(get and put\)
* 無法保證key的順序性
* 放入其中的元入必須確實地實作hashCode\(\)方法
* 是一種non-thread safe collection

### TreeMap

* 基於R-B tree\(紅黑樹\)來實作
* 常見操作之時間複雜度為O\(logn\) \(containsKey, get, put and remove\)
* 可以保證key的順序性, 若沒有在建構期間指定comparator, 會使用natural ordering\(即被放入TreeMap的元素類別自身實作Comparable介面時定義的排序規則\)或是自訂的comparator
* 是一種non-thread safe collection

### 重點

* 若想要排序HashMap的內容, 可以在HashMap集合蒐集完資料後, 將其轉換成TreeMap, 可用TreeMap的putAll\(\)方法進行轉換

### 範例

### 參考資料



