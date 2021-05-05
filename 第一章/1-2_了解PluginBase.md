# 第一章 第二节 了解PluginBase
参与编写者: smartcmd MagicLu550
#### 建议学习时间: 50分钟
##### 学习要点: 了解PluginBase类中的各个方法，以及他们的作用

PluginBase，即插件主类，所有的nk插件的主类都必须继承于此类

里面包含很多方法，不过最常用的也就这3个:
```
onLoad()插件读取时调用
onEnable()插件开启时调用
onDisable()插件关闭时调用
```

一个例子:
```java
public class PluginMain extends PluginBase{
    
    @Override
    public void onLoad(){
        //插件读取
    }
    
    @Override
    public void onEnable(){
        //插件启用
    }
    
    @Override
    public void onDisable(){
        //插件关闭
    }
}
```
### 一些技巧

很多的操作，例如注册Task,监听器，都需要插件主类对象
然而我们在开发中经常遇到这种情况：如果其他类需要插件主类对象，我们需要怎么实现呢？
这里提供了一种解决方法：

```java
public class PluginMain extends PluginBase {

    private static PluginMain instance;

    @Override
    public void onLoad() {
        //插件读取
    }

    @Override
    public void onEnable() {
        instance = this;
    }

    @Override
    public void onDisable() {
        //插件关闭
    }

    public static PluginMain getInstance() {
        return instance;
    }
}
```

另外一个类：
```java
public class Listener implements cn.nukkit.Listener{
    @EventHandler
    public void onPlayerJoin(PlayerJoinEvent event){
        PluginMain.getInstance().getLogger().info(event.getPlayer().getName());//用PluginMain的logger输出控制台信息
    }
}
```

PluginBase类的常用方法:

1. isEnabled和isDisabled分别是查看插件是否执行过onEnable方法和isDisable方法,插件状态为isEnabled时，方可进行注册监听器等操作

2. setEnabled()是强行执行onEnabled,调用它会执行一次onEnabled,无参数方法默认将isEnabled改为true

3. getDataFolder() 获得插件的附属文件夹,文件路径为plugins/插件名称

4. getDescription() 得到插件plugin.yml的相关信息

5. getLogger() 得到日志对象，可以通过它进行日志输出的操作

6. isInitialized() 返回这个插件是否已经初始化

7. getCommand(String name) 获得插件的指定名称的指令对象 # 这个不太明确

8. getConfig() 获得默认的配置文件对象

9. getServer() 获得服务端对象

10.getName() 获得插件名称,在plugin.yml标记的

11.getFullName() 获得插件完整的名字,一个插件完整的名字由"名字+v+版本号"组成

12.getFile() 获得插件文件的对象

13.getPluginLoader() 获得插件加载器对象

14.init(...) 当插件开始被加载读取时，调用该方法初始化插件的基本内容

15.onCommand(...) 执行plugin.yml中定义的命令

16.getResource(...) 见[1-5](1-5_如何使用配置文件.md)

17.saveConfig() saveDefaultConfig()保存默认配置文件

18.reloadConfig()重新加载配置文件，使得里面信息程序可以读取到

[上一节](1-1_what's plugin.md) [下一节](1-3_如何编写监听器.md)
