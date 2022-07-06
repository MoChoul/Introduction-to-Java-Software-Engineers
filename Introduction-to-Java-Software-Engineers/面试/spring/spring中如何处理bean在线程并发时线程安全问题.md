在一般情况下，只有无状态的 Bean 才可以在多线程环境下共享，在 Spring 中，绝大部分 Bean 都可以声明为 singleton 作用域，因为 Spring 对一些 Bean 中非线程安全状态采用 ThreadLocal 进行处理，解决线程安全问题。

ThreadLocal 和线程同步机制都是为了解决多线程中相同变量的访问冲突问题。同步机制采用了“时间换空间”的方式，仅提供一份变量，不同的线程在访问前需要获取锁，没获得锁的线程则需要排队。而 ThreadLocal 采用了“空间换时间”的方式。

ThreadLocal 会为每一个线程提供一个独立的变量副本，从而隔离了多个线程对数据的访问冲突。因为每一个线程都拥有自己的变量副本，从而也就没有必要对该变量进行同步了。ThreadLocal 提供了线程安全的共享对象，在编写多线程代码时，可以把不安全的变量封装进 ThreadLocal。

我们项目中的拦截器里就有这样的逻辑，在我们微服务中，网关进行登录以及鉴权操作，具体的微服务中需要用到token去解析用户信息，我们就在拦截器的preHandler里定义了threadlocal，通过token解析出user的信息，后续controller以及service使用的时候，直接从threadlocal中取出用户信息的，在拦截器的afterCompletion方法中清理threadlocal中的变量，避免变量堆积消耗内存