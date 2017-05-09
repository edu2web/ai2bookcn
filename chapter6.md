第6章 巴黎地图旅游
本章将创建一个“向导”应用，带给你一次巴黎的梦幻之旅。而你的朋友，虽然不能与你同行，也能借此做一次虚拟的巴黎之旅。创建一个完整的地图应用看似复杂，不过App Inventor提供了ActivityStarter组件，可以为每个选定的虚拟位置打开对应的谷歌地图。创建过程分为两步，首先通过点选菜单打开埃菲尔铁塔、卢浮宫以及巴黎圣母院的地图；然后修改有关参数，使应用同时适用于卫星视图及普通地图视图。


学习要点

本章介绍以下App Inventor组件及概念：

ActivityStarter组件：可以在当前应用中打开其他Android应用。本章用它来打开带有多个参数的谷歌地图；
ListPicker组件：用户可以从地点列表中进行选择。
设计组件


图6-1 设计器中的巴黎地图旅游
首先创建一个名为“ParisMapTour”的新项目，界面中包含：

Image组件——显示一张巴黎的图片；
Label组件——显示文字；
ListPicker组件——用一个关联按钮来打开列表；
ActivityStarter组件：非可视组件。
组件设计如图6-1所示。

表6-1中列出了创建用户界面所用的组件。从Palette中拖出组件，并修改为指定的名称。

表6-1 巴黎地图旅游中用到的组件
组件类型	面板中分组	命名	作用
Image	User Interface	Image1	在屏幕上显示巴黎的静态图片
Label	User Interface	Label1	显示文本：用Android发现巴黎
ListPicker	User Interface	ListPicker1	显示备选目的地列表
ActivityStarter	Connectivity	ActivityStarter1	选择目的地之后打开地图应用
设置ActivityStarter组件的属性

ActivityStarter组件用于在当前应用中，打开其他任何Android应用，如浏览器、谷歌地图，甚至你自己的应用。当用户从你的应用中打开另一个应用时，可以通过单击“后退”按钮返回到你的应用。在ParisMapTour应用中，将根据用户选择打开地图应用，来显示指定的地图。用户可以点击“后退”按钮返回到你的应用中，并继续选择不同的目的地。

ActivityStarter是一个相当底层的组件，需要为它设置一些属性信息，这些信息对于Java Android SDK程序员来说非常熟悉，但对世界上其他99.999％的人来说都很陌生。在本应用中，输入如表6-2中指定的属性，并要小心地使用大小写字母，这非常重要。

表6-2 用ActivityStarter打开谷歌地图必须设置的属性
属性	值
Action	android.intent.action.VIEW
ActivityClass	com.google.android.maps.MapsActivity
ActivityPackage	com.google.android.apps.maps
还有一个DataUri属性必须在块编辑器中设置，用于在谷歌地图中打开指定地图。这个属性是动态的，即根据用户对目的地的选择而改变：埃菲尔铁塔、卢浮宫或巴黎圣母院。

现在切换到块编辑器，在编程添加组件行为之前，还有两个细节需要特别关照一下：

下载文件metro.jpg并加载到项目中，将其设置为Image1的Picture属性；
ListPicker组件自带一个按钮，当用户点击时将列出备选项。将ListPicker1的Text属性设置为“选择巴黎的目的地”。
为组件添加行为

在块编辑器中，需要定义一个目的地列表，并设定两种行为：

当应用开始时，为ListPicker组件加载目的地列表，以供用户选择；
当用户从ListPicker中选择了一个目的地，地图应用将打开，并显示目的地地图。在应用的第一个版本中，只需打开地图，并搜索选中的地点。
创建目的地列表

打开块编辑器，用表6-3中列出的块创建一个destinations列表变量。

表6-3 创建destinations列表变量所需的块
块的类型	所在抽屉	作用
Initialize global destinations to	Variables	创建一个目的地列表
make a list	Lists	添加列表项
“埃菲尔铁塔”	Text	第一个目的地
“卢浮宫”	Text	第二个目的地
“巴黎圣母院”	Text	第三个目的地
如图6-2所示，变量destinations将调用make a list函数，其中插入了三个旅游目的地。


图6-2 在App Inventor中创建列表非常容易
让用户选择一个目的地

ListPicker组件用来显示列表项供用户选择。通过将ListPicker的Elements属性设置为某个list，可以预先将备选项目加载到ListPicker中。这里将ListPicker的Elements属性设置为刚刚创建的destinations列表。因为想在应用启动时显示此列表，因此加载列表行为必须在事件Screen1.Initialize中定义。表6-4中列出了所需的块。

表6-4 在应用启动时加载ListPicker备选项所需的块
块的类型	所在抽屉	作用
Screen1.Initialize	Screen1	应用启动时触发该事件
set ListPicker1.Elements to	ListPicker1	将Elements属性设置为需要显示的列表
get global destinations	Variables	读取目的地列表
块的作用
应用启动时触发Screen1.Initialize，如图6-3所示，事件处理程序通过设置ListPicker的Elements属性来显示三个备选目的地。


图6-3 在应用启动时需要执行的某些操作必须放在Screen1.Initialize事件处理程序中

测试：首先点击”connect”重新连接测试设备；然后在手机上点击 “选择巴黎的目的地”按钮，出现有三个选项的列表。

使用搜索打开地图

下面编写程序，当用户选中目的地时，ActivityStarter将打开谷歌地图并搜索选定的位置。

当用户选中ListPicker中的项目时，将触发ListPicker.AfterPicking事件；在AfterPicking事件的处理程序中，需要设置ActivityStarter组件的DataUri属性，让它知道要打开哪里的地图，然后用ActivityStarter.StartActivity打开谷歌地图。表6-5列出了此功能所需的块。

表6-5 用ActivityStarter打开谷歌地图所需要的块
块的类型	所在抽屉	作用
ListPicker1.AfterPicking	ListPicker1	当用户选择ListPicker中的某项时触发该事件
set ActivityStarter1.DataUri to	ActivityStarter1	DataUri告诉Maps在启动时打开哪里的地图
join	Text	将两段文本连接成DataUri
“http://maps.google.com/?q=”	Text	Maps所需的DataUri信息中的第一部分
ListPicker1.Selection	ListPicker1	用户选中的项（DataUri信息中的第二部分）
ActivityStarter1.StartActivity	ActivityStarter1	打开地图
块的作用
用户在ListPicker中选定的项存储在ListPicker.Selection中，选择触发了AfterPicking事件。如图6-4，DataUri属性是“http://maps.google.com/?q=”与选中项组合。如果用户选中了第一项“艾菲尔铁塔”，则DataUri将被设置为http://maps.google.com/?q=埃菲尔铁塔。


图6-4 设置DataUri打开选中的地图
为打开地图设定了ActivityStarter的其他属性，因此ActivityStarter1.StartActivity块将打开地图应用，并根据DataUri所限定的条件进行搜索。


测试：重新连接测试设备，点击“选择巴黎的目的地”按钮。当选中了某个地点，是否出现了该地点的地图？谷歌地图提供的“后退”按钮，可以返回到你的应用中并做再次选择，怎么样，有用吗？（可能需要多次单击后退按钮）

设立虚拟旅游

现在我们给应用增添一点趣味，让应用打开一些超级放大的地图，并欣赏巴黎的街景，以便在你旅游的同时，你的朋友在家也能跟随你游览的脚步。要做到这一点，你首先要在谷歌地图中找到那些特定地图的URL地址。继续使用相同的巴黎地标性建筑为目的地，但是当用户选中目的地时，使用选中项的索引值（在列表中的位置），来选择并打开一个指定放大倍数的地图或街景图。

在进行下一步之前，您可能想把到目前为止创建的简单地图应用保存起来（Save Project As），以便接下来所做的事情万一让应用出了问题，你还可以随时找回现有版本，并再试一次。

为特定地图寻找DataUri

首先在电脑上打开谷歌地图，搜索某个目的地的具体地图：

在计算机上浏览http://maps.google.com；
搜索地标（例如，艾菲尔铁塔）；
放大到你预期的级别；
选择你想要的视图类型（如：地址、卫星或街道视图）；
点击地图窗口左上部的Link(链接)按钮，（如图6-5）复制地图的URL地址。这个网址（或部分网址）将用于打开你应用中的地图。

图6-5 从浏览器中获取特定地图的URL地址
表6-6显示了即将使用的URL地址。

表6-6 在谷歌地图上完成虚拟之旅的URL地址
地标	地图URL
埃菲尔铁塔	https://maps.google.com/maps?q=%E5%9F%83%E8%8F%B2%E5%B0%94%E9%93%81%E5%A1%94&hl=zh-CN&ie=UTF8&ll=48.858369,2.294482&spn=0.001578,0.004128&sll=37.0625,-95.677068&sspn=61.153041,135.263672&t=h&z=19&iwloc=A。短网址：http://goo.gl/maps/e3oBk
卢浮宫	https://maps.google.com/maps?q=%E5%8D%A2%E6%B5%AE%E5%AE%AB&hl=zh-CN&ie=UTF8&ll=48.86061,2.337642&spn=0.003155,0.008256&sll=48.85826,2.294582&sspn=0.001578,0.004128&t=h&z=18
巴黎 圣母 院（街景）	https://maps.google.com/maps?q=%E5%B7%B4%E9%BB%8E%E5%9C%A3%E6%AF%8D%E9%99%A2&hl=zh-CN&ie=UTF8&ll=48.852545,2.348389&spn=0.006311,0.016512&sll=48.861595,2.33522&sspn=0.001585,0.004128&t=h&z=17&layer=c&cbll=48.852545,2.348389&panoid=3yzCn-eLwNIAAAQJORRDXw&cbp=12,30.77,,0,-16.32
将表6-6中的网址粘贴到浏览器中，你会看到前两个是放大的卫星视图，而第三个是街景图。

可以用这些URL直接打开地图，也可以用http://mapki.com中列出的谷歌地图协议。例如，仅凭GPS坐标来显示埃菲尔铁塔地图。从表6-6的长地址中找出这些坐标及地图geo:协议：

geo: 48.858369,2.294482&t=h&z=19
使用这样的DataUri打开的地图，与包含这些GPS坐标的长地址打开的地图基本相同。t = h特指既可以显示卫星视图，又可以显示地址视图的混合模式，而z = 19特指缩放级别。如果你有兴趣详细了解不同类型地图的参数设置，请查看http://mapki.com中的文档。

两种类型的URL随你选择用，我们对前两个DataUri使用geo格式，第三个使用长地址格式。

定义DataURIs列表

创建列表变量DataURIs，其中包含了每个地图的DataURI，如图6-6所示，其中的选项分别与destinations列表中的选项相对应（即第一个dataURI对应第一个目的地：埃菲尔铁塔）。


图6-6 虚拟之旅的地图地址列表
前两项显示了埃菲尔铁塔和卢浮宫的DataURIs，使用geo:协议。第三个DataURI是在太长，无法完整显示。从表6-6的最后一行复制此网址，并粘贴在text块中。

修改ListPicker.AfterPicking行为

在第一个版本的ListPicker.AfterPicking中，将ActivityStarter1的DataUri属性设置为字符串“http://maps.google.com/?q=”与用户所选的目的地（如“埃菲尔铁塔”）的串联（或组合）；在第二个版本中，AfterPicking程序则更为复杂，用户从一个列表（目的地）中选择，而DataUri要从另一列表（dataURIs）中读取。具体来说，当用户从ListPicker中选中一项，此时需要知道选中项的索引，以便用这个索引从dataURIs列表中选择正确的DataUri。稍后我们会详细解释索引的含义，但为了更好地描述这一概念，要先把这些块创建出来，便于我们理解。这项功能需要许多块，均在表6-7中列出。

表6-7 根据用户在第一个列表中的选择，来确定在第二个列表中的选择，需要如下的块
块的类型	所在抽屉	作用
Initialize global index to	Variables	变量用来保存用户所选项的索引
数字1	Math	变量index的初始值设为1
ListPicker1.AfterPicking	ListPicker1	当用户选中一项时，触发该事件
set global index to	Variables	将变量值设为所选项在列表中的排列位置
index in list	Lists	获得所选项的位置（索引）
ListPicker1.Selection	ListPicker1	所选项如"埃菲尔铁塔"，将其插入index in list的thing插槽
get global destinations	Variables	将其插入index in list的list插槽
set ActivityStarter1.DataUri	ActivityStarter1	在启动Activity打开地图之前，设置该属性
select list item	Lists	从dataURIs列表中选择一项
get global DataURIs	Variables	读取DataURIs列表
块的功能
当用户选中ListPicker1中的某项时触发AfterPicking事件，如图6-7所示。选中的项，如“埃菲尔铁塔”，成为ListPicker.Selection。AfterPicking事件的处理程序借此找到所选项在列表destinations中的位置，即索引值。索引值对应于所选目的地在列表中的位置。所以，如果“艾菲尔铁塔”被选中，则index为1；如果“卢浮宫”被选中，index为2；同样，如果“巴黎圣母院”被选中，则index为3。


图6-7 根据用户在一个列表中的选择，在另一个列表中做对应的选择
使用索引在另一个列表（本例中DataURIs）中做选择，选中项被设置为组件ActivityStarter的DataUri属性。一旦设置完成， ActivityStarter.StartActivity就可以打开地图了。


测试：在手机上点击“选择巴黎的目的地”按钮，将出现有三个选项的列表。选择其中一个，看看会出现哪张地图。

改进

下面是一些可以尝试的改进建议：

创建一个虚拟旅游，目的地富有异国情调，也可以是你的工作场所或学校；
创建一个可定制的虚拟旅游应用，让用户自己输入旅游地点，并地图URL连接，来生成一个旅游向导。相关数据需要存储到TinyWebDB数据库中，并且让应用可以调用这些已经输入的数据。有关创建TinyWebDB数据库的例子，请参见出题及测验应用。
小结

下面是本章涉及到的概念：

列表变量(List Variable)：可用于保存像旅行目的地以及地图URL这样的多条目数据；
ListPicker组件：允许用户从项目列表中选择。ListPicker的Elements属性用来保存列表内容，Selection属性用来保存选中的项，而用户选中后将触发AfterPicking事件；
ActivityStarter组件：用于打开另一个应用。本章展示了如何调用地图应用，你也可以打开浏览器或任何其他的Android应用，甚至是你自己创建的应用。更多信息请参见http://appinventor.googlelabs.com/learn/reference/other/activitystarter.html（无法访问）；
想要打开谷歌地图中的详细地图，可以设置ActivityStarter的DataUri属性。如何确定DataUri的值呢？可以在浏览器中打开详细地图，然后点击链接按钮，获得并使用URI：
直接将URI设置为ActivityStarter的DataUri属性；
或者使用在http://mapki.com中定义的协议，创建你自己的URI。
Index（索引）：根据某一项在整个列表中的位置来定义索引。在ListPicker块中，可以根据用户选中项在列表中的位置来确定索引。获取索引值是至关重要的，例如在本章中，需要用索引值在第二个相关的列表中进行选择。关于List变量及ListPicker组件的更多信息，请参见第19章。
