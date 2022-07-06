我之前写代码的时候也一直会用到这两个方法，我总结的区别大致分了三个方面

首先就是sleep不会释放锁，而wait会释放锁

接着呢就是sleep不会解除cpu占用，wait会释放cpu资源

然后还有就是sleep会导致线程阻塞，时间到了之后，线程继续向下执行，但是wait必须配合notify或者notifyAll来唤醒