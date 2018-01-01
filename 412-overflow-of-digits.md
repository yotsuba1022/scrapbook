# Overflow of Digits

在處理數字的時候, 有一個很重要的議題是要處理溢位\(overflow\)問題, 首先, 先來看一下若是在32位元的情境下, 我們對10進位整數的基本定義:

* **32-bit unsigned integer**: 即32-bit無號整數, 其範圍是 0 ~ \(2^32\)-1, 即 0~4,294,967,295  
  -&gt; \(2^32\)-1中, -1的原因是因為我們要從0開始算, 而非從1開始.

* **32-bit signed integer**: 即32-bit有號整數, -\(2^31\) ~ \(2^31\)-1, 即 -2,147,483,648 ~ 2,147,483,647  
  -&gt; 這邊是31次方而不是32次方是因為有號\(signed\)整數要同時紀錄正/負數的範圍, 其會各佔掉一半的空間, 所以相較於無號整數, 其最大值只有一半而已\(即少了2的一次方\), 上限-1的原因也是一樣要包含0.

### 關於Overflow

有了前面的定義, 我們就可以來看overflow了, 在32-bit的作業系統上, 記憶體只能記錄上述範圍的數字, 若超過了這個範圍, 記憶體就無法正常處理了, 這就是所謂的overflow. 這邊用簡單的例子來看一下overflow的特性:

* 32-bit signed integer overflow:

  * 2,147,483,647 + 1 = -2,147,483,648  
    由於前者已經是最大值了, 所以這時候若再+1, 會繞一圈回到最小值.

  * -2,147,483,648 - 2 = 2,147,483,646  
    同前一個情況, 你如果把最小值拿來-1, 就會繞回最大值, 把最小值拿來-2就會是最大值-1.

### 處理Overflow

這邊會介紹幾種對overflow的處理方式, 首先是針對LeetCode中的[Reverse Integer](https://github.com/yotsuba1022/LeetCode/blob/master/src/main/java/idv/carl/leetcode/algorithms/easy/reverseinteger/Solution.java)這個題目的解法用的, 但也是可以在其他不同的context下當作一個借鏡.

在這個題目中, 我們設定若發生overflow的話, 就要return 0, 其判斷overflow的邏輯大概長得像這樣:

```java
private final static int MAX_32_BIT_INTEGER = ( (int) Math.pow(2, 31) ) - 1; // 2,147,483,647
private final static int MIN_32_BIT_INTEGER = -(int) Math.pow(2, 31); // -2,147,483,648

public static int optimizedReverseInteger(int input) {
        int result = 0;
        while (input != 0) {
            if (result > MAX_32_BIT_INTEGER / 10 ||                           // overflow scenario 1
                result < MIN_32_BIT_INTEGER / 10 ||                           // overflow scenario 2
                ( result == MAX_32_BIT_INTEGER / 10 && (input % 10 > 7 ) ) || // overflow scenario 3
                result == MIN_32_BIT_INTEGER / 10 && ( input % 10 < -8 )) {   // overflow scenario 4
                return 0;
            }
            result = ( result * 10 ) + ( input % 10 );
            input /= 10;
        }
        return result;
}
```

這邊處理的方式大概分兩個面向, 四種情況:

#### 擺明就是要爆了的情況

* **Scenario1 \(正數, 且擺明會爆掉\)**: 假如result大於214748364, 譬如是214748365, 這時候碰到result \* 10的時候, 就會變成2147483650, 就超過最大值2147483647了. 這是第一種會overflow的情形.

* **Scenario2 \(負數, 且擺明會爆掉\)**: 若result小於-214748364, 譬如-214748365, 若遇到result \* 10, 就會變成-2147483650, 就小於最小值-2147483648了. 這是第二種會overflow的情形.

#### 在爆掉的邊緣的情況

* **Scenario3 \(正數, 但在爆掉邊緣\)**: 前兩種是擺明就會直接爆掉的, 再來若result剛好等於214748364, 這時result \* 10, 是剛好逼近最大值沒有爆掉\(2147483640\), 但這時若碰到加上\(input % 10\)的內容剛好大於7的話 \(例如8\), 就會變成2147483640 + 8 = 2147483648, 就又爆了.

* **Scenario4 \(負數, 但在爆掉邊緣\)**: 最後一種, 若result剛好等於-214748364, 這時result \* 10, 剛好達到-2147483640, 接近爆掉的範圍, 此時若\(input % 10\)的值小於8的話 \(如-9\), 就會變成-2147483649, 就爆了.

除了上面針對Reverse Integer的解法, 我們再來看一下另外一個同樣要處理overflow的題目是怎麼做的, 這題就是: [atoi](https://github.com/yotsuba1022/LeetCode/blob/master/src/main/java/idv/carl/leetcode/algorithms/medium/atoi/Solution.java)

在這個題目中, 我只截取處理overflow的區塊, 內容如下:

```java
int result = 0;
        while (index < inputLength && Character.isDigit(input.charAt(index))) {
            int currentDigit = Character.getNumericValue(input.charAt(index));
            if (( result > Integer.MAX_VALUE / 10 ) ||                       // overflow scenario 1
                ( result == Integer.MAX_VALUE / 10 && currentDigit >= 8 )) { // overflow scenario 2
                return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
            }
            result = ( result * 10 ) + currentDigit;
            index++;
}
```

這邊處理的方式基本上只有兩種:

* **Scenario1 \(輸入擺明會爆掉\)**: 若程式中的result大於214748364, 這只要碰到\(result \* 10\)就絕對會爆了, 而由於這邊的result本身可以想成是一個絕對值\(因為正負號會在最後return的時候才補上\), 所以這個條件就相當於前一個例子中的scenario1 and scenario2.

* **Scenario2 \(輸入在爆掉邊緣\)**: 若程式中的result剛好等於214748364, 那還有可能在overflow的範圍內, 所以這時候就要看下一輪輸入的值來判定是否會爆掉, 由於這裡要同時考慮正負號爆掉的情況, 所以取currentDigit &gt;= 8, 原因如下:

  * **若當前為正數**: 下一輪input &gt; 7就會爆 \(這與 input &gt;= 8 是等價的\)
  * **若當前為負數**: 下一輪input &lt; -8就會爆 \(在絕對值的角度來看, 這與 input &gt;= 8 也是等價的\)
  * 我們以絕對值的角度來看, 上面這兩個條件合體後就是currentDigit &gt;= 8了, 也相當於前一個例子中的scenario3 and scenario4.



