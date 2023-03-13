让当前线程进入等待状态。
wait方法必须从同步（synchronized）的上下文调用，即同步块或方法，否则会抛出IllegalMonitorStateException异常。

**线程判断共享资源状态的循环应避免使用if而使用while**