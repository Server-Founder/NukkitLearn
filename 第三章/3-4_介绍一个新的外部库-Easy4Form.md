[上一节](3-3_使用外部库GUI创建UI.md)

# 第三章 第四节 介绍一个新的外部库-Easy4Form

参与编写者：smartcmd

#### 建议学习时间：20分钟

Easy4Form是smartcmd写的一个十分简单的外部Form库
 
使用Easy4Form，你可以十分方便的写出Form交互功能

1.EZ4F包含的类:
![images/img_1.png](img_1.png)
怎么样，是不是很简单?

2.使用
我们以simpleForm作为栗子:
```java
BFormWindowSimple bf = new BFormWindowSimple();
bf.addButton(...)
...
        
...
bf.setResponseAction((e) ->{//e是此Form的ResponseEvent对象(ez4f会保证此event是”这个“form的返回事件)
    ...//这里不需要判空，ez4f会代替你处理这些细节
});
bf.sendToPlayer(player);//注意这里
```

搭配lambda表达式，是不是很方便？
使用ez4f，你可以省去创建监听器，匹配formid等十分麻烦的步骤