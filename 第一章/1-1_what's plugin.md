# 第一章 第一节 什么是插件
参与编写者: smartcmd
#### 建议学习时间: 50分钟
##### 学习要点: 了解nukkit插件结构，以及nukkit是如何加载它们的

1.nukkit插件结构:

一个完整的nk插件，它至少要包含以下2个部分:
```
 plugin.yml
        (包含了这个插件的基本信息，其中必须包含:
        name: FirstPlugin             # nukkit运行时识别的插件名
        main: net.noyark.www.Example  # 主类名称
        version: "0.0.1"              # 版本号
        author: 你的名字，这里指作者名称
        api: ["1.0.8"]                # 早期nukkit api为1.0.0，
                                      # 目前大概为1.0.8
        description: My first plugin  #介绍)
 继承于PluginBase的主类
```
如果插件缺失以上部分的任意部分，nk将无法加载此插件

2.nukkit的插件加载流程
```
   1.读取插件主类（load阶段）调用主类的load()方法
   2.启用插件（enable阶段）调用主类的enable()方法
```
建议将插件的初始化流程放在enable方法下，load方法被调用时nk本身还未完成启动，此时进行一些操作（例如获取level）很容易出现空指针

[上一节](1-0_前言.md)    [下一节](1-2_了解PluginBase.md)