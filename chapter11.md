# 第11章 广播中心

FrontlineSMS 是一款工具软件，用于联络那些无法访问互联网但可以用手机通信的人，通常用于互联网尚未普及地区的选举监督、天气预报广播等。软件作者Ken Banks借助于移动通信技术为人们提供帮助，他的贡献大概无人能及。

FrontlineSMS运行在连接了手机的电脑上。电脑和手机共同构成一个短信中转站，为群内人员提供文本通信服务。无法上网的人可以发送一个特殊代码来加入群，随后他们会收到来自中转站的各种广播消息。这个中转站我们称之为“广播中心”，对于那些没有网络的地方，广播中心成为与外界联系的重要手段。

使用App Inventor可以创建自己的短信处理应用。有趣的是，应用需要运行在一部android设备上，但应用的用户却不必使用Android手机，他们可以用任何手机，智能的或非智能的，与应用之间进行短信的交流。应用虽然具有图形化的用户界面（GUI），但GUI仅供应用的管理者使用，用来监控应用中的各种活动。

![](./images/0-12.png)

本章将创建一个与FrontlineSMS功能类似的广播中心，不过是运行在Android手机上。一台具有中转枢纽作用的移动设备，意味着管理者可以在移动中保持交流，这一点在某些场合下尤其重要，如选举监督和医疗争议谈判。

假想有一个“快闪舞蹈团”（FlashMob Dance Team，缩写为FMDT），他们可以召之即来，随时随地表演舞蹈，然后瞬间解散，消失得无影无踪，他们用你创建的广播中心来组织表演活动。人们只要向中心发送短信“joinFMDT”（参加快闪舞蹈团），即可完成入团注册，每个注册成功的人都可以向舞蹈团中的其他人广播消息。

广播中心用下面的方式处理收到的短信：

1. 如果发信人不在广播中心的成员名单中，则回复短信邀请他加入，并告知他申请代码；
2. 如果收到“joinFMDT”，则接收发信人为广播中心成员；【如果组员发送“joinFMDT”呢？】
3. 如果发信人已经是广播中心的成员，则转发该消息给全体广播中心成员。

我们来分步实现这些功能模块。首先，用自动回复来邀请人们加入广播中心。整个应用完成之后，对于创建这类“以短信为用户界面的应用”，你将有透彻的了解。

# 学习要点
本章包括下列App Inventor概念，其中有些你可能已经熟悉了：

* Texting组件：发送短信及处理收到的短信；
* 列表变量：在本例中用来记录电话号码清单；
* foreach块：对列表中的数据进行逐项重复操作。在本例子中，使用foreach块向电话号码列表中的所有手机广播消息；
* TinyDB组件：实现数据的永久存储，以保证当应用关闭并再次打开时，电话号码列表不丢失。

# 准备开始

你需要一部可以接收和发送短信的手机来测试程序，因为App Inventor自带的模拟器没有这个功能。您还需要招呼一些朋友给你发送短信，来充分地测试应用。

连接到App Inventor网站，创建新项目“BroadcastHub”，设置Screen1.Title属性为“广播中心”，并连接测试手机。

# 设计组件

广播中心有利于手机之间的通信：这些手机不需要安装应用，甚至不必是智能手机。因此在本例中不必为用户提供操作界面，只需为群管理员提供操作界面。

管理员的操作界面包括两个简单的部分，一是显示当前的“广播列表”，即已注册成员的电话号码清单，二是记录所有收到并被广播出去的短信。

为了创建这个界面，要添加表11-1中列出的组件。

表11-1 广播中心操作界面中的组件

|组件类型|	面板中分组|	命名|	作用|
|:-----|:-------|:-------|:------|
|Label	|User Interface|	Label1	|电话号码清单的标题|
|Label	|User Interface|	BroadcaseListLabel	|显示所有已注册的电话号码|
|Label	|User Interface	|Label2	|日志信息的标题|
|Label	|User Interface	|LogLabel	|显示收到及广播短信的记录|
|Texting	|Social	|Texting1|	处理短信|
|TinyDB	|Storage	|TinyDB1	|保存已注册的手机号码清单|

添加组件之后，还要设置以下属性：

1. 设置每个Label的Width属性为“Fill parent”，让组件在水平方向上充满手机；
2. 设置标题Label的FontSize属性（Label1和Label2）为18，并勾选FontBold框；
3. BroadcastListLabel和LogLabel的Height设置为200像素，用于显示多行；
4. 设置BroadcastListLabel的Text属性为“广播列表…”；
5. LogLabel的Text属性设置为空。

图11-1显示了应用在组件设计器中的布局。
![](./images/1-14.png)

图11-1 广播中心组件设计


# 为组件添加行为

在这个应用中，促使程序运行的事件是其他手机发来的短信，而不是用户在界面上的输入或点击，因此应用的任务是处理这些短信，并将发信人手机号码保存到列表中，具体操作如下：

* 如果短信发送者不在广播列表中，则回复一个邀请参加的短信；
* 如果收到短信“joinFMDT”，则将发送者注册为广播列表的一员；
* 如果短信发送者已经在广播列表中，则将该短信广播到列表中的所有手机。
现在开始创建第一个行为：收到短信时，回复发送者，邀请他注册，方法是向你发送短信“joinFMDT”。表11-2中列出了需要的块。

表11-2 邀请人们通过发短信来加入群组，需要下面的块

|块的类型	|所在抽屉	|作用|
|:----|:----|:-----|
|Texting.MessageReceived|	Texting1	|当手机收到短信时，触发该事件
|set Texting1.PhoneNumber to	|Texting1	|设置短信接收者的电话号码|
|参数number	|Variables	|MessageReceived事件的参数：发送者手机号|
|Set Texting1.Message	|Texting1	|设置要发送的邀请短信|
|“想加入快闪舞蹈团，请发送‘joinFMDT’到此号码。”	|Text	|邀请短信的内容|
|Texting1.SendMessage|	Texting1	|发送短信|

### 块的作用

根据在第4章“开车不发短信”中的经验，你应该很熟悉这些块。当手机收到短信时会触发Texting1.MessageReceived事件。如图11-2，在事件处理程序中设置Texting1组件的PhoneNumber及Message属性，然后发送短信。
![](./images/2-12.png)

图11-2 收到短信后回复邀请短信

![](./images/test-3.png)

测试：需要用第二部手机来测试这一功能；你不能给自己发短信，否则会永远循环下去！如果没有其他手机，可以注册Google Voice或类似的服务，从这些服务中给自己的手机发短信。用第二部手机发送“你好”到测试手机，则第二部手机会收到一个邀请加入“舞蹈团”的短信。

## 将某人加入广播列表

现在创建第二个行为：收到短信“joinFMDT”后，将发信人添加到广播列表中。首先定义列表变量BroadcastList来保存注册的电话号码。从Variables中拖出一个“initialize global name to”块，将name改为“BroadcastList”，并用make a list块初始化列表，此时列表为空。如图11-3（稍后将实现向列表中添加数据项的功能）。
![](./images/3-12.png)

图11-3 变量BroadcastList用于存储注册的电话号码【也可用create empty list块】
下面修改Texting1.MessageReceived事件处理程序，如果收到短信“joinFMDT”，则将发信人手机号码添加到BroadcastList中。判断短信内容需要使用Ifelse块（在第十章“出题”应用中使用过），将新号码添加到列表中需要使用add item to list块。整个设置所需的块见表11-3。在电话号码添加完之后，用BroadcastListLabel来显示新列表。

表11-3 检查来信内容，并将发信人添加到广播列表中，需要如下块

|块的类型	|所在抽屉|	作用|
|:----|:----|:----|
|initialize global BroadcastList to|	Variables	|定义广播列表变量|
|ifelse	|Control	|根据收到短信的内容决定做什么事|
|=	|Math	|判断短信内容是否等于“joinFMDT”|
|get messageText	|Variables	|将来信内容插入“=”块（左边）|
|“joinFMDT”	|Text	|将固定文本插入“=”块（右边）|
|add items to list	|Lists	|向广播列表中添加发信人电话号码|
|get number	|Variables	|将发信人手机号码插入“add items to list”|
|set BroadcaseListLabel.Text to|	BroadcaseListLabel	|显示新列表|
|get global BroadcastList	|Variables	|将其插入set BroadcaseListLabel.Text to块|
|set Texting1.Message to	|Texting1	|设置短信内容，准备用Texting1回复发信人|
|“恭喜你成功加入…”|	Text|	祝贺发信人加入群组成功。|

### 块的作用

如图11-4所示，对刚收到的短信进行回复，第一行的块将发信人手机号码设置为接收人手机号码，即设置Texting1.PhoneNumber为number。然后判断messageText是否为特殊代码“joinFMDT”：如果是，则将发送者手机号添加到BroadcastList并发短信祝贺；如果不是，则回复邀请短信。在Ifelse块之后，回复短信被发出（最后一行）。
![](./images/4-13.png)

图11-4 如果收到短信“joinFMDT”，则将发信人手机号添加到BroadcastList

![](./images/test-3.png)

测试：用第二部手机发送短信“joinFMDT”到测试手机，在测试手机收到短信的同时，第二部手机的号码出现在“已注册的电话号码”下面，第二部手机会收到祝贺短信。尝试发一个其他内容的短信，检查邀请短信是否能正常发送。

## 广播消息

下面来添加广播行为：当广播列表BroadcastList中的成员向广播中心发来短信时，将此信息转发给列表中的所有手机。这一功能稍显复杂，需要更多的控制块：增加一个Ifelse块和一个foreach块。新增的Ifelse块用于检查发送短信的手机号是否在广播列表中，而foreach块用于向列表中的所有手机广播这条短信。另外还要将之前的Ifelse块移动到新Ifelse块的“else”部分。表11-4列出了需要新增的块。

表11-4 向列表中的成员广播某个成员发来的短信需要新增的块

|块的类型|	所在抽屉|	作用|
|:---|:---|:---|
|ifelse|	Control	|根据发信人是否已在广播列表中来决定做不同的事|
|is in list？	|Lists	|检查某数据是否在列表中|
|get global BroadcastList	|Variables	|将其插入is in list？的list插槽中|
|get number	|Variables	|将其插入is in list？的thing插槽中|
|set Texting1.Message to	|Texting1	|设置将被广播出去的短信内容（列表成员的来信）|
|get messageText	|Variables	|即将被广播出去的列表成员来信|
|foreach	|Control	|向列表中的所有成员发送同一条短信|
|get global BroadcastList	|Variables	|将其插入foreach的list插槽|
|set Texting1.PhoneNumber to	|Texting1	|设置接收短信的手机号码|
|get item|	Variables	|BroadcaseList中当前正在操作的项/变量：保存的是手机号|


### 块的作用

这里使用了嵌套的ifelse块，使得程序更加复杂，如图11-5所示。嵌套的ifelse块指的是在一个ifelse块的“then”或“else”插槽中嵌入了另一个ifelse块。在本例中，外层的ifelse负责检查发信人的手机号是否已在广播列表中。如果在，则将该短信转发给列表中的所有人；如果不在，则执行内层ifelse判断：短信内容messageText是否为“joinFMDT”，并依据判断结果，执行不同的分支操作。

从理论上，if块和ifelse块可以做任意层级的嵌套，来实现更加复杂的行为（更多关于条件语句块的内容请参见第18章）。

在外层ifelse块的then分支中，使用foreach块来广播短信。foreach遍历BroadcastList列表中的每一项，并把短信发送给列表中的每个电话号码。在foreach执行循环时，BroadcastList中的每个电话号码依次被保存在item中（item是一个变量，代表了foreach当前正在处理的项）。在foreach块内，设置Texting.PhoneNumber的值为当前项item，并向其发送短信。有关foreach的更多信息，请参见第20章。
![](./images/5-13.png)

图11-5 检查发信人是否已在广播列表中，如果是，则广播此短信

![](./images/test-3.png)

测试：首先要有两部不同的手机通过发送“joinFMDT”到测试手机，实现成功注册。然后，从一部手机向广播中心发一条短信，这时两部手机都应该收到这条短信（包括发送短信的那一个）。

## 整理列表的显示

广播短信的功能已经实现，但管理员的界面尚需改进。首先，电话号码列表的显得很乱：用Label显示列表时，列表项之间用空格分隔，并且尽可能占满一行，像下面这样：

* (+861303318989 +861581235590 +8618902018909 +8613301103355 +8613801237890)
为了改善这种局面，使用表11-5列出的块创建一个过程displayBroadcastList，来实现每行只显示一个号码。请务必在add items to list块的下面调用该过程，以便显示更新后的列表。

表11-5 改进电话号码列表显示所需的块

|块的类型|所在抽屉|作用|
|:---|:---|:----|
|to procedure(“displayBroadcastList”)|	Procedures|	创建过程displayBroadcastList|
|set BroadcaseListLabel.Text to|	BroadcaseListLabel	|用来显示列表|
|“”	|Text	|空文本|
|foreach	|Control	|对电话号码列表进行遍历|
|pnumber	|foreach内置	|变量pnumber为遍历过程中正在访问的项|
|get global BroadcaseList	|Variables	|插入foreach块的in list插槽|
|set BroadcaseListLabel.Text to|	BroadcaseListLabel	|显示电话号码列表|
|join	|Text	|将多个文本片段连接为一个文本对象|
|BroadcaseListLabel.Text	|BroadcaseListLabel	|每次循环都以既有label内容为基础追加新项|
|“n”	|Text	|换行，以便下一个号码显示在下一行|
|get pnumber	|foreach内置	|遍历时列表中正在访问的项(手机号码)|

### 块的作用

过程displayBroadcastList中的foreach块逐行地将每个手机号码添加到label的末尾，如图11-6所示，用换行符（ n）来分隔每个号码，使得每个号码各占一行。

![](./images/6-13.png)
图11-6 逐行显示手机号码
不过displayBroadcastList过程不会主动做任何事，除非调用它。在Texting1.MessageReceived事件处理程序中，紧接着add item to list块调用它。过程的调用取代了列表BroadcastList在 BroadcastListLabel.Text中的默认显示。块call displayBroadcastList归属在Procedures抽屉中。

图11-7显示了Texting1.MessageReceived事件处理程序中相关的块。

![](./images/7-12.png)
图11-7 调用displayBroadcastList过程
关于用foreach来显示列表的详细信息请参见第20章，关于创建和调用过程的详细信息请参见第21章。

![](./images/test-3.png)


测试：重新启动应用来清除列表，然后用至少两个不同的手机进行注册（再次）。手机号码是否逐行显示了？

## 录广播过的短信

在收到短信并向其他手机发出广播之后，程序应该记录此类事件，以便管理员可以对活动进行监督。在组件设计器中，已经添加的LogLabel组件就是用于这一目的。下面编写程序，每当收到新的短信时，改变LogLabel的显示。

要创建像这样的一段文本：“来自+8613901231234的短信已经广播。”字符“+8613901231234”不是固定数据，而是MessageReceived事件自带的参数值。因此，要创建的文本包括三个部分：①“来自”；②手机号码，为参数number；③“的短信已经广播”。正如在前几章中所做的一样，用join将三个部分连接起来，表11-6列出了需要的块。

表11-6 构建广播日志所需要的块

|块的类型|	所在抽屉	|作用|
|:---|:----|:----|
|set LogLabel.Text to	|LogLabel	|在此显示日志|
|join	|Text|	由多个文本片段创建成一个文本对象|
|“来自”	|Text	|每条日志信息的第①部分|
|get number	|Texting1.MessageReceived事件内置参数	|日志信息的第②部分：短信发送者的手机号码|
|“的短信已经广播。n”	|Text|	日志信息的第③部分|
|LogLabel.Text	|LogLabel	|在原有日志前插入一条新的日志|

### 块的作用

在收到短信后，向BroadcastList列表中的所有号码广播此短信，再修改LogLabel，记录刚才的广播操作，如图11-8所示。需要注意的是，我们将消息添加到列表的开始，而不是结尾，因此最后发出的消息将显示在最顶端。
![](./images/8-11.png)

图11-8 向广播日志中添加一条新消息
join块创建了一条新记录：来自+8613901231234的短信已经广播。

每次短信广播之后，这条记录将被添加到LogLabel.Text的第一行，使最新的记录一直出现在顶部。join块中各个文本片段的顺序决定了日志中记录的顺序。在本例子中，新消息被编排在前三个插槽中，而LogLabel.Text，已经保存的现有记录，将插入最后一个插槽。

“的短信已经广播。n”中的“n”称为换行符，它让每条记录单独占一行，像这样：

* 来自+8613030123668的短信已经广播。
* 来自+8613901231234的短信已经广播。
关于使用foreach来显示列表的详细信息，请参见第20章。

## 将BroadcastList保存在数据库中

现在应用算是大功告成了，但通过前几章的学习，你可能猜到了一个问题：如果管理员将应用关闭再重新启动时，广播列表中的数据将会丢失，每个人都得重新注册。为了解决这个问题，要使用TinyDB组件实现BroadcastList列表在数据库中的存储和检索。

这里将使用与“出题”应用（第10章）中相类似的方案：

* 每次添加新项时，将列表保存到数据库中；
* 应用启动时，从数据库中加载列表，并保存到一个变量中。

用表11-7中所列的块，将列表存储到数据库中。TinyDB组件中的tag作为数据的标识，将保存在数据库中的不同数据区分开来。在本例中，你可以将数据标记为“broadcastList”。在Texting1.MessageReceived中，将这些块添加到add items to list块之下。

表11-7 用TinyDB来存储列表所需的块

|块的类型|	所在抽屉|	作用|
|:----|:-----|:----|
|TinyDB1.StoreValue	|TinyDB1	|将数据保存到数据库中|
|“broadcastList”	|Text	|将其插入StoreValue的tag插槽中|
|get global BroadcastList	|Variables	|将其插入StoreValue的value插槽中|

## 块的功能

当应用收到短信“joinFMDT”，并将新成员的手机号码添加到列表时，调用TinyDB1.StoreValue将BroadcastList保存到数据库中。tag（“broadcastList”）的使用是为了便于之后对数据的检索。如图11-9，被StoreValue调用的值（valueToStore）是变量BroadcastList。
![](./images/9-10.png)

图11-9 调用TinyDB来存储BroadcastList列表


## 从数据库加载广播列表（BroadcastList）

每次应用启动时都要加载广播列表，按照表11-8中列出的块来实现这一功能。应用的启动将触发Screen1.Initialize事件，因此将在该事件的处理程序中实现加载。使用存储时的tag（“broadcastList”）来调用TinyDB.GetValue。就像前几章一样，我们需要检查是否的确有数据返回，这里将检查返回值是否为列表，因为如果列表中没有数据，那么它也就不是列表。


### 块的作用

应用启动将触发Screen1.Initialize事件。如图11-10所示，使用TinyDB1.GetValue块向数据库请求数据，返回的数据临时保存在已定义的变量valueFromDB中。

表11-8 应用启动时加载广播列表所需要的块

|块的类型	|所在抽屉	|作用|
|:---|:---|:----|
|initialize global valueFromDB to	|Variables	|用于保存并检查数据库返回值的临时变量|
|“”	|Text	|设valueFromDB初始值为空|
|Screen1.Initialize	|Screen1	|应用启动时触发该事件|
|set global valueFromDB to	|Variables	|将数据库返回值暂时存放在其中|
|TinyDB1.GetValue	|TinyDB1	|向数据库请求数据|
|“broadcastList”	|Text	|将其插入GetValue的tag插槽|
|if|	Control|	判断数据库中是否有数据|
|is a list	|Lists	|如果数据库返回值是一个列表，则返回值不为空|
|get global valueFromDB	|Variables|	将其插入is a list?块|
|set global BroadcaseList to	|Variables	|将变量值设置为数据库的返回值|
|get global valueFromDB|	Variables	|数据库返回值不为空时，将返回值写入广播列表|
|call displayBroadcastList	|Procedures|	加载数据成功后，显示数据|
![](./images/10-10.png)
图11-10 从数据库中加载广播列表BroadcastList
事件处理程序中的if块是必需的，因为在首次启动应用时，数据库将返回空文本（“”），这时还没有生成广播列表。通过判断valueFromDB是否为列表，可以确定是否真的有数据返回。如果没有，则跳过那些保存返回数据以及显示数据的块。

![](./images/test-3.png)

测试：对于涉及数据库操作的应用，不适合做实时测试，因为每次连接测试设备时，都会清空数据库。为了测试数据库以及Screen.Initialize事件处理程序，需要将应用打包并下载到手机【点击“buildApp(provide QR code for .apk)”，下载并安装应用】。在手机上启动应用，用另外两部手机发送“joinFMDT”加入群组，再退出应用。当重启应用时，如果那些电话号码还在，说明数据库部分工作正常。

## 完整的广播中心应用
![](./images/11a.png)

图11-11完整的广播中心应用中的块
## 改进

在庆祝一个如此复杂的应用完工的时候，你也许想要做进一步的改进。例如：

* 在广播短信环节，广播中心向所有人发出短信，也包括发送这条短信的列表成员。修改此功能，将短信群发给除了发送者之外的所有成员；
* 允许列表成员退出群组，用手机发送短信“quitabc”给广播中心，请求从列表中删除自己。需要使用remove from list块；
* 管理员可以在操作界面上添加或删除广播列表中的成员（手机号）；
* 管理员可以指定某些不允许加入列表的手机号；
* 细化应用的功能，让任何人都可以加入到列表并接收广播，但只有管理员可以广播消息；
* 进一步细化应用，让任何人都可以加入到接收广播，但只有一个固定列表中的电话号码可以向全体成员广播消息（这正式赫尔辛基事件 的成功之处）；
* 应用中的广播列表可以永久保存，但日志却不能。每次关闭该应用再重新打开时，日志也从头开始。改进一下，让日志也能永久保存。

## 小结

以下是本章涵盖的内容：

* 应用不仅可以响应用户发起的事件，也可以响应非用户发起的事件，像收到短信这样的事件。这意味着应用的用户也可以是其他手机；
* ifelse与foreach块的嵌套使用可以构造出复杂的行为。有关条件语句if和循环语句foreach的详细信息，请参见第18章及第20章；
* join块可以用来创建一个多重内容组成的文本对象；
* TinyDB可以用于数据库操作：存储及检索数据。通用方案是，在数据发生变化时，调用StoreValue更新数据库；在应用启动时，调用GetValue从数据库中读取数据。
