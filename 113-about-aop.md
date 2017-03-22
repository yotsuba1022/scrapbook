# Aspect-Oriented Programming

AOP, 又名Aspect-Oriented Programming, 中文有人說方面\(側面\)導向, 我這邊的介紹基本上會儘量用原文, 因為我覺得中文翻過來其實不是很直觀. 關於AOP, 個人認為它基本上就是一種interceptor\(攔截器\)技術, 你可以指定在程式中某些特定的場合要觸發某些特定的行為. 後面會慢慢提到.

試想一下, 今天你想要為你的application中的每個method都去log開始與結束情形. 那你可能就會把這個log的程式重構成一個特定的class, 儘管如此, 你還是需要在每個你要log的method上呼叫這個class兩次, 這基本上是很OO的作法了, 但聽起來還是不夠好; 透過AOP,  你可以很簡單的指定log class上的方法在application中的每個method被呼叫的時候也一並被執行.

