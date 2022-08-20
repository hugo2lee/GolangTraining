第一周作业：服务器优雅退出
作业题目
假设我们现在有一个 Web 服务。这个 Web 服务会监听两个端口：8080 和 8081。其中 8080 是用于监听正常的业务请求，它会被暴露在外部网络中；而 8081 用于监听我们开发者的内部管理请求，只在内部使用。

同时为了性能，我们在该服务中使用了本地缓存，并且采用了 write-back 的缓存模式。这个缓存模式要求，缓存在 key 过期的时候才将新值持久化到数据库中。这意味着在应用关闭的时候，我们必须将所有的 key 对应的数据都刷新到数据库中，否则会存在数据丢失的风险。

为了给用户更好的体验，我们希望你设计一个优雅退出的步骤，它需要完成：

监听系统信号，当收到 ctrl + C 的时候，应用要立刻拒绝新的请求；
应用需要等待已经接收的请求被正常处理完成；
应用关闭 8080 和 8081 两个服务器；
我们能够注册一个退出的回调，在该回调内将缓存中的数据回写到数据库中。