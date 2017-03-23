# Two-Phase-Commit \(a.k.a 2PC\)

#### Two-Phase-Commit

在分布式系統中, 每個節點雖然可以知道自己的操作是成功還是失敗, 卻無法知道其它的節點是成功還是失敗. 當一個transaction跨越了多個節點的時候, 為了保持transaction的ACID特性, 需要引入一個作為協調者的元件來統一掌握所有節點\(或稱參與者\)的操作結果並最終指示這些節點是否要把操作結果真正的進行commit的動作.

