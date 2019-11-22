[上一章](第五章*如何使用配置文件.md) [下一章]()
# 第一部分 第六章 如何编写plugin.yml
参与编写者: SmallasWater
#### 建议学习时间: 10分钟
##### 学习要点: 了解plugin.yml内容
1. 关于plugin.yml
plugin.yml 是 nukkit加载插件 主要文件 在加载插件前必先加载

2. plugin.yml构成
```yaml
name: FirstPlugin             # nukkit运行时识别的插件名
main: net.noyark.www.Example  # 主类名称
version: "0.0.1"              # 版本号
author: 你的名字，这里指作者名称
api: ["1.0.9"]                # 早期nukkit api为1.0.0，
# 目前大概为1.0.9
depend: ["EconomyAPI"]        # 依赖的插件名称 添加后如果Plugins文件夹不存在添加的插件则关闭本插件
loadbefore: ["EconomyAPI"]    # 在xx插件之后加载 一般用作解决调用依赖库出现的ClassCastExpection
description: My first plugin  # 介绍
commands:                     # Commands指令列表 
  fp:                          # 指令名称 不要加 / 
   usage: "/fp help"           # 指令的用法 当onCommand返回false时 输出 usage内容
   description: "指令介绍"      # 指令的介绍
   permission: FirstPlugin.fp  # 指令权限 如果你希望插件仅允许 op执行 可以尝试这个
   aliases: []                 # 指令别名 可以增加中文名称
  permissions:
    FirstPlugin.fp:             # 权限名称
      description: ""          # 权限的介绍
      default: op              # 权限限制 op / notop   notop为非op可执行 op 为仅限op执行
```
[上一章](第五章*如何使用配置文件.md) [下一章]()
