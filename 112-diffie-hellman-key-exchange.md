# Diffie - Hellman Key Exchange

有時也簡稱為DH演算法, 是一種安全協定. 其可以讓雙方在完全沒有對方任何預先資訊的條件下通過public channel\(unsafe channel\)建立起一個金鑰. 這個金鑰可以在後續的通訊中作為對稱金鑰來加密通訊內容. 雖然DH演算法本身是一個匿名\(無認證\)的金鑰交換協定, 它卻是很多認證協定的基礎, 並且被用來提供傳輸層安全協定的短暫模式中的完備的前向安全性\(forward secrecy\).

講白話一點, DH演算法就是在教你如何"安全地"告訴對方密碼而不用擔心密碼被竊聽. 通過在public channel交換一個資訊, 就可以建立一個可用於在public channel上安全通信的shared secret.

以下是擷取自wiki的DH流程簡圖:

![](/assets/DH-architecture.png)

最早提出的這個協定使用如下的離散對數\(Discrete Logarithm\)公式來做運算:

![](/assets/DH-formula.png)

公式裡的g, x, b, p都是實數, 其中:

* g: generator
* p: prime
* x: 通訊雙方各會有一個, 相異的私鑰

g跟p會是兩邊先溝通好的機制.

有了上述公式以後, 接下來描述DH演算法的運作流程:

1. Alice與Bob協定使用p = 23以及g = 5

2. Alice選擇一個secret number\(整數\), a = 6, 計算A = g^a mod p並傳給Bob

   1. A = 5^6 mod 23 = 8

3. Bob選擇一個secret number\(整數\), b = 15, 計算B = g^b mod p然後傳給Alice

   1. B = 5^15 mod 23 = 19

4. Alice計算s = B^a mod p

   1. 19^6 mod 23 = 2

5. Bob計算s = A^b mod p

   1. 8^15 mod 23 = 2

這邊你可以看到, Alice跟Bob都得到了相同的值, 這是因為在mod p之下, g^ab = g^ba.

![](/assets/DH-formula2.png)

或是這樣看:

![](/assets/DH-formula3.png)

在這段過程中, 只有a, b, g^ab = g^ba mod p是秘密的, 其餘部分如: p, g, g^a mod p以及g^b mod p都可以在public channel上傳遞. 當Alice跟Bob最後得出了shared secret\(s\)之後, 他們就可以把這個s當作對稱金鑰來進行雙方的加密通訊, 畢竟這個金鑰只有他們兩個人有辦法知道. 當然, 為了使這個過程更安全, 必須使用足夠大的a, b以及p, 否則竊聽者可以實驗所有g^ab mod p的可能值\(上述例子為p - 23, 所以表示最多總共有22個這樣的值, 這樣a跟b再大也沒意義\).若p是一個2~300位的數字, 且a跟b至少有100位長, 那麼除非你手上有量子電腦, 不然大概是不太可能從g, p, g^a mod p中去導出a的. 這個難題就是我們常說的離散對數問題. 這邊要注意的是g可以不用選太大的數字.

如果這樣還太抽象, 可以參考看看下面這張圖, 我覺得這張圖已經把DH演算法的精髓都畫出來了:

![](/assets/DH-example.png)

以下是從wiki上擷取下來的圖, 可以說明這段過程中通訊雙方以及竊聽者對於所有資訊的能見度:

![](/assets/D-H-process.png)

秘密的整數a和b在對話結束後就會被丟棄, 這就是為什麼DH演算法可以達到完備的前向安全性, 因為私鑰不會存在一個過長的時間而增加洩密的機率.

至於DH演算法的弱點, 基本上就是中間人攻擊, 畢竟在上面的過程中, 我們可以發現沒有任何跟身份驗證有關的手法用來防禦中間人. 故在使用DH演算法時, 最好是要能夠提供一個機制來驗證通訊雙方的身份, 才可以讓此機制更為完善. 譬如說對g^a與g^b進行簽名.

