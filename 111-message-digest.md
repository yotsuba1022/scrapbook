# Messgae Digest & Hash Function

### Message Digest

所謂的Message Digest\(訊息摘要\), 即一段訊息, 或是某個特定資源/檔案的摘要, 這是一種類似於指紋\(fingerprint\)的概念; **理論上, 不同的訊息所產生出來的摘要基本上是不會相同的\(機率低到可以忽略\)**, 故可以用來驗證訊息在傳輸的過程中是否有被竄改過. 在當前的技術應用上, message digest通常是使用複查的hash function去計算出來的.

### Hash Function

在CS領域, 你應該可以常常看見hash function的蹤影, 像大家常提到的MD5\(Message Digest Algorithm 5\), SHA\(Secure Hash Algorithm\), MAC\(Message Authentication Code\), HMAC\(Hash-based Message Authentication Code\)... 等等.

基本上不論是何種

