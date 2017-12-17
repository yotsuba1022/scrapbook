# ArrayList v.s. LinkedList

我覺得這已經是個看到爛的老梗了, 但還是寫一下.

關於這兩個資料結構, 在工作上我目前只有用過ArrayList\(工作三年多了\), 會用到LinkedList的情境反而都是面試的時候\(~~面試都很愛考double linked list還有搭配各種奇怪條件/限制下的time complexity~~\).

基本上, ArrayList與LinkedList都是針對List介面而衍生出來的實作, 實作面上的差異如下:

* ArrayList: 透過可動態增減大小\(dynamically re-sizing\)的陣列來實作, 而且會預留空間\(~~某種程度上, 這就是會預先浪費空間的意思喇~~\)
* LinkedList: 透過double linked list來實作

再來, 在網路上查這兩個的比較, 大概都會看到如下的說法:

* ArrayList: 
  * 優點: 可透過index來取得資料, 故當程式會頻繁地運用索引去取得/搜尋集合裡的元素時, 效率會比較好\(time complexity是O\(1\)\)
  * 缺點: 因為底層實作是陣列, 所以想要在中間插入/移除元素時, 就要把其後所有的元素都往後/往前移動, 所以這部分效率就沒那麼好. 另一個我認為也有點是缺點的地方就是當加入的元素超過當前陣列的size後, JDK預設的做法是會allocate一個新的array, 其大小基本上會是原來array的1.5倍\(詳細大小會根據情形作調整, 但基本上就是大約在1.5倍左右\), 然後舊的array會被copy到新的array裡, 最後再把新的元素也加進去\(這就是worst case, time complexity是O\(n\), normal case則是O\(1\)\), 原始碼如下: 
    ![](/assets/4.6-1.png)
    ![](/assets/4.6-2.png)  
  
    ![](/assets/4.6-3.png)

    這樣做有什麼壞處呢? 如果你今天只會超出原本array size一點點, 可能剛好只是多加一個元素而已, 但是卻要多浪費將近0.5倍大小的陣列空間, 這其實在一定程度上也會給GC帶來不必要的效能損耗.
* LinkedList:
  * 優點: 因為底層實作是double linked list, 所以相較於ArrayList, 當想要在中間插入/移除物件時, 就比較不會有那麼大的effort, 因為只要把受影響的節點關係梳理好即可, 不必動到後面所有的元素\(只要O\(1\)即可, 但在ArrayList中, 這動作的代價基本上是O\(n\)\).
  * 缺點: 當需要搜尋/存取特定元素時就沒那麼方便了, 因為node基本上不會有index這種屬性, 只能用迭代的方式去找, 在當前的JDK中,  LinkedList並不會真的從頭開始找到尾, 而是會用二分法的方式看當前指定的index是位於list中的前半段/後半段, 再去進行迭代搜尋的動作, 原始碼如下:
    ![](/assets/4.6-4.png)  
  
    ![](/assets/4.6-5.png)

另外, 當要建立的list會很大的時候, 在記憶體的使用上, 就要考慮比較多一點了:

* ArrayList: 單一元素很單純, 不會像LinkedList一樣要多紀錄next/previous, 不過在加入元素的時候, 容易發生上面提到的配置過多記憶體的問題. 雖然JDK預設會給ArrayList配置空間\(根據source code, 這個大小為10\), 但是一但加入過多元素, 就會進行resize的動作, 比較好的做法是: 在宣告ArrayList的時候就先給一個預先計算好的適當大小.
* LinkedList: 對於其中的任一元素, 都會比ArrayList要來得佔空間, 因為一個node要同時紀錄next/previous的位置.











