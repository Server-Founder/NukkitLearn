[上一节]() [下一节]()

# 第三章 第一节 主要的Form种类及介绍

参与编写者：`iGxnon`

#### 建议学习时间：10分钟

**学习要点：了解三个主要的Form及其各组件**

UI是用户与程序交互的图形界面，Form则是Nukkit在UI方面上的一个呈现方式——**表格**。而这些可以被叫做三层架构中的**表示层**，用于接收用户传来的数据，并传递给**业务逻辑层**，最终可能会被写入**持久层(配置文件)**。

Nukkit提供了三种Form以及一些组件。正由于这些简单的表格配合组件，使得Nukkit的UI图形交互界面变得十分好写！

这一节我们不解析代码，只给学者提供一个思路，具体创建代码看下一节

所有组件和Form位置均在`cn.nukkit.form`包下

一. 三种Form

1. **Simple_Form** 
   - 特点：这种表格只能放入**按钮**这一种组件
   - 作用：主要用于做各种表格之间的跳转，点击执行命令等操作
   - 位置 `cn.nukkit.form.window.FormWindowSimple`
2. **Custom_Form**
   - 特点: 这种表格可以放入**除了按钮**以外的组件，不能设置content（可通过label这一组件弥补）
   - 作用: 接收用户传入的各种数据类型（字符串，布尔，数），用于传输数据
   - 位置 `cn.nukkit.form.window.FormWindowCustom`
3. **Modal_Form**
   - 特点: 这种表格**只能放入两个按钮(不能带有图片)**
   - 作用: 专门用于接收用户传入的布尔类型(true or false)，常见于一些确认界面中
   - 位置 `cn.nukkit.form.window.FormWindowModal`

以上的三种Form各自对应着自己的一个FormResponse，例如FormWindowSimple -> FormResponseSimple

FormResponse：接收玩家交互后传入的数据（例如玩家摁了哪个按钮），用于做数据传输或者处理

二. 组件

1. **Button**（按钮）
   - 特点：只能被放入Simple类型的Form中，可以传入图片
   - 位置 `cn.nukkit.form.element.ElementButton`
2. **Dropdown**（下滑块）
   - 特点：点击弹出一个下滑块列表，可供用户从有限的**备选类型**中选取一个
   - 位置 `cn.nukkit.form.element.ElementDropdown`
3. **Input** （文本输入框）
   - 特点：用户可以在框中键入一些字符串
   - 位置 `cn.nukkit.form.element.ElementInput`
4. **Label** （标签）
   - 特点：作用类似于content，在表格内添加一个文本标签
   - 位置 `cn.nukkit.form.element.ElementLabel`
5. **Slider** （水平滑块_1）
   - 特点：设置最小值和最大值后，可以滑动滑块获得一个数值（类似进度条）
   - 位置 `cn.nukkit.form.element.ElementSlider`
6. **StepSlider**（水平滑块_2）
   - 特点：类似于Dropdown，只不过将下滑块改成水平滑块
   - 位置 `cn.nukkit.form.element.ElementStepSlider`
7. **Toggle**（开关）
   - 特点：可以接受布尔值
   - 位置 `cn.nukkit.form.element.ElementToggle`

三. 创建流程

**上面我们已经学习到了三种Form和组件，现在就来讲一讲它们的创建流程:**

发送一个Form

- 实例化一种Form ——> 设置标题(Title)和内容(content，Custom类型除外) ——> 添加能加入的组件 ——> 将Form发送给玩家

接受玩家对Form的交互信息

- 监听`PlayerFormRespondedEvent` ——> 获取到FormResponse ——> 分析FormResponse中返回的交互数据 ——> 将数据进一步传输或者直接判断处理









