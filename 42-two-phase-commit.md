# Two-Phase-Commit \(a.k.a 2PC\)

#### Two-Phase-Commit

在分布式系統中, 每個節點雖然可以知道自己的操作是成功還是失敗, 卻無法知道其它的節點是成功還是失敗. 當一個transaction跨越了多個節點的時候, 為了保持transaction的ACID特性, 需要引入一個作為**協調者**的元件來統一掌握所有節點\(或稱**參與者**\)的操作結果並最終指示這些節點是否要把操作結果真正的進行commit的動作.

再來, 看一下兩階段各有什麼事情要做:

* 第一階段
  1. 協調者會問所有參與者, 是否可以執行commit操作了
  2. 各個參與者開始transaction執行的準備工作, 如: lock resources, 預留resources, 寫undo/redo log...等
  3. 參與者回覆協調者, 如果transaction的準備工作成功, 則回應"可以commit", 否則回應"拒絕commit"
* 第二階段
  * 如果所有的參與者都回"可以commit", 那麼協調者就向所有的參與者發送"正式commit"的命令. 參與者完成正式commit, 並釋放所有資源, 然後回應"完成", 協調者收集各個節點的"完成"回應後結束這個global transaction
  * 如果有任何一個參與者回覆"拒絕commit", 那麼協調者向所有的參與者發送"rollback操作", 則參與者們釋放所有資源, 然後回應"rollback完成", 協調者收集各節點的"rollback"回應後, 取消這個global transaction



