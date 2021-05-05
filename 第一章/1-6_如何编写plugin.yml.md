[上一节](1-5_如何使用配置文件.md) [下一节](1-7_案例玩家进入信息等效果.md)

# 第一章 第六节 如何编写plugin.yml
参与编写者: SmallasWater MagicLu550
#### 建议学习时间: 10分钟
##### 学习要点: 了解plugin.yml内容

1. 关于plugin.yml

plugin.yml 是 nukkit加载插件的主要文件 在加载插件前必先加载plugin.yml

2. plugin.yml构成

```yaml
name: FirstPlugin             # nukkit运行时识别的插件名
main: net.noyark.www.Example  # 主类名称,不能以cn.nukkit开头
version: "0.0.1"              # 版本号
author: 你的名字，这里指作者名称  # 感谢qq1586235767的提议，作者名称不能使用纯数字，否则会发生类型转换异常，您可以使用字符串避免这个问题
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
3. 其他构成

我们通过拆解PluginDescription类，可以知道

PluginDescription.java
```
    private String name;
    private String main;
    private List<String> api;
    private List<String> depend = new ArrayList<>();
    private List<String> softDepend = new ArrayList<>();
    private List<String> loadBefore = new ArrayList<>();
    private String version;
    private Map<String, Object> commands = new HashMap<>();
    private String description;
    private final List<String> authors = new ArrayList<>();
    private String website;
    private String prefix;
    private PluginLoadOrder order = PluginLoadOrder.POSTWORLD;
```
其中load属性分为POSTWORLD和STARTUP，他们区别官方在注释说明了

PluginLoadOrder.java
```
/**
     * 表示这个插件在服务器启动时就开始加载。<br>
     * Indicates that the plugin will be loaded at startup.
     *
     * @see cn.nukkit.plugin.PluginLoadOrder
     * @since Nukkit 1.0 | Nukkit API 1.0.0
     */
    STARTUP,
    /**
     * 表示这个插件在第一个世界加载完成后开始加载。<br>
     * Indicates that the plugin will be loaded after the first/default world was created.
     *
     * @see cn.nukkit.plugin.PluginLoadOrder
     * @since Nukkit 1.0 | Nukkit API 1.0.0
     */
    POSTWORLD
```
config里有一个load,分别为STARUP与POSTWORLD,前者使插件加载在地图之前,后者为使插件加载在地图之后,如果对地图加载有需求的话，必须填写POSTWORLD,否则将无法获取level

您也可以添加自己的网站: website属性

[上一节](1-5_如何使用配置文件.md) [下一节](1-7_案例玩家进入信息等效果.md)
