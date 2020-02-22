[上一节](1-6_如何编写plugin.yml.md) [下一节](1-8_案例玩家进入信息等效果.md)

# 第一章 第七节 PluginBase类
参与编写者: MagicLu550
#### 建议学习时间: 60分钟 
##### 学习要点: 了解PluginBase的方法

这里将会详细的介绍PluginBase类的常用方法。

PluginBase实现了Plugin接口，是插件运行的委托父类。它的子类被认定为服务端的运行类，它有
三个运行方法，在[第一章](1-1_如何搭建环境.md)已经提到,它的很多方法都是开发过程最常用的，
也是一切api调用的入口和途径

PluginBase.java
```java
package cn.nukkit.plugin;

import cn.nukkit.Server;
import cn.nukkit.command.Command;
import cn.nukkit.command.CommandSender;
import cn.nukkit.command.PluginIdentifiableCommand;
import cn.nukkit.utils.Config;
import cn.nukkit.utils.Utils;
import com.google.common.base.Preconditions;
import org.yaml.snakeyaml.DumperOptions;
import org.yaml.snakeyaml.Yaml;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.util.LinkedHashMap;

/**
 * 一般的Nukkit插件需要继承的类。<br>
 * A class to be extended by a normal Nukkit plugin.
 *
 * @author MagicDroidX(code) @ Nukkit Project
 * @author 粉鞋大妈(javadoc) @ Nukkit Project
 * @see cn.nukkit.plugin.PluginDescription
 * @since Nukkit 1.0 | Nukkit API 1.0.0
 */
abstract public class PluginBase implements Plugin {

    private PluginLoader loader;

    private Server server;

    private boolean isEnabled = false;

    private boolean initialized = false;

    private PluginDescription description;

    private File dataFolder;
    private Config config;
    private File configFile;
    private File file;
    private PluginLogger logger;


    public void onLoad() {

    }

    public void onEnable() {

    }

    public void onDisable() {

    }

    public final boolean isEnabled() {
        return isEnabled;
    }

    /**
     * 加载这个插件。<br>
     * Enables this plugin.
     * <p>
     * <p>如果你需要卸载这个插件，建议使用{@link #setEnabled(boolean)}<br>
     * If you need to disable this plugin, it's recommended to use {@link #setEnabled(boolean)}</p>
     *
     * @since Nukkit 1.0 | Nukkit API 1.0.0
     */
    public final void setEnabled() {
        this.setEnabled(true);
    }

    /**
     * 加载或卸载这个插件。<br>
     * Enables or disables this plugin.
     * <p>
     * <p>插件管理器插件常常使用这个方法。<br>
     * It's normally used by a plugin manager plugin to manage plugins.</p>
     *
     * @param value {@code true}为加载，{@code false}为卸载。<br>{@code true} for enable, {@code false} for disable.
     * @since Nukkit 1.0 | Nukkit API 1.0.0
     */
    public final void setEnabled(boolean value) {
        if (isEnabled != value) {
            isEnabled = value;
            if (isEnabled) {
                onEnable();
            } else {
                onDisable();
            }
        }
    }

    public final boolean isDisabled() {
        return !isEnabled;
    }

    public final File getDataFolder() {
        return dataFolder;
    }

    public final PluginDescription getDescription() {
        return description;
    }

    /**
     * 初始化这个插件。<br>
     * Initialize the plugin.
     * <p>
     * <p>这个方法会在加载(load)之前被插件加载器调用，初始化关于插件的一些事项，不能被重写。<br>
     * Called by plugin loader before load, and initialize the plugin. Can't be overridden.</p>
     *
     * @param loader      加载这个插件的插件加载器的{@code PluginLoader}对象。<br>
     *                    The plugin loader ,which loads this plugin, as a {@code PluginLoader} object.
     * @param server      运行这个插件的服务器的{@code Server}对象。<br>
     *                    The server running this plugin, as a {@code Server} object.
     * @param description 描述这个插件的{@code PluginDescription}对象。<br>
     *                    A {@code PluginDescription} object that describes this plugin.
     * @param dataFolder  这个插件的数据的文件夹。<br>
     *                    The data folder of this plugin.
     * @param file        这个插件的文件{@code File}对象。对于jar格式的插件，就是jar文件本身。<br>
     *                    The {@code File} object of this plugin itself. For jar-packed plugins, it is the jar file itself.
     * @since Nukkit 1.0 | Nukkit API 1.0.0
     */
    public final void init(PluginLoader loader, Server server, PluginDescription description, File dataFolder, File file) {
        if (!initialized) {
            initialized = true;
            this.loader = loader;
            this.server = server;
            this.description = description;
            this.dataFolder = dataFolder;
            this.file = file;
            this.configFile = new File(this.dataFolder, "config.yml");
            this.logger = new PluginLogger(this);
        }
    }

    public PluginLogger getLogger() {
        return logger;
    }

    /**
     * 返回这个插件是否已经初始化。<br>
     * Returns if this plugin is initialized.
     *
     * @return 这个插件是否已初始化。<br>if this plugin is initialized.
     * @since Nukkit 1.0 | Nukkit API 1.0.0
     */
    public final boolean isInitialized() {
        return initialized;
    }

    /**
     * TODO: FINISH JAVADOC
     */
    public PluginIdentifiableCommand getCommand(String name) {
        PluginIdentifiableCommand command = this.getServer().getPluginCommand(name);
        if (command == null || !command.getPlugin().equals(this)) {
            command = this.getServer().getPluginCommand(this.description.getName().toLowerCase() + ":" + name);
        }

        if (command != null && command.getPlugin().equals(this)) {
            return command;
        } else {
            return null;
        }
    }

    @Override
    public boolean onCommand(CommandSender sender, Command command, String label, String[] args) {
        return false;
    }

    @Override
    public InputStream getResource(String filename) {
        return this.getClass().getClassLoader().getResourceAsStream(filename);
    }

    @Override
    public boolean saveResource(String filename) {
        return saveResource(filename, false);
    }

    @Override
    public boolean saveResource(String filename, boolean replace) {
        return saveResource(filename, filename, replace);
    }

    @Override
    public boolean saveResource(String filename, String outputName, boolean replace) {
        Preconditions.checkArgument(filename != null && outputName != null, "Filename can not be null!");
        Preconditions.checkArgument(filename.trim().length() != 0 && outputName.trim().length() != 0, "Filename can not be empty!");

        File out = new File(dataFolder, outputName);
        if (!out.exists() || replace) {
            try (InputStream resource = getResource(filename)) {
                if (resource != null) {
                    File outFolder = out.getParentFile();
                    if (!outFolder.exists()) {
                        outFolder.mkdirs();
                    }
                    Utils.writeFile(out, resource);

                    return true;
                }
            } catch (IOException e) {
                Server.getInstance().getLogger().logException(e);
            }
        }
        return false;
    }

    @Override
    public Config getConfig() {
        if (this.config == null) {
            this.reloadConfig();
        }
        return this.config;
    }

    @Override
    public void saveConfig() {
        if (!this.getConfig().save()) {
            this.getLogger().critical("Could not save config to " + this.configFile.toString());
        }
    }

    @Override
    public void saveDefaultConfig() {
        if (!this.configFile.exists()) {
            this.saveResource("config.yml", false);
        }
    }

    @Override
    public void reloadConfig() {
        this.config = new Config(this.configFile);
        InputStream configStream = this.getResource("config.yml");
        if (configStream != null) {
            DumperOptions dumperOptions = new DumperOptions();
            dumperOptions.setDefaultFlowStyle(DumperOptions.FlowStyle.BLOCK);
            Yaml yaml = new Yaml(dumperOptions);
            try {
                this.config.setDefault(yaml.loadAs(Utils.readFile(this.configFile), LinkedHashMap.class));
            } catch (IOException e) {
                Server.getInstance().getLogger().logException(e);
            }
        }
    }

    @Override
    public Server getServer() {
        return server;
    }

    @Override
    public String getName() {
        return this.description.getName();
    }

    /**
     * 返回这个插件完整的名字。<br>
     * Returns the full name of this plugin.
     * <p>
     * <p>一个插件完整的名字由{@code 名字+" v"+版本号}组成。比如：<br>
     * A full name of a plugin is composed by {@code name+" v"+version}.for example:</p>
     * <p>{@code HelloWorld v1.0.0}</p>
     *
     * @return 这个插件完整的名字。<br>The full name of this plugin.
     * @see cn.nukkit.plugin.PluginDescription#getFullName
     * @since Nukkit 1.0 | Nukkit API 1.0.0
     */
    public final String getFullName() {
        return this.description.getFullName();
    }

    /**
     * 返回这个插件的文件{@code File}对象。对于jar格式的插件，就是jar文件本身。<br>
     * Returns the {@code File} object of this plugin itself. For jar-packed plugins, it is the jar file itself.
     *
     * @return 这个插件的文件 {@code File}对象。<br>The {@code File} object of this plugin itself.
     * @since Nukkit 1.0 | Nukkit API 1.0.0
     */
    protected File getFile() {
        return file;
    }

    @Override
    public PluginLoader getPluginLoader() {
        return this.loader;
    }
}

```
当然我们从这里面可以抓住几个要点:

- 运行方法: onLoad(),onEnable(),onDisable()
- 获得方法: boolean isEnabled()
            ,boolean isDisabled(),
            File getDataFolder(),
            PluginDescription getDescription(),
            PluginLogger getLogger(),
            boolean isInitialized(),
            PluginIdentifiableCommand getCommand(String name),
            Config getConfig(),
            Server getServer(),
            String getName(),
            String getFullName(),
            File getFile(),
            PluginLoader getPluginLoader()
- 设置的方法: setEnabled(boolean enabled)
- 初始化的方法: init(PluginLoader loader, Server server, PluginDescription description, File dataFolder, File file)
- 命令方法: boolean onCommand(CommandSender sender, Command command, String label, String[] args)
- 产生配置文件方法: InputStream getResource(String filename),
               boolean saveResource(String filename),
               boolean saveResource(String filename, boolean replace)，
               boolean saveResource(String filename, String outputName, boolean replace),
               void saveConfig(),
               void saveDefaultConfig(),
               void reloadConfig()

这里介绍一下常用的方法

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

16.getResource(...) 见[第五章](1-5_如何使用配置文件.md)

17.saveConfig() saveDefaultConfig()保存默认配置文件

18.reloadConfig()重新加载配置文件，使得里面信息程序可以读取到

[上一节](1-6_如何编写plugin.yml.md) [下一节](1-8_案例玩家进入信息等效果.md)