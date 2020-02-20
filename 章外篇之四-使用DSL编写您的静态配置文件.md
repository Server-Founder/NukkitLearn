# NukkitLearn章外篇之四-使用Groovy DSL编写您的静态配置文件

参与编写者: MagicLu550

### 什么是DSL

DSL：以极其高效的方式描述特定领域的对象、规则和运行方式的语言。

需要有特定的解释器与其配合。

高效简洁的领域语言，与通用语言相比能极大降级理解和使用难度，同时极大提高开发效率的语言。

能够描述特定领域的世界观和方法论的语言。

DSL 通过在表达能力上做的妥协换取在某一领域内的高效。

而有限的表达能力就成为了 GPL 和 DSL 之间的一条界限。

### 使用SimpleDSL编写您的定制配置解析器

SimpleDSL是我编写的一款自定义解析框架，可以更方便的解析Groovy DSL

首先，下载[SimpleDSL](https://github.com/MagicLu550/SimpleDSL4j)

你可以将SimpleDSL的源码直接复制到项目，或者编译为jar包导入，之后您就可以使用SimpleDSL了

之后创建一个类，继承DSLParser类,

```groovy

class Example extends DSLParser {

    Example(File file) {
        super(file,"UTF-8")
    }

    @Override
    void byProperty(String name, Object args,String stack) {
        println name+" : "+args
        println(stack)
    }

    @DSLMethod
    void hello(Closure closure){
        closure()
    }
    @DSLMethod
    void items(Closure closure){
        closure()
    }

}
```
代码解释: 

@DSLMethod是指代父标签方法,在配置文件中，就可以这样使用
```groovy
items{
    hello {
      url "hello,world"
    }
}
```
只有定义了@DSLMethod，且最后一个参数为Closure类型，才能使用`{}`
而不使用`{}`的元素，可以直接使用，这种类型元素，会在byProperty中解析到
上述的配置在`Example`的解析下输出为
```log
url : ["helloworld"]
items.hello
```
因此我们可以知道，url为无`{}`体

如果hello定义为
```groovy
@DSLMethod
void hello(String name,Closure closure){
    print name
}
```
则在配置中可以
```groovy
hello "name",{
    url "hello,world"
}
```
输出结果为`name`
### 使用自定义解析器
解析config.groovy
```groovy
items{
    hello {
      url "hello,world"
    }
}
```
解析代码
```groovy
@Override
void onEnable(){
    Example example = new Example(this.getDataFolder()+"/config.groovy")
    example.getValue("items.hello.url") //获得hello,world
}

```
### 使用Groovy DSL做配置的特点
1. 可以拦截接收配置信息
如上文中:
如果hello定义为
```groovy
@DSLMethod
void hello(String name,Closure closure){
    print name
}
```
则在配置中可以
```groovy
hello "name",{
    url "hello,world"
}
```
输出结果为`name`
这样，我们可以拦截接收配置信息
2. 编写代码
因为我们的配置文件是groovy源文件，因此可以理所当然的使用groovy源代码
```groovy
class Test{
    public static final HELLO = "helloworld"
}
hello "name",{
    url Test.HELLO
}
```
同样通过byProperty拦截url元素，可以获得"helloworld"