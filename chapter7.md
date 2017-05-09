第7章 安卓，我的车在哪？
你把车停得尽量靠近体育馆，但演唱会一结束，你却忘了车停在哪儿，你的同伴也很茫然。幸运的是，你的Android手机还在，它从来不忘事，你新装了一款热门应用“Android，我的车在哪儿？”有了这个应用，在停车时点一下按钮，Android的位置传感器会“记住”车的GPS坐标和地址。当稍后重新打开应用时，它会指给你从现在位置到停车位置的方向，问题解决了！


学习要点

本章涵盖如下概念：

LocationSensor组件：确定Android设备的位置；
TinyDB组件：直接在设备数据库中记录数据；
ActivityStarter组件：在应用中打开谷歌地图，并显示从一个位置到另一个位置的方向。
准备开始

登陆App Inventor网站，开始一个新项目“AndroidWhere”（项目名称不能有空格），将屏幕标题设置为“Android，我的车在哪儿？”，连接测试手机。

设计组件

应用包含下列可视组件：

多个Label组件：显示当前位置和“记住”的位置信息，有些Label显示静态文本，如GPSLabel显示“GPS：”；其他Label，如CurrentLatLabel显示来自位置传感器的数据。给这些Label设定一个默认值（0,0），当GPS取得位置信息时，这个值将随之改变；
两个Button组件：记录位置和指示该位置的方向；
以及三个非可视组件：

LocationSensor组件：获取当前位置信息；
TinyDB组件：永久保存位置信息；
ActivityStarter组件：用于打开谷歌地图，以获得当前位置和记住位置之间的路线。
按照图7-1所示的组件设计器截图来创建组件。


图7-1 组件设计器中应用的用户界面
跟随表7-1，逐个拖出组件，并做相应设置，创建如图7-1所示的用户界面。

表7-1 应用中的所有组件
组件类型	面板中分组	命名	作用
Label	User Interface	CurrentHeaderLabel	显示标题“当前位置”
HorizontalArrangement	Screen Arrangement	CurrentAddrArrangement	放置地址信息
Label	User Interface	CurrentAddressLabel	显示“地址：”
Label	User Interface	CurrentAddressDataLabel	显示动态数据：当前地址
HorizontalArrangement	Screen Arrangement	CurrentGPSArrangement	安置GPS信息
Label	User Interface	GPSLabel	显示“GPS：”
Label	User Interface	CurrentLatLabel	显示动态数据：当前纬度
Label	User Interface	CommaLabel	显示“，”
Label	ser Interface	CurrentLongLabel	显示动态数据：当前经度
Button	User Interface	RememberButton	点击记录当前位置
Label	User Interface	RememberedAddressTitleLabel	显示“已记录的地点”
HorizontalArrangement	Screen Arrangement	RememberAddrArrangement	安置已保存的GPS信息
Label	User Interface	RememberedAddressLabel	显示“地址：”
Label	User Interface	RememberedAddressDataLabel	显示动态数据：已记录的地址
HorizontalArrangement	Screen Arrangement	RememberGPSArrangement	安置已记录的GPS信息
Label	User Interface	RememberedGPSlabel	显示“GPS：”
Label	User Interface	RememberedLatLabel	显示动态数据：已记录的纬度
Label	User Interface	Comma2Label	显示“，”
Label	User Interface	RememberedLongLabel	显示动态数据：已记录的经度
Button	User Interface	DirectionsButton	点击来显示地图
LocationSensor	Sensors	LocationSensor1	感知GPS信息
TinyDB	Storage	TinyDB1	永久保存已记录的位置信息
ActivityStarter	Connectivity	ActivityStarter1	打开地图
用以下方式设置组件属性：

设置显示静态文本的Label的Text属性为固定文本，参照表7-1；
设置显示动态GPS数据的Label的Text属性为“0.0”；
设置显示动态地址的Label的Text属性为“未知”；
取消勾选RememberButton和DirectionsButton的Enabled属性（设置为不可用）；
设置ActivityStarter属性（表7-2），以便ActivityStarter.startActivity可以打开谷歌地图。（图7-1中ActivityStarter的属性显示不完整。）表7-2中未列出的属性可以留空。
表7-2 打开谷歌地图所要设定的ActivityStarter属性
属性	值
Action	android.intent.action.VIEW
ActivityClass	com.google.android.maps.MapsActivity
ActivityPackage	com.google.android.apps.maps

提示：ActivityStarter组件可在应用中打开安装在设备上的任何其他Android应用。要打开地图，表7-2中的属性必须一字不差地输入；要打开其他应用，请参阅http://appinventor.googlelabs.com/learn/reference/other/activitystarter.html 中的App Inventor文档。

为组件添加行为

需要为应用设定如下行为：

当LocationSensor读取到位置信息时，将数据填写到相应的Label中，表示传感器已经读取到当前位置信息，用户这时可以选择保存此位置信息；
当用户点击RememberButton时，当前位置信息被复制到“已记录的地点”名下的Label中。这些信息要保存到设备数据库中，以便用户关闭并再次打开应用时，数据不会消失；
当用户点击DirectionsButton时，打开谷歌地图，并显示“已记录”位置的方向；
当应用重新启动时，从数据库中加载“已记录”的位置信息。
显示当前位置

两种情况会触发LocationSensor.LocationChanged事件，（1）传感器首次读取位置信息时；（2）设备的位置变化，传感器读数更新时。首次读数有时仅需几秒钟，但如果GPS卫星信号受到屏蔽，会一直没有读数（也与设备的设置有关）。有关GPS和LocationSensor的更多信息，请参见第23章。

在读取到位置信息时，程序要将数据写到相应的Label中。表7-3列出了所有相关的块。

表7-3 读取到位置信息时，用户界面显示这些信息所需要的块
块的类型	所在抽屉	作用
LocationSensor1.LocationChanged	LocationSensor	当手机收到新的GPS读数时，触发该事件
set CurrentAddressDataLabel.Text to	CurrentAddressDataLabel	将当前地址的新数据写入label
LocationSensor1.CurrentAddress	LocationSensor	该属性保存了街道地址信息
set CurrentLatLabel.Text to	CurrentLatLabel	将纬度信息写入相应的label
get latitude	Variables	插入set CurrentLatLabel.Text to块的插槽
set CurrentLongLabel.Text to	CurrentLongLabel	将经度信息写入相应的label
get longitude	Variables	插入set CurrentLongLabel.Text to块的插槽
set RememberButton.Enabled to	RememberButton	设置“记住我现在的位置”按钮属性
true	Logic	插入set RememberButton.Enabled to插槽
块的作用
如图7-2所示，latitude（经度）和longitude（纬度）是LocationChanged事件的参数，因此可以从Variables抽屉中抓取；但CurrentAddress则不是参数，而是LocationSensor的属性，因此要从LocationSensor抽屉里抓取。LocationSensor除了获取GPS位置信息之外，还通过调用谷歌地图，获得了与位置信息相对应的街道地址信息。

事件处理程序还启用了RememberButton，该按钮的初始设置为禁用（未选中），因为在传感器获得读数之前，用户不需要“记住”什么，而现在我们可以为“记住”行为编写程序了。


图7-2 使用LocationSensor读取当前位置信息

测试：用手机（wifi与电脑连接）实时测试位置感知应用是无效的。将程序打包并下载到手机上：选择“buildApp(provide QR code for .apk)”，按照提示在手机上打开应用。GPS及地址信息显示在屏幕上，同时RememberButton变为可用。

如果没有获得读数，检查一下Android设备的位置及安全性设置，并尝试走到户外。要了解更多信息，请参见第23章。

记录当前位置

当用户点击RememberButton时，当前位置信息被写入“已记录的地点”下方的label中。表7-4显示了实现这一功能所需要的块。

表7-4 记录并显示当前位置所需要的块
块的类型	所在抽屉	作用
RememberButton.Click	RememberButton	用户点击按钮时触发该事件
set RememberedAddressDataLabel.Text to	RememberedAddressDataLabel	将传感器获得的地址信息写入“已记录”label中
LocationSensor1.CurrentAddress	LocationSensor	该属性保存了街道地址信息
set RememberedLatLabel.Text to	RememberedLatLabel	将纬度信息写入“已记录”label中
LocationSensor1.Latitude	LocationSensor	该属性保存了纬度信息
set RememberedLongLabel.Text to	RememberedLongLabel	将经度信息写入“已记录”label中
LocationSensor1.Longitude	LocationSensor	该属性保存了经度信息
set DirectionsButton.Enabled to	DirectionsButton	设置DirectionsButton的Enabled属性
true	Logic	设置DirectionsButton的Enabled属性为真
块的作用
当用户点击RememberButton时，当前位置信息将写入“已记录”label中，如图7-3所示。

注意到DirectionsButton已可用，这会有点儿小麻烦，因为如果用户立即点击DirectionsButton，记住的位置也是当前位置，因而地图中不会提供方向有关的信息。但是，人们似乎不会这么做，当用户移动位置时（例如步行到演唱会），则当前位置将偏离已记录的位置。


图7-3 将当前位置信息写入“已记录”label中

测试：将应用的新版本下载到手机，并再次测试。当单击RememberButton时，当前位置信息是否被写入到“已记录”的label中？

显示“已记录”位置的方向

当用户点击DirectionsButton 时，应用将打开谷歌地图，地图中显示从用户当前位置到“已记录”位置（即停车的位置）的方向。

ActivityStarter组件可以打开任何Android应用，也包括谷歌地图，但必须做一些相应的设置。不过像打开浏览器或地图这样的应用，设置起来相当简单。

打开地图的关键是设置ActivityStarter.DataUri属性，该属性无异于你在浏览器中直接输入的网址。要想搞清楚这一点，只需在浏览器中打开http://maps.google.com，并询问，比如旧金山与奥克兰之间的方向。当结果出来时，点击地图的左上部的链接按钮，并检查显示的URL。这正是你在应用中所需要的URL。

所不同的是，带有方向的地图涉及到两个位置，即起点和终点，它们分别用一组特定的GPS坐标来表示（而非城市之间）。该URL必须采用以下形式：

http://maps.google.com/maps?saddr=37.82557,-122.47898&daddr=37.81079,-122.47710
在浏览器中输入网址，说说看，它指引你跨越了那个著名的地标性建筑？

这里需要为URL设定动态参数：起点地址（saddr）和终点地址（daddr）。在前几章中，你已经学会用join块将文本连接起来，这里也是如此。将当前位置和已记录位置的GPS数据插入到URL中，设置ActivityStarter.DataUri属性为URL，然后调用ActivityStarter.StartActivity。表7-5列出了此项功能所需要的块。

块的作用
用户点击DirectionsButton时，事件处理程序生成一个地图URL，然后调用ActivityStarter打开地图应用并加载地图，如图7-4所示，用join创建的URL发送给地图应用。

最终的URL包含了地图域名（http://maps.google.com/maps）以及两个URL参数：saddr与daddr，用来指定方向的起点位置及终点位置。在本应用中，saddr被设定为当前位置的纬度和经度，而daddr被设定为已记录的停车位置的纬度和经度。

表7-5 打开一张带有方向指示的地图所需要的块
块的类型	所在抽屉	作用
DirectionsButton.Click	DirectionsButton	用户点击”指示方向”按钮触发该事件
set ActivityStarter1.DataUri to	ActivityStarter1	设置要打开地图的URL
join	Text	将URL的各组成部分连接起来
“http://maps.google.com/maps?saddr=”	Text	URL中固定的部分，后面接起点经纬度
CurrentLatLabel.Text	CurrentLatLabel	当前位置的纬度值
“,”	Text	放在经纬度值之间的逗号
CurrentLongLabel.Text	CurrentLongLabel	当前位置的经度值
“&daddr=”	Text	URL中的第二个参数，后面接终点经纬度
RememberedLatLabel.Text	RememberedLatLabel	已记录位置的纬度
“,”	Text	放在经纬度值之间的逗号
RememberedLongLabel.Text	RememberedLongLabel	已记录位置的经度
ActivityStarter1.StartActivity	ActivityStarter1	打开地图

图7-4 生成一个URL，用来打开地图并指示方向

测试：用手机下载新的版本并再次测试，一旦取得读数，单击RememberButton然后走开。当单击DirectionsButton时，地图是否提示您如何追溯你的脚步？点击几次后退按钮。你是否有回到了你的应用？

永久保存已记录的位置信息

现在已经具备了一个全功能的应用：记住起点位置，并从当前用户所在的位置绘制一张回到起点的地图。虽然用户“记住”了位置，但假如应用被关闭，然后再重新打开，“记住”的信息也将消失。实际上你希望用户能够记录下车的位置，关闭应用，走到别处，然后重新启动应用，并获取已记录的车辆所在位置的方向。

如果你能想起“开车不发短信”应用（第4章），说明你的思路是正确的，我们需要使用TinyDB数据库来永久保存这些数据，采取的方案也与之前的应用类似：

当用户点击RememberButton时，位置信息存储到数据库中；
当应用启动时，从数据库中加载位置信息并保存到一个变量或属性中。
从修改RememberButton.Click事件处理程序开始，来存储这些要被“记住”的信息。存储纬度、经度和地址三组信息，需要三次调用TinyDB.StoreValue。表7-6列出了所要补充的块。

表7-6 永久保存位置信息所需要的块
块的类型	所在抽屉	作用
TinyDB1.StoreValue(3)	TinyDB1	将数据保存在设备数据库中
“address”	Text	插入TinyDB1.StoreValue的tag插槽中
LocationSensor1.CurrentAddress	LocationSensor1	插入TinyDB1.StoreValue的value插槽中，永久保存地址信息
“lat”	Text	插入第二个TinyDB1.StoreValue的tag插槽中
LocationSensor.CurrentLatitude	LocationSensor	插入第二个TinyDB1.StoreValue的value插槽中，永久保存纬度信息
“long”	Text	插入第三个TinyDB1.StoreValue的tag插槽中
LocationSensor.CurrentLongitude	LocationSensor	插入第三个TinyDB1.StoreValue的value插槽中，永久保存经度信息
块的作用
如图7-5所示，TinyDB1.StoreValue将LocationSensor属性中的位置信息保存到数据库中。你该记得在“开车不发短信”中，StoreValue函数有两个参数，tag与value，tag充当已存储数据的标识，value是你实际想保存的数据，即本例中的LocationSensor数据。


图7-5 在数据库中存储被“记住”的位置信息
启动应用时读取“记住”的位置信息

将数据保存在数据库中，是为了以后可以调用它。在本应用中，如果用户在保存了位置信息之后退出应用，那么当应用重新打开时，你希望从数据库中读出信息并显示给用户。

在前几章中讨论过，应用的启动会触发Screen.Initialize事件，而在启动时从数据库中读取数据是一种惯例，我们也不例外。

使用TinyDB.GetValue函数来读取存储的GPS数据。要读取的存储数据包括地址、纬度及经度，因此要调用GetValue函数三次。像在“开车不发短信”中一样，要事先检查数据库中否保存了数据（如，第一次启动应用时，TinyDB.GetValue将返回一个空文本）。

挑战一下自己，看看是否可以独立创建这些块，然后再与图7-6进行比较。


图7-6 在应用启动时，从数据库中读取数据，如果数据不为空则显示数据
块的作用
理解这些块的方法是设想用户的使用过程：用户首次打开应用，先保存位置信息，稍后再次打开应用。首次打开应用，数据库中没有信息可加载，也不必填写“已记录”label或启用DirectionsButton。在后续的使用中，如果确有数据存储，就要从数据库中加载这些位置信息。

首先用“address”为tag（标签）调用TinyDB1.GetValue函数，之前在存储位置信息时使用过这个tag。读取的值保存在变量tempAddress中，并检查其是否为空。


undefined

测试：将新版本应用下载到手机，并再次测试。点击RememberButton，并确保“记住”读数。关闭应用并再次打开。那些数据是否还在？

完整的应用：Android，我的车在哪儿？

图7-7显示了完整的“Android，我的车在哪儿？”应用中所用到的块。


图7-7 “Android，我的车在哪儿”应用中所有的块
改进

可以尝试如下改进：

创建一个“Android，他们在哪儿？”的应用，让一群人可以了解彼此行踪。无论你在徒步旅行还是在公园散步，这个应用都有助于你节省时间，甚至可能挽救生命。应用中的数据是共享的，因此要使用web数据库，用TinyWebDB组件来替代TinyDB。更多信息请参见第22章。
创建一个“行踪”应用，用列表来记录自己的位置改变，即行踪。当记录数达到一定数量时，或超过一定时间时，开始一个新的“行踪”，因为即使是轻微的位移也会产生一个新的位置读数。这类应用需要使用列表来存储位置记录，需要帮助时请参见第19章。
小结

下面是本章涉及到的概念：

LocationSensor组件：可以报告用户的纬度、经度及当前的街区地址。当传感器首次获得数据或数据发生变化（设备移动）时，将触发LocationChanged事件。有关LocationSensor的更多信息，请参见第23章；
ActivityStarter组件：可以在一个应用中打开其他应用，包括谷歌地图。对于地图，需要将ActivityStarter的DataUri属性设置为想要打开的地图的URL地址。如果你想显示两个GPS坐标之间的方向，URL应该写成下面的格式，你可以用实际位置的GPS坐标来替换下面的示例数据：http://maps.google.com/maps/?saddr=0.1,0.1&daddr=0.2,0.2；
join用来将文本片段拼凑（连击）成单一的文本对象，也可以让静态文本与动态数据相连接。对于地图URL来说，GPS坐标就是动态数据；
TinyDB让数据永久地保存在在手机的数据库中。保存在变量或属性中数据，会随着应用的关闭而丢失，但存储在数据库中的数据，可以在每次启动应用时被载入。有关TinyDB和数据库的详细信息，请参见第22章。
