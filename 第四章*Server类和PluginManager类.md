[上一章](第三章*计时器的介绍.md) [下一章](第五章*各种实体类的方法介绍.md)
# 第二部分 第四章 Server类和PluginManager类
参与编写者: MagicLu550
#### 建议学习时间: 20分钟
##### 学习要点: 了解Server类和PluginManager类

一. Server

1.概述

Server类是插件几乎所有接口的入口，几乎一切的接口都是基于这个类获得的,而在nukkit中,Server类
是作为一个对象单独存在,并且不允许外部调用其构造方法,但可以根据getInstance方法或者插件主类提供的
getServer方法可以获得,这里我们提到了两个获得Server的方法
```
    Server.getInstance();
    this.getServer();//this.getClass() == mainClass
```
Server对象是在Nukkit类里完成初始化的,所以我们不需要担心Server对象是否存在的问题.加载插件
前,Server对象已经存在.一切启动的初始化操作都在Server的构造方法中完成.对于Server的复杂原理,
这里不过多赘述,可以参见第四部分的内容

Nukkit.java Line. 108-115
```

        try {
            if (TITLE) {
                System.out.print((char) 0x1b + "]0;Nukkit is starting up..." + (char) 0x07);
            }
            new Server(PATH, DATA_PATH, PLUGIN_PATH, language);
        } catch (Throwable t) {
            log.throwing(t);
        }
```
2. Server类的常用方法
    * addOp(String) 可以添加op管理员的名称,name是玩家名称
    * addWhitelist(String) 可以添加白名单
    * batchPackets​(Player[], DataPacket[]) 批量发送数据包,后面[数据包发送篇](第七章*如何发送数据包.md)详细讲解
    * broadMessage(String) 发送服务器广播信息，所有玩家可见
    * addRecipe​(Recipe) 添加配方,这个配方指包括合成、炉子、炼药台等使用的配方。
    * broadcastPacket​(Player[], DataPacket) 向所有玩家广播数据包
    * forceShutdown() 强制关闭服务端
    * doAutoSave() 自动保存
    * generateLevel​(String) 产生一个level(世界),String为名字,返回创建是否成功
    * getAllowFlight() 获得这个服务器是否是允许飞行的
    * getApiVersion() 获取插件api的版本
    * getCommandAliases() 将返回以(一个指令名对应着多个别名)为一对的Map集合
    * getCommandMap() 获取指令Map,通过它可以注册一些命令,[前面已经说到过](第四章*如何编写命令.md)
    * getDataPath() 获取服务端的数据目录
    * getDefaultGamemode() 获取服务端的默认模式(如创造模式等)
    * getDefaultLevel() 获取默认的世界对象,如World
    * getDifficulty() 获得游戏难度，如和平模式等
    * getIp() 获得服务端的ip地址
    * getIpBans() 获得封禁的信息(ban)
    * getLanguage() 获得服务端默认语言(如zh等)
    * getLevelByName(String) 通过世界名称获得世界对象
    * getMaxPlayers() 获得最大人数
    * getMotd() 获得服务端的motd
    * getName() 获得服务端的名称
    * getNameBans() 获得迸封禁的玩家表
    * getOfflinePlayer(String) 通过玩家名称得到不在线玩家
    * getOnlinePlayers() 获得在线玩家,UUID都是唯一的标识符
    * getPlayerExact(String) 通过名称获得一个确切的玩家​
    * getPluginManager() 获得插件管理器
    * getOps() 获得管理员清单
    * getPort() 获得端口
    * getPluginPath() 获得插件文件夹的位置
    * getScheduler() 获得任务表，可以注册有关多线程之类的东西
    * getSubMotd() 获得附属的motd
    * hasWhitelist() 是否有白名单
    * isOp​(String) 判断一个玩家是否是op,String为玩家名称
    * reload() 重启服务端
    * reloadWhitelist() 重新加载白名单
    * removeOnlinePlayer​(Player) 删除在线玩家
    * removeOp​(String) 删除指定管理员
    * removeWhitelist​(String) 删除一个玩家的白名单
    * shutdown() 关闭服务端
    * unloadLevel​(Level) 卸载一个世界
    
二. PluginManager
1. 概述
PluginManager是插件管理器,很多的插件加载和数据储存都在这里进行,如监听器
的注册等
使用这个类也可以实现动态加载插件等一系列的操作,PluginManager的加载基于JavaPluginLoader

2. 常用的方法
    * addPermission(Permission) 添加Permission对象
    * callEvent(Event) 触发一个事件
    * clearPlugins() 清空插件
    * disablePlugin(Plugin) 停止一个插件，这个插件会提前调用onDisable
    并卸载
    * getPlugin​(String) 得到其他插件的插件对象
    * loadPlugin​(File) 加载一个插件，路径默认为服务端根目录
    * loadPlugins​(File) 加载一个文件夹的插件
    * registerEvents​(Listener, Plugin) 注册监听器
    * removePermission​(String) 删除一个Permission
    
[上一章](第三章*计时器的介绍.md) [下一章](第五章*各种实体类的方法介绍.md)
