# Garbage collection \(GC\)

在開始談GC之前, 先記住一個原則, 即"stop-the-world". "stop-the-world"是JVM中的VM thread的其中一項工作, 這個工作會停下整個程式以利GC的進行. 所以, 當"stop-the-world"啟動時, 所有跟GC無關的執行緒都會被停下來, 直到GC完成後才可以繼續執行, 所以你可能會發現很多人在調整GC的時候都會想要去減少"stop-the-world"的工作時間.

