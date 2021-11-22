---
title: Minecraft performance tuning and more
date: 2021-11-22 18:44:10
tags: minecraft
---

多年的 Minecraft 服务器运维经验可以整理一下了，当然也讲点别的。

本文内容针对 Forge 1.12.2 版本，其他 Minecraft 版本同理但有所出入

Minecraft 服务器的性能主要有三个大开销，下面分别来讲讲三个部分以及它们的优化策略。

1. TileEntity Tick
    > Tick 的实现可以在 `WorldServer#tick` 找到。
    
    众所周知的开销大户，在一个大型整合包的中后期档中占用了主要部分。可以使用 `/forge track start tileentity 10` `/forge track tileentity` 来统计十秒内的 TileEntity tick 时间占用。找到对性能影响最大的几个方块拆除之。

    常见情况：匠魂冶炼炉满了造成的巨幅卡顿，Ender IO 机器的 _输入/输出_ 面放了一个容器。

    当然更常见的情况是人多了，机器多了，虽然每个只占用 1ms，但是积攒起来还是很容易达到 50ms (20 TPS) 的上限。这个时候可能需要和玩家沟通，把生产线拆除甚至换成创造箱子等。如果服务器玩家组分时段上线，也可以考虑限制或禁止区块强制加载。

2. Entities Tick
    
    实体的情况比较复杂。玩家身上穿了过多能与世界交互的装备（如吸铁石）就会造成卡顿，同样需要与这些玩家进行沟通。其他情况下一般需要清理掉落物品，减少刷怪，安装 AIImprovements 模组，在后期档直接开启和平模式等。实体可能只占用 10ms 的 Tick 时间，但是聊胜于无吧。

3. World Generation

    跑图卡顿，在 1.13 之前的单线程世界生成所存在的问题。考虑删除 BOP，RTG 等大量修改世界生成的模组。

4. 其他部分

    - 虽然 Minecraft 采用了先进的 Netty 异步网络数据处理方式，但是仍然可能发生从网络线程引入的卡顿。典型的情况是一个超大型 _应用能源_ 网络，打开终端之后会因为处理，发送，接收大量数据而造成卡顿。一个带有特殊渲染模型的多方块结构也可能发生这种现象，因为要实时获取多方块机器数据。这种情况下考虑开启 BBR 算法，AE 拆分子网络等。

    - JVM 调优可以直接使用 [Thermos](https://cyberdynecc.github.io/Thermos/install) 给出的参数：`java -XX:+UseG1GC -XX:+UseFastAccessorMethods -XX:+OptimizeStringConcat -XX:MetaspaceSize=1024m -XX:MaxMetaspaceSize=2048m -XX:+AggressiveOpts -XX:MaxGCPauseMillis=10 -XX:+UseStringDeduplication -XX:hashCode=5 -Dfile.encoding=UTF-8`

    - 推荐使用 HotSpot JVM 而不是奇奇怪怪的各种 JVM，你不知道这些模组会用到什么奇怪的黑科技。

    - **不要使用洋垃圾 Xeon 开服！**

## 后记

我们很少探索过大型整合包的后期档，很少探索大型 Mod 整合包带来的科技魅力，便是因为后期的性能问题。倘若能在性能和复杂的生产线之间做出很好的平衡，也许我们就可以欣赏到 Minecraft 中那种创造的魅力。
