[上一节](3-2_使用NukkitX自带的Form创建UI.md)

# 第三章 第三节 使用外部库GUI创建UI

参与编写者：`iGxnon`

#### 建议学习时间：20分钟

**学习要点：掌握GUI创建三种Form的方式以及对交互数据的处理**

上一节中我们学习了如何使用NukkitX自带的三种Form表格和组件创建UI，三种Form创建流程都不外乎: 

写UI视图 ——> 监听交互并处理。可是人类总是想着各种方法偷懒，这一节就引出外部库 GUI (作者: him188)

GUI: [Github地址](https://github.com/Him188/GUI) ，使创建UI的方式变得更加简洁 注: GUI的maven仓库已经失效，只能下载jar依赖。

另外介绍一下 FormAPI [GitHub地址](https://github.com/qPexLegendary/FormAPI)  使用这个依赖也可以达到更简洁创建UI，不过它可能会有一种打不开UI的bug   

如图: ![](images/3-2_使用外部库GUI创建UI/3-1.png)

这种神秘力量只能用"我代码写错了"才能解释清楚，所以建议使用GUI

注意: 学习本节之前，你得完全掌握 匿名内部类 **或者** lambda 表达式 的使用

一: GUI的简单实现原理(没有兴趣的可以直接跳过):

- GUI提供了不止三种Form表格 (但往上追溯还是那三种)
- ![](images/3-2_使用外部库GUI创建UI/3-3.png)

- 例如:`FormSimple` 提供给了一个 `onClicked`

  ```java
  /**
       * 在玩家提交表单后调用 <br>
       * Called on submitted
       *
       * @param listener 调用的方法
       */
      public final ResponsibleFormWindowSimple onClicked(@NotNull BiConsumer<Integer, Player> listener) {
          Objects.requireNonNull(listener);
          this.buttonClickedListener = listener;
          return this;
      }
      /**
       * 在玩家提交表单后调用 <br>
       * Called on submitted
       *
       * @param listener 调用的方法(无 Player)
       */
      public final ResponsibleFormWindowSimple onClicked(@NotNull Consumer<Integer> listener) {
          Objects.requireNonNull(listener);
          this.buttonClickedListener = (id, player) -> listener.accept(id);
          return this;
      }
      /**
       * 在玩家提交表单后调用 <br>
       * Called on submitted
       *
       * @param listener 调用的方法(无参数)
       */
      public final ResponsibleFormWindowSimple onClicked(@NotNull Runnable listener) {
          Objects.requireNonNull(listener);
          this.buttonClickedListener = (id, player) -> listener.run();
          return this;
      }
  ```

- `Consumer`和`BiConsumer`都是接口`Interface`，我们可以使用 lambda 表达式直接写它的实现类。也可以用 匿名内部类去编写。

- 最后 GUI 帮我们监听事件并执行我们在 `Consumer` 或 `BiConumer`的实现类中的`accept`方法。

- 综上：我们无需监听事件，在写UI视图的同时也能将用户交互后的处理写完。

二. 代码部分

- `FormSimple ` 构造方法

  ```java
      public FormSimple() {
      }
  
      public FormSimple(String content) {
          super(content);
      }
  
      public FormSimple(String title, String content) {
          super(title, content);
      }
  
      public FormSimple(String title, String content, String... buttons) {
          super(title, content, buttons);
      }
  
      public FormSimple(String title, String content, ElementButton... buttons) {
          super(title, content, buttons);
      }
  
      public FormSimple(String title, String content, @NotNull List<ElementButton> buttons) {
          super(title, content, buttons);
      }
  
  ```

  构造方法与NukkitX自带的FormWindowSimple差不多。所以不必赘述。

  `FormSimple `继承了`ResponsibleFormWindowSimple`，在 `ResponsibleFormWindowSimple` 我们常常用的方法有:

  - `addButton(ElementButton btn)` 添加一个按钮

  - `addButton(ElementButton btn, @NotNull ClickListenerSimple clickListener) `添加一个按钮，并在`ClickListenerSimple`中写入按下这个按钮的处理

    `ClickListenerSimple`: 最终继承了 Consumer 并且传入了Player这个泛型，我们可以获取player并且操作

    这个方法可以通过添加 ResponsibleButton 来代替。ResponsibleButton是一种可传入点击后处理的特殊按钮

  - `setParent(FormWindow form)` 设置父表格，用于 goBack(Player player) 这个方法返回上一个表格

  - `onClicked(@NotNull Consumer<Integer> listener)` 写入处理交互事件的匿名实现类 Integer 返回被按的按钮id- 

- Demo

  ```java
  import moe.him188.gui.window.FormSimple; //导入依赖
  
  class UI {
      public static void menu(Player player) {
          FormSimple form = new FormSimple("我是标题", "我是内容");
          form.addButton(new ElementButton("我是普通按钮"));  // 角标 0
          form.addButton(new ResponsibleButton("我是特殊按钮还带图标",
                  new ElementButtonImageData("path", "textures/blocks/bedrock.png"),
                  () -> player.sendMessage("你按了特殊按钮"))); // 这一行是lambda表达式，如果不清楚使用idea new 一下ClickListenerSimple, 会弹出提示使用匿名内部类  角标 1
          player.showFormWindow(form.onClicked(id -> { //这里也用了lambda
              if(id == 0) player.sendMessage("你按了普通按钮");
          }));
          // 上面onClicked内只需写入第一个按钮的处理方法。因为第二个按钮的处理方法在添加的时候就写了，无需重复!
      }
  }
  ```

  - 完成上述代码后，即可达到使用NukkitX自带的Form创建UI的所有流程
  - 综上: 人类的偷懒是永无止境的

- `FormCustom` 构造方法

  ```java
  	public FormCustom() {
      }
  
      public FormCustom(String title) {
          super(title);
      }
  
      public FormCustom(String title, Element... contents) {
          super(title, contents);
      }
  
      public FormCustom(String title, @NotNull List<Element> contents) {
          super(title, contents);
      }
  
      public FormCustom(String title, @NotNull List<Element> contents, @NotNull String icon) {
          super(title, contents, icon);
      }
  ```

  又是和NK自带的差不多，所以不用多说

  `FormCustom`内常用方法和`FormSimple的差不多`，不同点: 

  - `onResponded(@NotNull Consumer<FormResponseCustom> listener)` 这个类似FormSimple中的onClicked。只不过泛型变成了`FormResponseCustom` 这个上一节我们已经了解，是Custom这个表格的Response，可以获得交互数据
  - 没有了` addButton` ，我们讲过: **无法在Custom类型的Form中添加 按钮**
  - `addElement(Element element)` 这个甚至在`ResponsibleFormWindowCustom`中没有重写，也就意味着这个和NK自带的Custom类型的Form添加组件是一样的。

- Demo

  ```java
  import moe.him188.gui.window.FormCustom; //导入依赖
  
  class UI {
      public static void menu(Player player) {
          FormCustom form = new FormCustom("我是标题");
          form.addElement(new ElementInput("文本输入框")); //角标 0
          player.showFormWindow(form.onResponded(response -> { //这里也用了lambda
              player.sendMessage(response.getInputResponse(0));
          }));
      }
  }
  ```

  - 简洁，明了

- `FormModal` 构造方法

  ```java
  	public FormModal(String trueButtonText, String falseButtonText) {
          super(trueButtonText, falseButtonText);
      }
  
      public FormModal(String content, String trueButtonText, String falseButtonText) {
          super(content, trueButtonText, falseButtonText);
      }
  
      public FormModal(String title, String content, String trueButtonText, String falseButtonText){
          super(title, content, trueButtonText, falseButtonText);
      }
  ```

  和NK自带的也是差不多的

  `FormModal` 常用方法和`FormCustom`几乎一样，不同点

  - `onResponded(@NotNull Consumer<Boolean> listener)` 可以看出，这里的泛型是Boolean，于是我们可以直接放到 if 内判断
  - 没有 `addElement`和`addButton`

- Demo

  ```java
  import moe.him188.gui.window.FormModal; //导入依赖
  
  class UI {
      public static void menu(Player player) {
          // 一行写完
          player.showFormWindow((new FormModal("标题","内容","确定","取消")).onResponded(bool -> player.sendMessage(bool ? "确定" : "取消")));
      }
  }
  ```

  - 压缩再压缩！

三. 结语

- 学到此处，你应当能够熟练的操控UI，和之前学到的知识结合，写出一个漂亮的公会插件！
- GUI的表格不止这三种，但是对于我们来说，这三种已经完全够用了，感兴趣的话可以去看看GUI的github上的README，里面写了更多表格的使用方式

[上一节](3-2_使用NukkitX自带的Form创建UI.md)



另外附上GUI教程传播授权图

![](images/3-2_使用外部库GUI创建UI/3-2.png)









