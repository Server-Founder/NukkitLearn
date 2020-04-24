[上一节]() [下一节]()

# 第三章 第二节 使用NukkitX自带的Form创建UI

参与编写者：`iGxnon`

#### 建议学习时间：40分钟

**学习要点：掌握三种Form的创建方式以及对交互数据处理方式**

上一节我们已经介绍了各种Form和组件，并且梳理了一个UI从创建到处理用户交互数据的一个流程，这一节正式介绍代码

一. Simple_Form

- FormWindowSimple中的构造方法

```java
public FormWindowSimple(String title, String content) {
       this(title, content, new ArrayList<>());
}
public FormWindowSimple(String title, String content, List<ElementButton> buttons) {
       this.title = title;
       this.content = content;
       this.buttons = buttons;
}
```

我们可以看到有两种构造方法都要传入`title`和`content`，这两个值对应着Simple_Form的**标题**和**内容**。

第二种构造方法多了一个由`ElementButton`组成的一个List，表示了这个Simple_Form中的按钮列表

不过我们常常使用第一种构造方法，然后调用`addButton(ElementButton btn)`来添加按钮。

-  ElementButton的构造方法

```java
public ElementButton(String text) {
    this.text = text;
}
public ElementButton(String text, ElementButtonImageData image) {
    this.text = text;
    if (!image.getData().isEmpty() && !image.getType().isEmpty()) this.image = image;
}
```

第一个构造方法不用多说，传入一个按钮上显示的文本`text`。我们发现第二个构造方法可以传入一个`ElementButtonImageData`。ElementButtonImageData，简单介绍一下，这个是按钮上图片的解释类。我

们来看一看它的构造方法`ElementButtonImageData(String type, String data)`

- String `type`

  只能填入"url"或者"path"

  "url"：在`data`内填入一个url图片地址，即可解析到按钮上

  "path"：在`data`内填入一个本地资源包内一个图片的路径，即可解析到按钮上

- String `data`

  填入一个url或者是一个路径，取决于`type`所填入的值

  例如"textures/blocks/bedrock.png" (type是"path"下) 即可在按钮左边显示一个基岩的图片



- 一个简单的实例：

```java
class UI {
    public static final int MENU = 114514; //用于标记menu这个UI界面，方便监听事件时候判断

	public static void menu(Player player) {
    	FormWindowSimple form = new FormWindowSimple("我是标题", "我是内容");
   	 	form.addButton(new ElementButton("我是按钮1"));
    	form.addButton(new ElementButton("我是按钮2", new ElementButtonImageData("path", 		"textures/blocks/bedrock.png")));
    	form.addButton(new ElementButton("我是按钮3"));
    	player.showFormWindow(form, MENU); //将form发送给玩家,将MENU标记给了这个form
    
        // showFormWindow部分代码
        /**
         * Shows a new FormWindow to the player
         * 将一个FormWindow发送给玩家
         * You can find out FormWindow result by listening to PlayerFormRespondedEvent
         * 你可以从监听PlayerFormRespondedEvent中得到玩家的交互数据
         * @param window to show
         * @return form id to use in {@link PlayerFormRespondedEvent}
         */

        //public int showFormWindow(FormWindow window) {
          //  return showFormWindow(window, this.formWindowCount++);
        //}

        /**
         * Shows a new FormWindow to the player
         * You can find out FormWindow result by listening to PlayerFormRespondedEvent
         *
         * @param window to show
         * @param id form id
         * @return form id to use in {@link PlayerFormRespondedEvent}
         */
        //public int showFormWindow(FormWindow window, int id) {
          //  ModalFormRequestPacket packet = new ModalFormRequestPacket();
          //  packet.formId = id;
          //  packet.data = window.getJSONData();
          //  this.formWindows.put(packet.formId, window);

          //  this.dataPacket(packet);
          //  return id;
        //}
    }
}	
```

以上的方法可以将一个id标记为`114514`的`menu`UI界面发送给玩家。

但是玩家对Form的交互却没有任何响应(即按了三个按钮也没响应)，所以我们需要监听玩家的**交互事件**并取得交互数据

交互事件：**PlayerFormRespondedEvent**

- 监听器的代码：

```java
@EventHandle
public void onFormResponse(PlayerFormRespondedEvent event) {
    Player player = event.getPlayer();
    int id = event.getFormID(); //这将返回一个form的唯一标识`id`
    if(id == UI.MENU) { //判断出这个UI界面是否是我们上面写的`menu`
        FormResponseSimple response = (FormResponseSimple) event.getResponse(); //这里需要强制类型转换一下
        //获取到被按的按钮的id(如果按"x"则返回-1)
        //按钮id: 我们将上面的addButton中传入的ElementButton放入一个数组，返回的id就是被按的Button的角标
        //例如：玩家按了"我是按钮1"这个按钮，则会返回0
        int clickedButtonId = response.getClickedButtonId();
        //分类别讨论
        switch (clickedButtonId){
            case 0:
                player.sendMessage("你摁了按钮1");
                break;
            case 1:
                player.sendMessage("你摁了按钮2");
                break;
            case 2:
                player.sendMessage("你摁了按钮3");
                break;
            default:
                break;
        }
    }
}
```

完成上述代码后，用户将收到一个UI界面，并且按任意按钮都会发送信息提示

二. Custom_Form

- FormWindowCustom中的构造方法

  ```java
  public FormWindowCustom(String title) {
      this(title, new ArrayList<>());
  }
  public FormWindowCustom(String title, List<Element> contents) {
      this(title, contents, (ElementButtonImageData) null);
  }
  public FormWindowCustom(String title, List<Element> contents, String icon) {
      this(title, contents, icon.isEmpty() ? null : new ElementButtonImageData(ElementButtonImageData.IMAGE_DATA_TYPE_URL, icon));   
  }
  public FormWindowCustom(String title, List<Element> contents, ElementButtonImageData icon) {
      this.title = title;
      this.content = contents; 
      this.icon = icon;
  }
  ```

  我们可以看到这有很多种构造方法，不过我们常用的只有**第一种**。另外，FormCustom没有content这一个属性，不过可以通过添加Label组件弥补

  在实例化一个CustomForm之后，我们需要调用`addElement(Element element)`这个方法去添加组件

  注意：无法在CustomForm中添加**ElementButton**这个组件！

  - 简单实例

    ```java
    class UI {
    	public static void menu(Player player) {
            FormWindowCustom form = new FormWindowCustom("我是标题");
            // 添加一个标签组件
            form.addElement(new ElementLabel("我是标签"));
            // 添加一个下滑块组件
            form.addElement(new ElementDropdown("我是下滑块", Arrays.asList("元素1", "元素2", "元素3")));
            // 添加一个文本输入框组件
            form.addElement(new ElementInput("我是文本输入框"));
            // 添加一个水平滑块_1 (text, 最小值, 最大值, 滑动最小步数)
            form.addElement(new ElementSlider("我是水平滑块", 0, 100, 1));
            
        }
    }
    ```

    

