# Diffie - Hellman Key Exchange

有時也簡稱為DH演算法, 是一種安全協定. 其可以讓雙方在完全沒有對方任何預先資訊的條件下通過public channel\(unsafe channel\)建立起一個金鑰. 這個金鑰可以在後續的通訊中作為對稱金鑰來加密通訊內容. 雖然DH演算法本身是一個匿名\(無認證\)的金鑰交換協定, 它卻是很多認證協定的基礎, 並且被用來提供傳輸層安全協定的短暫模式中的完備的前向安全性\(forward secrecy\).

講白話一點, DH演算法就是在教你如何"安全地"告訴對方密碼而不用擔心密碼被竊聽. 通過在public channel交換一個資訊, 就可以建立一個可用於在public channel上安全通信的shared secret.

以下描述DH演算法的運作流程:

最早提出的這個協定使用如下的離散對數\(Discrete Logarithm\)公式來做運算:

![](/assets/DH-formula.png)

公式裡的g, x, b, p都是實數, 其中:

* g: generator
* p: prime
* x: 通訊雙方各會有一個, 相異的私鑰

g跟p會是兩邊先溝通好的機制



