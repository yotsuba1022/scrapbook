# HashSet v.s. TreeSet

一般來說, 在大部分的操作情境下\(add, remove and contains\), HashSet的效能會比TreeSet要來得好\(O\(1\) v.s.O\(logn\)\), 但HashSet並不像TreeSet可以保證順序性\(natural ordering/custom comparator\).

### HashSet

* 常見的幾個操作之時間複雜度皆為O\(1\): add/remove/contains/size
* 無法保證順序性
* 是一個non-thread safe collection

### TreeSet

* 常見的幾個操作之時間複雜度皆為O\(logn\)
* 可以保證順序性, 若沒有在建構期間指定comparator, 會使用natural ordering或是自訂的comparator
* 是一個non-thread safe collection

### 重點

* HashSet與TreeSet皆保證其中之元素不會重複\(duplicate-free, 參見Java doc對兩者的add\(\)之解釋\)
* 一般來說, 把元素加入HashSet會比加入TreeSet要來得快, 可以在加入所有元素至HashSet之後, 再把這個HashSet轉成TreeSet.
  ```java
  Set<String> convertedTreeSet = new TreeSet<String>(hashSet);
  ```
* 這兩種collection實作都不是thread safe的





