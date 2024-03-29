---
title: 一件小事说起，谈谈游戏的那些“升级”策略
date: 2021-11-22 19:12:16
tags:
  - thoughts
  - minecraft
---

本来在[知乎](https://zhuanlan.zhihu.com/p/399605196)上发过了，想了想在这里再发一遍，万一没了呢。~当然我又改了改~

## 引言

> “升级”还是“不升级”，这是一个值得考虑的问题。~--L 姆雷特~

某个聊天记录截图中某人攻击了 Minecraft 1.7.10 Mod 的开发和维护群体，给了我一点启示。

在之前我也认为维护旧版本或者给旧版本增添新内容是个不可理喻的行为。在长期的分析和信息搜集后，我认为有几条开发者考量的因素（至少他们声称是这样）：

1. 新版本拥有更好的优化（如地图生成，网络通信上的优化）

2. 新版本的底层引入了更多的机制，可以在此基础上实现更多功能

3. 大部分玩家只玩一两个 mod，重度整合玩家是少数，所以迁移成本低

下面我们来一条一条分析。

1. 新版本只在 Java 新版本上进行了兼容或升级，但这主要是 Forge 的“功劳”，且 Forge 在 1.17 之前一直不官方声明推荐使用 Java 11。地图生成被抽象成了各种 Features，可是生成速度更慢了。原版的地图加载变得更慢，Enigmatica 6 的世界生成速度甚至不如 GTNH，经常造成假死。而网络通信上，Forge 进行了超级大改造，让开发门槛变得更高，服务端与本地端的兼容更加困难，经常需要完整同步所有文件才能解决 mismatch 问题。渲染上掉帧现象普遍，Sodium 告诉了我们什么是真正的渲染引擎。

2. Mod 要实现的新内容很大程度上并不版本依赖，你能说 IE 的多方块机器借鉴或直接使用了 MC 的什么新机制吗，都是旧有的 Block，TileEntity。你甚至可以在远古版本实现一个 IE。对于现在的 Mod 来说没有什么是不可能的。

3. 这是某些 Modder 的一厢情愿，请自行查阅 CurseForge 上各类轻重度整合包的下载量。**GTNH 是很火的。**

综上所述我认为这些理由站不住脚，不构成 Mod 进行大版本迁移的充分条件。笔者认为 Mod 进行大版本迁移有两种原因：

1. 习惯：升级到最新版是一种强迫症。

2. 风气：当一个 Mod 尤其是大型 Mod 进行版本升级后，它的附属和联动 Mod 就会被玩家或整合包作者催着升级。

其实就是这么个简单的逻辑，再加上上面那些可有可无的理由。然而**升级是苦痛的**，每个大版本都要换一次 mapping，至少要修好几天 bug，并且适应新的结构（尽管它们实现的功能完全一样或者就是改个名字）。在这一过程中程序员很少能体验到价值实现，心情不会太好。在以前版本的经验这个版本也都要寻求其他方法实现。

这就引出了不升级的原因。那就是**我当前这个版本还没优化好，为什么我要升级？**或者我只是想简单加个功能，慢慢慢慢再完善。这些灵感有时候就是会 slip away，如果你忙于升级的话你的大脑不会给你提供这些灵感。我并没有可靠证据，但事实说明一切。Reika 还是不断在更新，GTNH 还是不断在更新。他们都能不断实现更多玩法和功能，RoC 就有大变样。

那些选择升级的呢？MC 最近发布了一连串新版本，Forge 也进行了一连串大改。我相信大部分 Modder 都和我一样在迁移的路上身心俱疲。从结果来看也是这样。主流 Mod 如 IE，AE2，RefinedStorage 根本没有内容上的突破创新，只进行了材质的换皮。CoFH 系甚至退步了，只有 Mekanism 发展了核裂变线（也不完善）。我们可以看到 Fabric 欣欣向荣，然而只是在尝试打磨和消化旧有的玩法，并且优化性能或者仅仅是代码更漂亮。对于 Modder 来说，时间是宝贵的，大部分人只是作为一个 part-time job 来做这些事情。久而久之，很多经典 Mod 就跟不上了，我相信大家都知道例子。这就是一种 keep-up 的升级。

这一现象并不孤立存在于 Minecraft Mod 之中，Minecraft 本身也陷入了这样的困境。我现在笑称 Minecraft 是个 RPG 游戏了，一堆为 RPG 地图服务的改动，数据包，function，在 Mojang 本就拉垮的架构能力大笔一挥下加入了更大的屎山。Modder 都不知道自己怎么加一个地牢 Loot 了。

> Minecraft 难道是个 RPG 游戏吗？

Minecraft 本身就是在玩法上的创新起家的。最早它扩展了沙盒游戏的玩法，拥有更少的限制和更多的创造性。地狱更新，红石更新，末地更新，冒险更新，都让这个游戏的流程越来越多，可玩性越来越高。在 1.7 之后 Minecraft 已经和 Mod 密不可分了，它们作为一个整体共同进步。然而在 1.14 我认为 Mojang 选择了错误的路线，它不知道 Minecraft 要往何处去，于是进行了材质翻新，并且加入了大量冒险内容的扩充。要我说，材质翻新是走的最臭的一步棋，它强制改变了游戏的整体风格，并且 Modder 抽了风的一样也升级了材质，使得老玩家对于新版本的不熟悉和不信任感飙升。

（此处一定会有 n 多人反驳我说你怎么代表老玩家我怎么觉得材质好看，你开心就好，我说了是我的个人观点）这代表着 Minecraft 的一个重大的转变：Minecraft 本身去发展冒险内容，言下之意是，你们 Mod 去搞其他东西。可是冒险内容上的更新会很频繁，而 Mod 的创新速度正如前言是肯定跟不上的。“自大的更换材质”是一种对自己认识不清的一部分，因为不清楚自己的发展路线所以在盲目的更新。也同时暴露出，Mojang 根本不知道自己想干什么了。

要想理解为什么我说这是 Minecraft 走上下坡路的一个重要转折点，我们来看看其他游戏是怎样的。

## 来看看其他游戏吧

1. Paradox Interactive（P 社）游戏。使用 Clausewitz 引擎的所有游戏都受限于这个引擎几百年的祖传单核，拉垮机能和狗屎一样的脚本。P 社选择了卖各种 DLC 吞 Mod 的发展方向。游戏后期的玩法因为太卡而基本无法被开发，各种机能受限，许多简化操作的关键命令不实现。这些都是搁置了很多年的问题。最后只能靠推出一代一代新游戏来解决，但人生又有几个十年？而且拿走的都是特别精华的灵感。

2. 暴雪。暴雪属于是一把好牌玩成臭狗屎的典型。魔兽争霸，星际争霸，在编辑器事件上走到了玩家的对立面。各种“教你玩游戏”，知乎有很多其他相关文章，此处不再赘述。

3. R6。这砍一刀，那砍一刀，逻辑只有出场率和胜率，那你要干员干嘛，全部小叮当得了。

4. 命运 2。这砍一刀，那砍一刀，退环境，那你还接着出新内容干嘛，重新发布一遍游戏不就完了。

这些都是明眼看着走下坡路的游戏，他们的共同特点就是游戏核心玩法上没有创新，没有进步，不思进取，而且不接受在这上的 Modding，一旦凉凉就会成为鬼服游戏。

暴雪本来坐拥最大的 MMORPG（魔兽世界）和 RTS 群体，守望先锋和炉石传说也非常火爆，可是在游戏发布没多久就直接用完了灵感，进入摆烂模式，推出日复一日的换皮活动和年复一年的换色皮肤。要我说那么宏大的世界观都喂狗了，包袱回收还不如网红爽文，更别提郭德纲于谦了。

Minecraft 有一个巨大优势在于可以进行完全的，简单的 Modding，这是这些游戏所没有的。但是 Mod 社区终究是脆弱的，需要仔细考量和保护的。StarCraft 1.08b 的那些 EUD 玩法被暴雪一刀砍没了，现在又想通过模拟器加回来，然而以前那些大牛们早已不在了。所以其实这些决定真的只在一念间。

要我说啊，Minecraft 现在最大的突破口就是怎样实现虚拟世界中才能实现的想法，点子，进而组成无穷可能的 MMORPG 世界，**书写千万篇玩家们共同谱写的故事，而不是在单机自闭**。（当然你要能写出真 AI 也行）
