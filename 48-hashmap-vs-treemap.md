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
* 可以保證key的順序性, 若沒有在建構期間指定comparator, 會使用natural ordering\(**自行判斷Key的型別並且用ASC排序, 前提是Key的型別必須實作Comparable, 若沒有實作, 在runtime期間會拋出java.lang.ClassCastException**\)或是自訂的comparator
* 是一種non-thread safe collection

### 重點

* 若想要排序HashMap的內容, 可以在HashMap集合蒐集完資料後, 將其轉換成TreeMap, 可用TreeMap的putAll\(\)或是constructor方法進行轉換:
  ```java
  // putAll():
  Map<K, V> treeMap = new TreeMap<>();
  treeMap.putAll(hashMap);

  // constructor:
  Map<K, V> treeMap = new TreeMap<>(hashMap);
  ```

### 範例

以下測試程式簡單地示範了這幾個特性:

* HashMap與TreeMap的put是會更新舊值且回傳舊值的, 同時value允許為null
* TreeMap會根據natural ordering/自訂義的comparator進行排序
* 將HashMap轉換成TreeMap後, 也會遵照natural ordering

```java
package idv.carl.scjp.collection.map;

import idv.carl.scjp.domain.Student;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import java.util.*;

import static org.junit.Assert.assertEquals;
import static org.junit.Assert.assertNull;

/**
 * @author Carl Lu
 */
public class MapTest {

    private Map<Long, Student> hashMap;
    private Map<Long, Student> treeMapWithLongKey;
    private Map<String, Student> treeMapWithStrKey;
    private Map<Integer, Student> treeMapWithIntegerKey;

    private Comparator<Long> compareByIdDesc = (Long id1, Long id2) -> ( -id1.compareTo(id2) );
    private Comparator<String> compareByNameDesc = (String name1, String name2) -> ( -name1.compareTo(name2) );
    private Comparator<Integer> compareByAgeDesc = (Integer age1, Integer age2) -> ( -age1.compareTo(age2) );

    private Student student1 = new Student(1L, "Carl", 28);
    private Student student2 = new Student(2L, "RuRu", 18);
    private Student student3 = new Student(3L, "Miffy", 16);
    private Student student4 = new Student(4L, "Lara", 38);
    private Student student5 = new Student(5L, "BB", 8);
    private Student student6 = new Student(6L, "Gru", 12);
    private Student updatedStudent1 = new Student(1L, "Carl", 30);

    @Before
    public void init() {
        hashMap = new HashMap<>();
        treeMapWithLongKey = new TreeMap<>();
        treeMapWithStrKey = new TreeMap<>();
        treeMapWithIntegerKey = new TreeMap<>();
    }

    @After
    public void clear() {
        hashMap.clear();
        treeMapWithLongKey.clear();
        treeMapWithStrKey.clear();
        treeMapWithIntegerKey.clear();
    }

    @Test
    public void testPutWithHashMap() {
        hashMap.put(student1.getId(), student1);
        hashMap.put(student2.getId(), student2);
        hashMap.put(student3.getId(), student3);
        hashMap.put(student4.getId(), student4);
        hashMap.put(student5.getId(), student5);
        hashMap.put(student6.getId(), null);

        assertEquals(6, hashMap.size());
        assertEquals(student1, hashMap.get(student1.getId()));
        assertNull(hashMap.get(student6.getId()));

        assertEquals(student1, hashMap.put(updatedStudent1.getId(), updatedStudent1));
        assertNull(hashMap.put(student6.getId(), student6));
        assertEquals(updatedStudent1, hashMap.get(updatedStudent1.getId()));
        assertEquals(student6, hashMap.get(student6.getId()));
    }

    @Test
    public void testPutWithTreeMap() {
        treeMapWithLongKey.put(student1.getId(), student1);
        treeMapWithLongKey.put(student2.getId(), student2);
        treeMapWithLongKey.put(student3.getId(), student3);
        treeMapWithLongKey.put(student4.getId(), student4);
        treeMapWithLongKey.put(student5.getId(), student5);
        treeMapWithLongKey.put(student6.getId(), null);

        assertEquals(6, treeMapWithLongKey.size());
        assertEquals(student1, treeMapWithLongKey.get(student1.getId()));
        assertNull(treeMapWithLongKey.get(student6.getId()));

        assertEquals(student1, treeMapWithLongKey.put(updatedStudent1.getId(), updatedStudent1));
        assertNull(treeMapWithLongKey.put(student6.getId(), student6));
        assertEquals(updatedStudent1, treeMapWithLongKey.get(updatedStudent1.getId()));
        assertEquals(student6, treeMapWithLongKey.get(student6.getId()));
    }

    @Test
    public void testTreeMapWithNaturalIdOrdering() {
        treeMapWithLongKey.put(student2.getId(), student2);
        treeMapWithLongKey.put(student1.getId(), student1);
        treeMapWithLongKey.put(student5.getId(), student5);
        treeMapWithLongKey.put(student4.getId(), student4);
        treeMapWithLongKey.put(student3.getId(), student3);

        String actualKeyOrdering = Arrays.toString(new ArrayList<>(treeMapWithLongKey.keySet()).toArray());
        String expectedKeyOrdering = Arrays.toString(
                new Long[] {student1.getId(), student2.getId(), student3.getId(), student4.getId(), student5.getId()});

        assertEquals(expectedKeyOrdering, actualKeyOrdering);
    }

    @Test
    public void testTreeMapWithIdOrderingDesc() {
        treeMapWithLongKey = new TreeMap<>(compareByIdDesc);
        treeMapWithLongKey.put(student2.getId(), student2);
        treeMapWithLongKey.put(student1.getId(), student1);
        treeMapWithLongKey.put(student5.getId(), student5);
        treeMapWithLongKey.put(student4.getId(), student4);
        treeMapWithLongKey.put(student3.getId(), student3);

        String actualKeyOrdering = Arrays.toString(new ArrayList<>(treeMapWithLongKey.keySet()).toArray());
        String expectedKeyOrdering = Arrays.toString(
                new Long[] {student5.getId(), student4.getId(), student3.getId(), student2.getId(), student1.getId()});

        assertEquals(expectedKeyOrdering, actualKeyOrdering);
    }

    @Test
    public void testTreeMapWithNaturalNameOrdering() {
        treeMapWithStrKey.put(student2.getName(), student2);
        treeMapWithStrKey.put(student1.getName(), student1);
        treeMapWithStrKey.put(student5.getName(), student5);
        treeMapWithStrKey.put(student4.getName(), student4);
        treeMapWithStrKey.put(student6.getName(), student6);
        treeMapWithStrKey.put(student3.getName(), student3);

        String actualKeyOrdering = Arrays.toString(new ArrayList<>(treeMapWithStrKey.keySet()).toArray());
        String expectedKeyOrdering = Arrays.toString(
                new String[] {student5.getName(), student1.getName(), student6.getName(), student4.getName(), student3.getName(),
                        student2.getName()});

        assertEquals(expectedKeyOrdering, actualKeyOrdering);
    }

    @Test
    public void testTreeMapWithNameOrderingDesc() {
        treeMapWithStrKey = new TreeMap<>(compareByNameDesc);
        treeMapWithStrKey.put(student2.getName(), student2);
        treeMapWithStrKey.put(student1.getName(), student1);
        treeMapWithStrKey.put(student5.getName(), student5);
        treeMapWithStrKey.put(student4.getName(), student4);
        treeMapWithStrKey.put(student6.getName(), student6);
        treeMapWithStrKey.put(student3.getName(), student3);

        String actualKeyOrdering = Arrays.toString(new ArrayList<>(treeMapWithStrKey.keySet()).toArray());
        String expectedKeyOrdering = Arrays.toString(
                new String[] {student2.getName(), student3.getName(), student4.getName(), student6.getName(), student1.getName(),
                        student5.getName(),});

        assertEquals(expectedKeyOrdering, actualKeyOrdering);
    }

    @Test
    public void testTreeMapWithNaturalAgeOrdering() {
        treeMapWithIntegerKey.put(student2.getAge(), student2);
        treeMapWithIntegerKey.put(student1.getAge(), student1);
        treeMapWithIntegerKey.put(student5.getAge(), student5);
        treeMapWithIntegerKey.put(student4.getAge(), student4);
        treeMapWithIntegerKey.put(student6.getAge(), student6);
        treeMapWithIntegerKey.put(student3.getAge(), student3);

        String actualKeyOrdering = Arrays.toString(new ArrayList<>(treeMapWithIntegerKey.keySet()).toArray());
        String expectedKeyOrdering = Arrays.toString(
                new Integer[] {student5.getAge(), student6.getAge(), student3.getAge(), student2.getAge(), student1.getAge(),
                        student4.getAge()});

        assertEquals(expectedKeyOrdering, actualKeyOrdering);
    }

    @Test
    public void testTreeMapWithAgeOrderingDesc() {
        treeMapWithIntegerKey = new TreeMap<>(compareByAgeDesc);
        treeMapWithIntegerKey.put(student2.getAge(), student2);
        treeMapWithIntegerKey.put(student1.getAge(), student1);
        treeMapWithIntegerKey.put(student5.getAge(), student5);
        treeMapWithIntegerKey.put(student4.getAge(), student4);
        treeMapWithIntegerKey.put(student6.getAge(), student6);
        treeMapWithIntegerKey.put(student3.getAge(), student3);

        String actualKeyOrdering = Arrays.toString(new ArrayList<>(treeMapWithIntegerKey.keySet()).toArray());
        String expectedKeyOrdering = Arrays.toString(
                new Integer[] {student4.getAge(), student1.getAge(), student2.getAge(), student3.getAge(), student6.getAge(),
                        student5.getAge(),});

        assertEquals(expectedKeyOrdering, actualKeyOrdering);
    }

    @Test
    public void testConvertHashMapToTreeMapAndVerifyByNaturalOrdering() {
        hashMap.put(student3.getId(), student3);
        hashMap.put(student1.getId(), student1);
        hashMap.put(student5.getId(), student5);
        hashMap.put(student4.getId(), student4);
        hashMap.put(student2.getId(), student2);

        treeMapWithLongKey.putAll(hashMap);

        String actualKeyOrdering = Arrays.toString(new ArrayList<>(treeMapWithLongKey.keySet()).toArray());
        String expectedKeyOrdering = Arrays.toString(
                new Long[] {student1.getId(), student2.getId(), student3.getId(), student4.getId(), student5.getId()});

        assertEquals(expectedKeyOrdering, actualKeyOrdering);
    }

}
```

### 參考資料

* HashMap Java Doc: [連結](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)
* TreeMap Java Doc: [連結](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html)



