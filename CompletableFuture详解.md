## <font style="color:rgb(0, 0, 0);">Future使用对比</font>
**<font style="color:rgb(0, 0, 0);">Future表示一个异步计算的结果</font>**<font style="color:rgb(0, 0, 0);">。它提供了isDone()来检测计算是否已经完成，并且在计算结束后，可以通过get()方法来获取计算结果。在异步计算中，Future确实是个非常优秀的接口。但是，它的本身也确实存在着许多限制：</font>

+ **<font style="color:rgb(0, 0, 0);">并发执行多任务</font>**<font style="color:rgb(0, 0, 0);">：Future只提供了get()方法来获取结果，并且是阻塞的。所以，除了等待别无他法；</font>
+ **<font style="color:rgb(0, 0, 0);">无法对多个任务进行链式调用</font>**<font style="color:rgb(0, 0, 0);">：如果你希望在计算任务完成后执行特定动作，比如发邮件，但Future却没有提供这样的能力；</font>
+ **<font style="color:rgb(0, 0, 0);">无法组合多个任务</font>**<font style="color:rgb(0, 0, 0);">：如果你运行了10个任务，并期望在它们全部执行结束后执行特定动作，那么在Future中这是无能为力的；</font>
+ **<font style="color:rgb(0, 0, 0);">没有异常处理</font>**<font style="color:rgb(0, 0, 0);">：Future接口中没有关于异常处理的方法；</font>

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);">针对 Future 使用中的同时对多个异步任务进行编排的不足，java 使用 </font>**<font style="color:rgb(0, 0, 0);">CompletableFuture是Future接口的扩展和增强</font>**<font style="color:rgb(0, 0, 0);">。CompletableFuture实现了Future接口，并在此基础上进行了丰富地扩展，完美地弥补了Future上述的种种问题。更为重要的是，</font>**<font style="color:rgb(0, 0, 0);">CompletableFuture实现了对任务的编排能力</font>**<font style="color:rgb(0, 0, 0);">。借助这项能力，我们可以轻松地组织不同任务的运行顺序、规则以及方式。从某种程度上说，这项能力是它的核心能力。而在以往，虽然通过CountDownLatch等工具类也可以实现任务的编排，但需要复杂的逻辑处理，不仅耗费精力且难以维护。</font>



## 常用方法  

![](https://cdn.nlark.com/yuque/0/2024/png/26411187/1711117567688-9bc6cccc-2690-48ae-8d0f-421967547771.png)  






  


