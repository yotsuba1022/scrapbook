# Garbage collection \(GC\)

在開始談GC之前, 先記住一個原則, 即"**stop-the-world**". "stop-the-world"是JVM中的VM thread的其中一項工作, 這個工作會停下整個程式以利GC的進行. 所以, 當"stop-the-world"啟動時, 所有跟GC無關的執行緒都會被停下來, 直到GC完成後才可以繼續執行, 所以你可能會發現很多人在調整GC的時候都會想要去減少"stop-the-world"的工作時間.

現在來回想一下你平常會怎麼讓GC動作, 就我個人而言, 我會直接想到的有以下兩種:

1. 把reference設成null
2. 直接呼叫System.gc\(\) -&gt; 沒事真的別用這個

而GC是怎麼樣認定要去收垃圾的呢? 基本上有兩個前提:

1. 物件很快就會變成**unreachable**的狀態
2. old generation對new generation的reference只剩下少量存在時

如此的假設被稱為weak generation hypothesis. 在Java1.3之後的預設JVM裡, 便區分出了young generation與old generation.

Young generation: 大部分的新建物件都會座落在此區塊. 由於多數的物件在建立之後很快就會變成unreachable, 所以這些物件都會被建立在這裡, 然後消失. 對於這樣的情形\(我指在這區塊消失\), 我們稱之為輕度GC\(**minor GC**\).

Old generation: 對於那些沒有變成unreachable然後又活過young generation的物件就會進到這個區塊. 而通常來講, old generation的空間是大於young generation的, 正因為如此, 跟young generation比起來, 這個區塊比較少發生GC. 不過當物件在這個區塊消失時, 我們稱之為重度GC\(**major GC**\)/完整GC\(**full GC**\).

Permanent generation: 又稱為"method area", 這裡放的基本上都是class, character string, 通常是不會發生物件活過old generation然後又進到這個區塊的. 不過這個區塊也是會有GC發生的, 一樣歸類為major GC.

再來這邊會有一個問題, 如果old generation的物件必須要參照到young generation的物件該怎麼辦呢? 在此要介紹一個新的名詞, 叫做"card table", card table是存在於old generation中的, 其為一個512byte的區塊. 每當一個old generation的物件參照到young generation的物件時, 便將其記錄在此區塊中. 所以當young generation發生GC的時候, 只要來看這張table即可, 而不用再去一個一個掃過所有old generation的物件.

