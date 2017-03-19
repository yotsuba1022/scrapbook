# Messgae Digest & Hash Function

### Message Digest

所謂的Message Digest\(訊息摘要\), 即一段訊息, 或是某個特定資源/檔案的摘要, 這是一種類似於指紋\(fingerprint\)的概念; **理論上, 不同的訊息所產生出來的摘要基本上是不會相同的\(機率低到可以忽略\)**, 故可以用來驗證訊息在傳輸的過程中是否有被竄改過. 在當前的技術應用上, message digest通常是使用複查的hash function去計算出來的.

### Hash Function

在CS領域, 你應該可以常常看見hash function的蹤影, 像大家常提到的MD5\(Message Digest Algorithm 5\), SHA\(Secure Hash Algorithm\), MAC\(Message Authentication Code\), HMAC\(Hash-based Message Authentication Code\)... 等等.

基本上不論是何種Hash, 都具備以下幾點特性:

* 針對相同訊息去做計算, 都會產生出相同的結果
* 只有message digest, 是不能還原成原來的訊息的, 因此在演算法的設計上必須是不可逆的
* 不同的訊息所算出來的摘要必須是不同的

儘管上面提到的所有演算法都具備這些特性, 但這邊還是提一下一些要注意的點:

* MD5所計算出來的digest長度為128bits, 這個意思是說不同的訊息還是有1/\(2^128\)的可能性是會重複的
* SHA的強度比MD5更高, 其所計算出來的digest長度為160bits, 所以不同的訊息還是有1/\(2^160\)的可能性會重複
* 雖然以上兩種演算法都有一定機率重複, 但其機率已經低到可以忽略了, 所以實際應用上還是會看得到這兩種演算法. 此外, 此兩種演算法計算摘要時, 只需要原訊息即可, 不需要額外的資訊
* 相較於MD5與SHA, MAC/HMAC的差異是需要多一支加密用的金鑰, 故其演算法還是要以密碼學演算法為基礎, 只不過是可以不考慮可逆性的
* 若使用MAC/HMAC, 則永遠都會存在要考慮通訊雙方交換金鑰的問題



