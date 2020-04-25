[上一节](3-1_主要的Form种类及介绍.md) [下一节](3-2_使用外部库GUI创建UI.md)

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

- 简单的实例：

```java
class UI {
    public static final int MENU = 114514; //用于标记menu这个UI界面，方便监听事件时候判断

	public static void menu(Player player) {
    	FormWindowSimple form = new FormWindowSimple("我是标题", "我是内容");
   	 	form.addButton(new ElementButton("我是按钮1"));
    	form.addButton(new ElementButton("我是按钮2", new ElementButtonImageData("path","textures/blocks/bedrock.png")));
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
        //重点: 按钮id: 我们将上面的addButton中传入的ElementButton放入一个数组，返回的id就是被按的Button的角标
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

完成上述代码后，用户将收到一个UI界面，并且按任意按钮都会发送信息提示。在外部只需要UI.menu(player)即可召唤UI

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
    class UI implements Listener { //一般实际开发中不在这个类中写监听器
        public static final int MENU = 114514;
    	public static void menu(Player player) {
            FormWindowCustom form = new FormWindowCustom("我是标题");
            // 添加一个标签组件
            form.addElement(new ElementLabel("我是标签"));  // 组件角标: 0 (Label也占一个元素角标)
            // 添加一个下滑块组件
            form.addElement(new ElementDropdown("我是下滑块", Arrays.asList("元素1", "元素2", "元素3")));  // 组件角标: 1
            // 添加一个文本输入框组件
            form.addElement(new ElementInput("我是文本输入框"));  // 组件角标: 2
            // 添加一个水平滑块_1 (text, 最小值, 最大值, 滑动最小步数)
            form.addElement(new ElementSlider("我是水平滑块", 0, 100, 1));  // 组件角标: 3
            // 添加一个水平滑块_2 
            form.addElement(new ElementStepSlider("我是水平滑块", Arrays.asList("元素1", "元素2", "元素3")));  // 组件角标: 4
            // 添加一个开关
            form.addElement(new ElementToggle("我是开关"));  // 组件角标: 5
            player.showFormWindow(form, MENU);
        }
        
        // 另外，CustomForm会自带一个"提交"按钮，用于交付数据，我们无需添加
        
        // 监听器部分
        @EventHandle
        public void onFormResponse(PlayerFormRespondedEvent event) {
            Player player = event.getPlayer();
            int id = event.getFormID(); //这将返回一个form的唯一标识`id`
            if(id == MENU){
                FormResponseCustom response = (FormResponseCustom) event.getResponse();
                //我们通过 FormResponseCustom 中提供的:
                // - getDropdownResponse(int id) 返回Dropdown中被选择的元素的封装对象
                // - getInputResponse(int id) 返回文本输入框中的字符串
                // - getSliderResponse(int id) 返回数值水平滑块的浮点值
                // - getStepSliderResponse(int id) 返回StepSlider中被选择的元素的封装对象
                // - getToggleResponse(int id) 返回开关的布尔类型(开和关)
    		   // 注意: getDropdownResponse和getStepSliderResponse返回的都是一个FormResponseData对象，我们需要调用它的getElementContent()获得被选元素的字符串数值
                // id 和 SimpleForm中的按钮角标一样，从0开始往后标记
                String dropDown = response.getDropdownResponse(1).getElementContent();
                String input = response.getInputResponse(2);
              float slider = response.getSliderResponse(3);
                String stepSlider = response.getDropdownResponse(4).getElementContent();
                boolean toggle = response.getToggleResponse(5);
                // 获取到以上数据后就能进行一些数据传输了
            }
        }
    }
    ```
    

- 完成上述代码后，用户将会收到一个数据填写表格(CustomForm)，并且我们可以通过监听器去获取用户填入的数据

三. Modal_Form

- FormWindowModal中的构造方法

  ```java
  public FormWindowModal(String title, String content, String trueButtonText, String falseButtonText) {
          this.title = title;
          this.content = content;
          this.button1 = trueButtonText;
          this.button2 = falseButtonText;
  }
  ```

  `title`和`content`不用多说，这里解释`trueButtonText`和`falseButtonText`

  `trueButtonText`：填入一个字符串，但应填入"确定"，"同意"，"是"或者它们的近义词，这个将会是true的选项按钮

  `falseButtonText`：填入一个字符串，但应填入"拒绝"，"否"，"返回"或者它们的近义词，这个将会是false的选项按钮

  注意: ModalForm中只能填入这两个按钮，无法添加更多或者更少！并且按钮没有图标。

  PS: ModalForm的所有功能其实都是可以用SimpleForm代替，不过我们通常会使用ModalForm对于用户确定某个行为所弹出的UI。

  - 简单实例

    ```java
    class UI implements Listener {
        public static final int MENU = 1433223;
        public static void menu(Player player) {
            FormWindowModal form = new FormWindowModal("我是标题", "我是内容", "点我确定", "点我取消"); //这就实例化好了一个ModalForm
            player.showFormWindow(form, MENU); // 标记id并且发送
        }
        
        @EventHandle
        public void onFormResponse(PlayerFormRespondedEvent event) {
            Player player = event.getPlayer();
            int id = event.getFormID(); //这将返回一个form的唯一标识`id`
            if(id == MENU) {
                FormResponseModal response = (FormResponseModal) event.getResponse();
                int id = response.getClickedButtonId();
                //注意: 返回的按钮id是从1开始的，也就是说"true"代表1，"false"代表2
                if(id == 1){
                    //这里执行确定后的代码
                }else{
                    // 这里执行取消后的代码
                }
            }
        }
    }
    ```

  - 完成上述代码后，用户将收到一个是否确认界面，我们在监听器内写入确认或者取消后发生的代码

四. 小案例

- 张三想要一个按钮，如果他是OP的话，按钮text为"撤销OP"，并且点击就会撤销OP，如果他不是OP的话，按钮text为"给予OP"，点击后添加OP

  - 我们在addButton的时候判断张三是否是op，如果是，则addButton("撤销OP")，如果不是，则addButton("给予OP")，这样就可以做到同一个位置的按钮对于不同状态的张三可以显示不同内容

  - 我们在事件监听中，监听到如果张三按到了切换OP状态的这一个按钮，并且判断张三此时是不是OP，然后给予OP或者撤销OP。

    这固然可以完成上面的需求，但是如果有一种状态，只能在创建UI时候能获取，在监听器中无法获取呢？假如上面的例子，我们无法在监听器中获取张三是否为OP，怎么办呢?

    我们可以从Event中获取Form对象，根据clickButtonId来获取被按的按钮对象，进而获取它的text属性并判断是否含有"撤销"这一个字符串。就能做到无需检测张三此时的状态，而从一个按钮上的文本来进行判断。

    这里代码就不贴了，感兴趣的可以自己去写一写这个小案例。

五. 开发技巧

- 我们会将一些拥有特定状态的人才会显示的按钮放到一个Form表格的最后一个。例如"OP系统"，这个按钮往往是在最后一个位置上的。这样可以不打乱整体的按钮角标值。只需在监听器中扩充一个角标就行(普通用户根本不会按到OP系统)

[上一节](3-1_主要的Form种类及介绍.md) [下一节](3-2_使用外部库GUI创建UI.md)