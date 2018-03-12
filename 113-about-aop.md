# Aspect-Oriented Programming

AOP, 又名Aspect-Oriented Programming, 中文有人說方面\(側面\)導向, 我這邊的介紹基本上會儘量用原文, 因為我覺得中文翻過來其實不是很直觀. 關於AOP, 個人認為它基本上就是一種interceptor\(攔截器\)技術, 你可以指定在程式中某些特定的場合要觸發某些特定的行為. 後面會慢慢提到.

試想一下, 今天你想要為你的application中的每個method都去log開始與結束情形. 那你可能就會把這段log的程式重構成一個特定的class \(e.g. MyLogger\), 儘管如此, 你還是需要在每個你要log的method上呼叫這個class兩次, 這基本上是很OO的作法了, 但聽起來還是不夠好; 透過AOP,  你可以很簡單的指定MyLogger class上的特定方法在application中的每個method被呼叫的時候也一並被執行.

這裡希望你能夠先理解一點, AOP跟OOP之間的關係並非是競爭\(compete\), 而是AOP補足\(complement\)了OOP . OOP確實解決了很多我們現實生活中碰到的問題, 但在前一段的例子中, 你可以看到當出現這種跟橫切\(crosscutting\)邏輯有關的時候, 尤其是scale較大的情況下, OOP就顯得有點無力了, 你可以想想看為所有method都加上前後logging method是多繁瑣且重複的工, 這其實滿不OO的. 當然, 你也不可能用AOP去開發整個application, 畢竟技術的目的不同. 所以在用OOP開發的過程中, 我們可以在碰到crosscutting的情況下借助AOP的特性讓開發更為順暢.

#### AOP Concepts

AOP的概念是由很多東西組成的, 這邊就一一條列出來:

* **Joinpoints**: 所謂的jointpoint就是在application執行的過程中被良好定義\(well-defined\)的一個點. 典型的joinpoints如一個對方法的呼叫, 類別的初始化或物件實例化等. Joinpoint是AOP的核心概念且其定義了你可以於application中的何處透過AOP插入額外的邏輯.
* **Advice**: 在特定的joinpoint上執行的程式就是advice. Advice有很多種, 如before, after等. 在OOP的角度來說, advice是以class中的method的形式存在的.
* **Pointcus**: 一個Pointcut就是一組joinpoints的集合\(collection\), 其用意是用來定義何時要執行某個advice. 通常你可以透過複雜的關係去構成\(compose\)pointcuts, 進而在advice被執行時去達到約束的效果.
* **Aspects**: 一個aspect就是advice跟pointcuts的組合\(combination\). 這個組合定義了一段需在application中某處被執行的邏輯.
* **Weaving**: 這就是實際上在合適的時間點於application中插入aspects的流程. 縫合\(weaving\)的時間點基本上可以分為三種:
  * **Compile time**: Static AOP的做法, 通常會被當作是build process的額外步驟 
  * **Runtime**: Dynamic AOP的做法
  * **Load-time weaving \(LTW\)**: AspectJ提供, 這比較複雜, 基本上就是當bytecode被classloader load的時候進行縫合
* **Target**: 當一個物件的execution flow被某些AOP process更改了, 這個物件就稱為target object \(a.k.a. advised object\). 
* **Introduction**: 這是一種可以供你更改一個物件的結構的流程\(process\), 講白一點就是改變物件行為, 但是卻不用修改該類別的程式碼. 這基本上就是在執行時期動態加入方法或行為的方式.

#### Types of AOP

AOP基本上只有分為以下兩種, 主要的差別就是weaving的時間點以及weaving的手段

* **Static AOP**: 大多數的AOP都是static的, 對static AOP來說, weaving就是build process的額外步驟. 用Java的角度來看, 透過static AOP達成的weaving基本上都會更改到application的bytecode, 若有必要的話, 也會改變或著是擴張原本的code. 很明顯地, 這是效能比較好的做法, 畢竟最終的結果就是bytecode, 而不必於runtime的時候去決定advice的執行時機.

  這種做法的缺點就是不管你對aspect做了多少變更, 儘管只是多加了一個很小的joinpoint, 你都必須要重新compile你的application, 最常見的例子就是AspectJ的compile-time weaving了.

* **Dynamic AOP**: Spring AOP就是屬於這一種, weaving的動作會在runtime的時候動態地執行. 至於細節如何達成, 則要依實作而定. 對Spring來說, 主流的方式就是**為所有的advised object創造proxy**, 並且允許advice在需要的時候被呼叫.

  這種做法的一個小缺點就是效能沒有static AOP那麼好, 但好處是很明顯的, 你不必因為任何一點對aspect的更改就要重新compile你的application.

#### **Proxies in Spring AOP**

* CGLIB proxy
* JDK dynamic proxy

#### Advice in Spring

* Before
* After returning
* After \(finally\)
* Around
* Throws
* Introduction



