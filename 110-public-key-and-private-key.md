# Public Key & Private Key

假設今天有兩把鑰匙, 一把叫A, 一把叫B. 然後因為我比較喜歡B, 所以就把B保留起來不跟任何人說, 故**B是私鑰\(private key\)**; 同時, 我跟大家說, **A是公鑰\(public key\)**. 然後今天有一份文件C, 不能讓別人看到, 所以我就用A去對C作加密\(**encrypt**\). 若有人找到這份文件, 但是他並不知道B\(解密用的鑰匙\), 所以他還是無法得知C的具體內容, 只有我自己可以透過B去解密\(**decrypt**\)C文件, 這樣我就可以保護資料了. 試想以下情境: 若我和某D進行通訊, 某D使用公鑰A去加密我們之間通訊的內容, 如此一來, 就算有人知道了我們通訊的內容\(加密過後的\), 他也無法得知詳細的具體內容, 只有我可以透過私鑰B去解開通訊的內容, 這樣我跟D之間就可以傳送加密的數據了.

如此一來, 我們知道**用公鑰加密, 並用私鑰來解密**, 即可解決安全傳輸的問題了.

![](/assets/publickey_privatekey001.png)

那如果我用私鑰加密一段資料\(當然只有我可以用私鑰加密, 因為只有我知道B是我的私鑰\), 結果大家都看得到我加密後的內容了, 因為大家都知道公鑰是A, 那這有什麼意義呢?

試想一種情境, 若某D跟我說, 有個人冒充我給他發訊息, 該怎麼辦呢? 這邊我就可以把我要發送的訊息, 內容是K, 用我的私鑰B去加密, 加密後的內容是K', 把K'發送給某D, 再告訴某D解密看看是不是K. 某D使用我的公鑰A去解密後, 發現真的是K. 這代表說, **能夠用我的公鑰去解密的資料, 必然是用我的私鑰去加密的, 因此某D就可以確認說這東西確實是我發的了**. 這種確認發送方身份的過程稱為**數位簽章**\(**digital signature**\)\[見文末的補充區塊\]. 當然具體的過程會更複雜一點, 不過這種用私鑰來加密的過程, 用途就是數位簽章了, 或是說欲確定**不可否定性\(non-repudiation\)**.

![](/assets/digital_signature.png)

整理一下:

* 公鑰和私鑰會成對出現, 其之間是數學相關的
* 公開的密鑰叫做公鑰, 只有自己知道的叫做私鑰
* 用公鑰加密的資料只有相對應的私鑰可以解密
* 用私鑰加密的資料只有相對應的公鑰可以解密
* 若可以用公鑰解密, 則必然是相對應的私鑰加的密
* 若可以用私鑰解密, 則必然是相對應的公鑰加的密
* 此類型的加解密方式稱為非對稱式加密, 常見應用如RSA

所以:

* 公鑰與私鑰會成對出現
* 私鑰只有自己知道
* 大家可以用我的公鑰對我發加密的訊息
* 大家可以用我的公鑰解密訊息的內容, 如果可以解開, 說明是經過我的私鑰加密的了, 如此也可以驗證說訊息確實是我發的了.

結論:

* 用公鑰加密資料, 用私鑰解密資料
* 用私鑰加密資料\(數位簽章\), 用公鑰來驗證數位簽章
* 通常在實際的應用中, 公鑰不太會單獨出現, 基本上是以數位簽章的方式出現, 這是為了考量公鑰的安全性與有效性

### 補充

關於**數位簽章\(digital signature\)**:

* 使用情境:  
  若A要發送訊息給B, 那麼B要怎麼確認這個訊息真的是由A所發送的呢?

* 解法:  
  A在發送訊息前, 先用自己的private key將訊息加密, 之後再傳送給B, 此時B再利用A的public key對收到的訊息解密.  
  根據對稱式金鑰加密, 我們知道: **若訊息可以正確地被解密, 我們就能確定訊息是由A所發出的**; 儘管這個訊息在傳送的過程中被C給攔截了, 且C用A的public key將這段訊息還原成原本的訊息, 但C還是沒有辦法偽裝成A來發送訊息, **因為C沒有A的public key**. 因此, 數位簽章的主要作用就是可以確保**不可否定性\(non-repudiation\)**, 其背後的意義表示說**授權機制可以很容易地被建立起來**.



