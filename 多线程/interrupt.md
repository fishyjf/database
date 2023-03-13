没有任何语言方面的需求一个被中断的线程应该终止。中断一个线程只是为了引起该线程的注意，被中断线程可以决定如何应对中断。interrupt()方法不会中断一个正在运行的线程。

**如果线程在调用 Object 类的 wait()、wait(long) 或 wait(long, int) 方法，或者该类的 join()、join(long)、join(long, int)、sleep(long) 或 sleep(long, int) 方法过程中受阻，那么调用interrupt方法会生效，其中断状态将被清除，它还将收到一个InterruptedException异常。如果目标线程被IO流或这NIO中的Channel所阻塞，那么调用interrupt方法同样会生效，IO会被中断返回ClosedByInterruptException。如果以上条件都不满足，则设置线程的中断状态（isInterrupted）为true**

synchronized在获锁的过程中是不能被中断的，意思是说如果产生了死锁，则不可能被中断。

interrupted()是个Thread的static方法，用来恢复中断状态。


