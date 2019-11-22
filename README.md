# 在看本教程前，一定要阅读这个readme
# Nukkit教程整理

#### 友情项目

[基于java开发的SCP基金会服务端](https://github.com/jsmod2-java-c/JSmod2-Core)

#### 关于主要作者

作者实际上已经退坑了，但是不影响他写这个教程，只是提醒以下几点。
- 不要加作者问其如何搭建服务器
- 不要找作者定制插件
- 总之，作者除了写这个教程，将不会参与任何和mc有关的活动和交易
- 而且一定学完java基础教程，再看本教程，作者不会回答任何有关基础的问题
#### 关于本教程

Nukkit官方说过: nukkit是一款高性能的核能驱动的minecraft基岩版服务器,它的
速度更快，性能相比pocketMine更高。
```
Nukkit is nuclear-powered server software for Minecraft Bedrock Edition. It has a few key advantages over other server software:

Written in Java, Nukkit is faster and more stable.
Having a friendly structure, it's easy to contribute to Nukkit's development and rewrite plugins from other platforms into Nukkit plugins.
Nukkit is under improvement yet, we welcome contributions.
```

目前，,依托java语言的健壮性,nukkit形成了强大的生态，并且出现了很多分支，目前使用最广泛的分支是[NukkitX](http://nukkitx.com)

感谢您能观看我们编写的教程。该教程在编写阶段，更新间隔可能会很长。
请谅解。该教程是为了简化目前很多晦涩难懂的教程，使得有过java语法基础的朋友
学习nukkit和了解nukkit。目前nukkit的学习难度主要在于其资料过少。我们将会
自己整理和参考其他相关资料来编写该教程，也感谢您参与编写，造福大家

同时，转发本教程应当附上github原地址，不得任意转发和搬运，以及商业使用。
注意: 对于java的语法有一定了解可以观看此教程。
本教程不涉及语法，只涉及nukkit的各种库的解释以便于开发
您所需要掌握的最基本的java基础体系
```
  --基础部分
    -- 语法
      -- 变量定义
      -- 方法定义
      -- 基本表达式
        -- 逻辑表达式
        -- 算数表达式
      -- 流程控制
        -- 条件语句
        -- 循环语句
        -- 选择语句
    -- 面向对象
      -- 类，对象的概念
      -- 定义类和声明对象
      -- 包的概念
      -- 匿名内部类
      -- 接口
      -- 抽象类，抽象方法
      -- lambda表达式
      -- 面向对象的三大特征
        -- 继承
        -- 封装
        -- 多态
      -- 枚举
      -- 注解
   -- 类库
      -- 反射
      -- 多线程
      -- 字符串操作
      -- 数字包装类
      -- io流
        -- bio
        -- nio
      -- 套接字Socket
        -- udp
        -- tcp
      -- 时间类库
      -- 系统类库

```
未来，我们会开发nukkit-d,是对于nukkit设计思想的整理和重新实现，以解决目前nukkit维护难的问题
#### 关于我们

感兴趣者可以随时发送pr等
其中本教程不定期更新，大概寒假时期更新最快
相关意见可以联系843983728@qq.com或者加qq:843983728

这里将会整理一系列的nukkit教程，感谢学习，由Server-Founder推出

qq群号: 931210534

![qq群](images/0-00.png)

#### 贡献标准和须知

1. 教程要求尽量自然，易懂，符合本项目所追求的"人人可以学习nukkit"的宗旨
2. 编写教程可以在参与编写者添加自己的名字，若没有可以自己手动加入，如第一章所同
3. 照搬其他教程原文需要得到作者同意，并且在下面注明参考文献。
4. 若发现侵权行为，与本项目领导者无关，但我们会积极配合找到侵权者
5. 请尽量以第一章为范本编写您的内容。
6. 如果有知识缺点，请在readme里注明这个缺点出处，写在知识缺点里,知识缺点在章节中标注。
7. 教程补充首要的是解决知识缺点

#### 知识缺点:
   - 关于fallBackPrefix的作用 - 第二章和第四章 [1]
#### 目录
- 第一部分 基础准备
  - [如何搭建开发环境](第一章*如何搭建环境.md)
  - [插件要素](第二章*插件要素.md)
  - [如何编写监听器](第三章*如何编写监听器.md)
  - [如何编写指令](第四章*如何编写命令.md)
  - [如何使用配置文件](第五章*如何使用配置文件.md)
  - 如何编写plugin.yml
  - 案例： 实现玩家进入功能
- 第二部分 nukkit的工具和各种事件介绍
  - 主要的事件的介绍
  - 事件相关方法
  - 计时器的介绍
  - Server类和PluginManager类
  - 各种实体类的方法介绍
  - 各种工具类的介绍
- 第三部分 nukkit相关实例
- 第四部分 nukkit原理剖析




