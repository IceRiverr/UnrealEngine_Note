> 初稿：2020年12月18日

## 思考
是否可以使用 UE4 的蓝图，加上改造的 Latent Function，从而实现 Pause 和 Condition 的目的？这样的话，每一个任务都使用一个 Actor 来代替。在 Actor 中通过函数来模拟全部流程。

将巫师3中取回睾丸的那一段任务，我来尝试一下，怎么在 UE4 中复现。
1. 将一个任务的流程，使用一个 Acotr 来代替，当需要有心的任务时，就 Spawn 这样一个任务 Actor 中到场景中即可。
2. 写几个 Latent Function，用来完成所有的任务。


看一下 UE4 是怎么来写 Delay 函数的。典型的就是 Delay 函数。

是否可以写一个 REDkit 的 pause 和 branch 的节点。怎么来写多输出的蓝图节点。

## 参考
https://unktomi.github.io/Latent-Actions-Cont/Cont.html

https://github.com/unktomi/Latent-Actions-Cont

[Creating Latent Functions for Blueprints in C++](https://www.casualdistractiongames.com/post/2016/05/15/creating-latent-functions-for-blueprints-in-c)

https://www.chiark.greenend.org.uk/~sgtatham/coroutines.html

http://johnmcroberts.com/index.php/2017/11/30/custom-delayed-blueprint-function-in-c/

[UE4 潜在事件](https://blog.csdn.net/qq_40756668/article/details/107150236)

[UE4 ActionGame知识点总结1-FLatentActionInfo以及FLatentActionManager使用](https://blog.csdn.net/hui211314ddhui/article/details/80710229)

[UE4 Latent Action(潜在事件)节点的基础创建方法](https://blog.csdn.net/flowersplug/article/details/80408493)

