# HashSet v.s. TreeSet

一般來說, 在大部分的操作情境下\(add, remove and contains\), HashSet的效能會比TreeSet要來得好\(O\(1\) v.s.O\(logn\)\), 但HashSet並不像TreeSet可以保證順序性\(natural ordering/custom comparator\).

### HashSet

* 常見的幾個操作之時間複雜度皆為O\(1\): add/remove/contains/size
* 無法保證順序性
* 是一個non-thread safe collection

### TreeSet

* 常見的幾個操作之時間複雜度皆為O\(logn\)
* 可以保證順序性, 若沒有在建構期間指定comparator, 會使用natural ordering\(即被放入TreeSet的元素類別自身實作Comparable介面時定義的排序規則\)或是自訂的comparator
* 是一個non-thread safe collection

### 重點

* HashSet與TreeSet皆保證其中之元素不會重複\(duplicate-free, 參見Java doc對兩者的add\(\)之解釋\)
* 一般來說, 把元素加入HashSet會比加入TreeSet要來得快, 可以在加入所有元素至HashSet之後, 再把這個HashSet轉成TreeSet.
  ```java
  Set<String> convertedTreeSet = new TreeSet<>(hashSet);
  ```
* 這兩種collection實作都不是thread safe的

### 範例

以下測試程式簡單地示範了這幾個特性:

* HashSet與TreeSet皆為deplicate-free的collection
* TreeSet會根據natural ordering/自訂義的comparator進行排序
* 將HashSet轉換成TreeSet後, 也會遵照natural ordering

```java
package idv.carl.scjp.collection.set;

import idv.carl.scjp.domain.Student;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import java.util.*;

import static org.junit.Assert.*;

/**
 * @author Carl Lu
 */
public class SetTest {

    private Set<Student> hashSet;
    private Set<Student> treeSet;

    private Comparator<Student> compareByName = (Student s1, Student s2) -> ( s1.getName().compareTo(s2.getName()) );
    private Comparator<Student> compareByAge = (Student s1, Student s2) -> ( s1.getAge().compareTo(s2.getAge()) );

    private Student student1 = new Student(1L, "Carl", 28);
    private Student student2 = new Student(2L, "RuRu", 18);
    private Student student3 = new Student(3L, "Miffy", 16);
    private Student student4 = new Student(4L, "Lara", 38);
    private Student student5 = new Student(5L, "BB", 8);

    @Before
    public void init() {
        hashSet = new HashSet<>();
        treeSet = new TreeSet<>();
    }

    @After
    public void clear() {
        hashSet.clear();
        treeSet.clear();
    }

    @Test
    public void testAddAndDuplicateFree() {
        assertTrue(hashSet.add(student1));
        assertTrue(hashSet.add(student2));
        assertTrue(hashSet.add(student3));
        assertTrue(hashSet.add(student4));
        assertTrue(hashSet.add(student5));
        assertFalse(hashSet.add(student1));
        assertEquals(5, hashSet.size());

        assertTrue(treeSet.add(student1));
        assertTrue(treeSet.add(student2));
        assertTrue(treeSet.add(student3));
        assertTrue(treeSet.add(student4));
        assertTrue(treeSet.add(student5));
        assertFalse(treeSet.add(student3));
        assertEquals(5, treeSet.size());
    }

    @Test
    public void testTreeSetNaturalOrdering() {
        treeSet.add(student3);
        treeSet.add(student4);
        treeSet.add(student5);
        treeSet.add(student1);
        treeSet.add(student2);

        String actualIdSequence = Arrays.toString(treeSet.stream().map(Student::getId).toArray());
        String expectedIdSequence = Arrays.toString(
                new Long[] {student1.getId(), student2.getId(), student3.getId(), student4.getId(), student5.getId()});
        assertEquals(expectedIdSequence, actualIdSequence);
    }

    @Test
    public void testTreeSetOrderByName() {
        treeSet = new TreeSet<>(compareByName);
        treeSet.add(student5);
        treeSet.add(student4);
        treeSet.add(student3);
        treeSet.add(student2);
        treeSet.add(student1);

        String actualNameSequence = Arrays.toString(treeSet.stream().map(Student::getName).toArray());
        String expectedNameSequence = Arrays.toString(
                new String[] {student5.getName(), student1.getName(), student4.getName(), student3.getName(),
                        student2.getName()});

        assertEquals(expectedNameSequence, actualNameSequence);
    }

    @Test
    public void testTreeSetOrderByAge() {
        treeSet = new TreeSet<>(compareByAge);
        treeSet.add(student5);
        treeSet.add(student3);
        treeSet.add(student4);
        treeSet.add(student2);
        treeSet.add(student1);

        String actualAgeSequence = Arrays.toString(treeSet.stream().map(Student::getAge).toArray());
        String expectedAgeSequence = Arrays.toString(
                new Integer[] {student5.getAge(), student3.getAge(), student2.getAge(), student1.getAge(), student4.getAge()});
        assertEquals(expectedAgeSequence, actualAgeSequence);
    }

    @Test
    public void testConvertHashSetToTreeSetAndVerifyByNaturalOrdering() {
        hashSet.add(student3);
        hashSet.add(student4);
        hashSet.add(student2);
        hashSet.add(student1);
        hashSet.add(student5);

        treeSet = new TreeSet<>(hashSet);

        String actualIdSequence = Arrays.toString(treeSet.stream().map(Student::getId).toArray());
        String expectedIdSequence = Arrays.toString(
                new Long[] {student1.getId(), student2.getId(), student3.getId(), student4.getId(), student5.getId()});
        assertEquals(expectedIdSequence, actualIdSequence);
    }

}
```

範例程式[點我](https://github.com/yotsuba1022/scjp/blob/master/src/test/java/idv/carl/scjp/collection/set/SetTest.java)

### 參考資料

* HashSet Java Doc: [連結](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html)
* TreeSet Java doc: [連結](https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html)



