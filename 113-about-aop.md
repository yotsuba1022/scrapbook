# Aspect-Oriented Programming

AOP, 又名Aspect-Oriented Programming, 中文有人說方面\(側面\)導向, 我這邊的介紹基本上會儘量用原文, 因為我覺得中文翻過來其實不是很直觀. 關於AOP, 個人認為它基本上就是一種interceptor\(攔截器\)技術, 你可以指定在程式中某些特定的場合要觸發某些特定的行為. 後面會慢慢提到.

試想一下, 今天你想要為你的application中的每個method都去log開始與結束情形. 那你可能就會把這段log的程式重構成一個特定的class \(e.g. MyLogger\), 儘管如此, 你還是需要在每個你要log的method上呼叫這個class兩次, 這基本上是很OO的作法了, 但聽起來還是不夠好; 透過AOP,  你可以很簡單的指定MyLogger class上的特定方法在application中的每個method被呼叫的時候也一並被執行.

這裡希望你能夠先理解一點, AOP跟OOP之間的關係並非是競爭\(compete\), 而是AOP補足\(complement\)了OOP . OOP確實解決了很多我們現實生活中碰到的問題, 但在前一段的例子中, 你可以看到當出現這種跟橫切\(crosscutting\)邏輯有關的時候, 尤其是scale較大的情況下, OOP就顯得有點無力了, 你可以想想看為所有method都加上前後logging method是多繁瑣且重複的工, 這其實滿不OO的. 當然, 你也不可能用AOP去開發整個application, 畢竟技術的目的不同. 所以在用OOP開發的過程中, 我們可以在碰到crosscutting的情況下借助AOP的特性讓開發更為順暢.

#### AOP Concepts

AOP的概念是由很多東西組成的, 這邊就一一條列出來:

* Joinpoints:
* Advice:
* Pointcus:
* Aspects:
* Weaving:
* Target:
* Introduction:



#### Types of AOP



