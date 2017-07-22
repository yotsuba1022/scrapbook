# 塞不進去

先來講一下大概的情境:

我有一個List \(此處稱listA\), 是透過Arrays.asList構成的, 然後在某些場合下, 我會往這個listA裡面繼續塞值, 以利用來處理商業邏輯, 可是當條件判斷要往listA裡面塞值的時候, 就出現了以下這個exception:

```java
java.lang.UnsupportedOperationException
```

查了一下, 才發現透過Arrays.asList建構出來的list, 是fixed-size list, 所以才會塞不進去...

這邊有[一篇](https://stackoverflow.com/questions/2965747/why-do-i-get-an-unsupportedoperationexception-when-trying-to-remove-an-element-f)也是踩到一樣的雷, 特此紀錄一下, 下面用一個簡單的方法以及單元測試說明一下可塞進去與不可塞進去的場合:

```java
package idv.carl.scjp.collection.list;

import java.util.List;

/**
 * @author Carl Lu
 */
public class ListAddDemo {

    public static void addToList(String value, List<String> list) {
        list.add(value);
    }

}
```

原始碼[點我](https://github.com/yotsuba1022/scjp/blob/master/src/main/java/idv/carl/scjp/collection/list/ListAddDemo.java)

```java

package idv.carl.scjp.collection.list;

import org.junit.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

import static org.junit.Assert.assertEquals;

/**
 * @author Carl Lu
 */
public class ListAddDemoTest {

    @Test
    public void testAddToNonFixedSizeList() {
        List<String> nonFixedSizeList = new ArrayList<>();
        nonFixedSizeList.add("Que");
        ListAddDemo.addToList("Pa so!!!", nonFixedSizeList);
        assertEquals(2, nonFixedSizeList.size());
    }

    @Test(expected = UnsupportedOperationException.class)
    public void testAddToFixedSizeList() {
        List<String> fixedSizeList = Arrays.asList("Que", "pa");
        fixedSizeList.add("so");
    }

}
```

原始碼[點我](https://github.com/yotsuba1022/scjp/blob/master/src/test/java/idv/carl/scjp/collection/list/ListAddDemoTest.java)

