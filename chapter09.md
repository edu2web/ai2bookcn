# 第9章 木琴


很难相信，利用技术来记录和播放音乐只能追溯到1878年，也就是爱迪生获得留声机专利时。时至今日，我们已经有了长足的进步，音乐合成器、光盘、采样和混音、播放音乐的手机，甚至是拥塞的远程互联网。在本章中，通过创建一个可以录制和播放音乐的木琴应用，你也将成为这种进步的推动者。
## 作品描述

如图9-1所示，这个应用（最初由App Inventor团队的Liz Looney创建）可以做到：

图9-1 木琴应用的用户界面
* 通过触摸屏幕上的彩色按钮播放八个不同的音符；
* 按“播放”按钮，回放之前弹奏的音符；
* 按“重置”按钮清除之前弹过的音符，以便输入新曲。

## 学习要点

本章程涵盖了以下概念：

* 使用单一的声音组件来播放不同的音频文件；
* 使用Clock组件来计算并实现两个音符之间的延迟；
* 在创建一个过程时做判断；
* 创建能够自我调用的过程；
* 列表的高级应用，包括添加、删除及读取项。

## 准备开始

登陆App Inventor网站，创建新项目“Xylophone”，屏幕标题设置为“木琴”，并连接到测试手机或模拟器。

## 设计组件

应用中有13个不同的组件（其中8个Button组成了乐器键盘），见表9-1。要在编程之前一次性创建这么多组件，显得有些乏味，因此我们按功能将应用划分为若干部分，分步来创建组件，这需要在组件设计器与块编辑器之间反复切换，就像第五章“瓢虫快跑”应用中一样。
表9-1木琴应用的所有组件

| 组件类型 |面板中分组 | 命名 | 作用 |
|:------- |:--------|:------|:--------|
| Button	|User  Interface|	Button1 |	播放低音C |
|Button|	User Interface|	Button2|	播放D
|Button	|User Interface|	Button3	|播放E
|Button|	User Interface	|Button4	|播放F
|Button|	User Interface	|Button5	|播放G
|Button|	User Interface	|Button6	|播放A
|Button|	User Interface	|Button7	|播放B
|Button	|User Interface	|Button8	|播放高音C
|Sound	|Media	|Sound1	|播放音符
|Button|	User Interface	|PlayButton	|回放曲子
|Button|	User Interface	|ResetButton	|清除保存的曲子，开始新曲
|HorizontalArrangement	|Layout|	HorizontalArrangement1	|放置“播放”及“重置”按钮
|Clock|	User Interface	Clock1	记录音符之间的延迟|Clock|	User Interface	Clock1	记录音符之间的延迟
## 创建键盘

用户界面中包含了从低音C到高音C的大调五声（七音符）音阶的八个音符键盘，本节将创建这样的音乐键盘。

## 创建第一个音符按钮

首先创建前两个木琴键，用按钮来实现：

1.从面板（palette）的user interface组中拖出一个按钮，保留Button1的名称，我们希望它像木琴的键一样，是一个洋红色（Magenta）的长条，因此做如下设置：
* BackgroundColor属性：为洋红色（Magenta）；
* Text属性：为“C”；
* Width属性：为“Fill parent”，使其占满屏幕；
* Height属性：为40像素。
2.重复上述步骤创建第二个按钮，名为Button2，放在Button1下面。Width及Height属性值同Button1，但BackgroundColor属性设为红色，Text属性设置为“D”。

（稍后将重复步骤2来创建其余六个音符按钮。）在组件设计器中看起来如图9-2所示。

图9-2 用按钮来充当音符按键
在手机上的显示看起来与此相似，只是两个彩色按钮之间没有空白。

## 添加Sound组件


 
我们不能让木琴没有声音，创建一个Sound组件，名字为Sound1。MinimumInterval（最小间隔）属性设置为0（默认值为500毫秒）。这可以让我们的演奏要多快有多快，而不必等半秒钟（500毫秒）。不必设置Source属性，稍后我们会在块编辑器中设置。 下载1.wav和2.wav，并加载到项目中。与前几章不同，这里的声音文件必须保持原有文件名，不能修改，理由稍后就会明晰。后面还有六个声音文件需要加载。

## 声音与按钮的连接

当某个按钮被点击时，用程序来实现播放声音的行为，即：如果Button1被点击，则播放1.wav，如果Button2被点击，播放2.wav，等等。切换到块编辑器，如图9-3所示，进行以下设置：

1. 从Screen1项下的Button1抽屉里拖出Button1.Click块；

2. 从Sound1抽屉里拖set Sound1.Source块，放置在Button1.Click块中；

3. 输入“text”来创建一个文本块（而不是从Built-in项下的Text抽屉里拖出，这样更便捷。）设置文本值为“1.wav”，并与Sound1.Source块连接；

4. 添加Sound1.Play块。
图9-3点击按钮时播放声音
对Button2进行同样设置，如图9-4（只改了文件名），代码几乎完全重复。


图9-4 添加更多的声音
重复的代码提示我们最好是创建一个过程，像在第3章“打地鼠”和第5章“瓢虫快跑”中那样。具体来说，我们将创建一个带数字参数的过程，将Sound1的Source属性设置为相应的声音文件，并播放该声音文件。这是对程序进行重构改进而又不改变程序行为的又一个例子，这一概念在“打地鼠”一章中首次引入。用join块将数字（如1）与文本“.wav”连接起来，创造出正规的文件名（如“1.wav”）。下面是创建这个过程的步骤：

1. 在块编辑器中打开Procedures抽屉，拖出“to procedure”块；
2. 单击procedure将过程名改为playNote；
3. 点击procedure块左上角的蓝色方块呼出内部组件，将一个input x块插入“inputs”块；
4. 将input x块中的x改为number；
5. 将set Sound1.Source to块从Button1.Click事件处理程序中拖出，放在PlayNote过程内“do”的右边，Sound1.Play块也将随之移动；
6. 将1.wav块拖入垃圾桶；
7. 从Text抽屉中拖出join块放到set Sound1.Source to的插槽内；
8. 将鼠标悬停在playNote的number参数上，呼出并拖动get number块，并将其放入join块的第一个插槽中；
9. 从Text抽屉中拖出空文本块，放在join块的第二个插槽中；

##将文本值设置为“.wav”。（切记不要输入引号）; ##从Procedures抽屉中拖出call PlayNote块，放到空的Button1.Click内； ##在number插槽中插入文本“1”。 现在，当Button1被点击时，过程PlayNote将以数字1为参数被调用。该过程将Sound1.Source属性设为“1.wav”，并播放该声音。 创建一个Button2.Click块，调用参数为2的PlayNote过程。（可以复制现有的PlayNote块，将其移动到Button2.Click块内，并将参数更改为2；也可以复制整个Button1.Click块，然后将Button1改为Button2，再将参数1改为2。）程序如图9-5所示。



图9-5 创建一个过程来演奏音符
告诉Android加载声音 此时在手机上测试程序会让你失望：第一次按键时，不但没听到预想的声音，手机还弹出错误提示：“Error 703：Unable to play 1.wav”（不能播放1.wav）；第二次再按同一个键时，才听到声音。这是因为Android系统是在程序运行时才加载声音文件（只需加载一次），加载过程需要一点时间。第一次按键，当call Sound1 play块开始执行时，set Sound1.Source to块的加载任务尚未完成，因此系统给出错误提示；等到第二次按键时，声音文件已经加载完成，因此可以正常播放。为什么前几章没有出现过这个问题？因为我们在组件设计器中预先设置了Sound组件的Source属性为某个声音文件，当程序启动时，声音文件会自动加载。而这里，直到程序启动之后，我们也没有对Sound1.Source进行设置，因此没有对声音做初始化。我们必须在程序启动时直接加载声音文件，如图9-6所示。


图9-6 在应用启动时加载声音文件

测试：在手机中重新启动应用，按键之后立刻播放声音。（如果你没有听到声音，请确保手机上的媒体音量没有被设置为静音。）
## 实现其余的音符

两个按钮已经实现了演奏音符的功能，现在需要回到组件设计器，加载其余六个声音文件3.wav、4.wav、5.wav、6.wav、7.wav和8.wav，并添加其余六个音符。首先创建六个新Button组件，重复此前的步骤，但Text及backgroundColor属性的设置有所不同，具体设置如下：

* Button3（“E” Pink / 粉红色）
* Button4（“F”，Orange / 橙色）
* Button5（“G”，Yellow / 黄色）
* Button6（“A”，Green / 绿）
* Button7（“B”，Cyan / 青色）
* Button8（“C”，Blue / 蓝）

Button8的TextColor属性需要改为白色，这样更加醒目，如图9-7所示。


图9-7 在组件设计器中放置其余的声音按钮
回到块编辑器中，为每个新按钮创建Click块并以相应的参数调用PlayNote过程。同样，在Screen.Initialize中加载新的声音文件，如图9-8所示。


测试：现在所有按钮都已经就绪，点击不同按钮会演奏不同的音符。

图9-8对按钮单击事件编程，使得键盘与音调相对应
## 记录并回放音符

用按键来弹奏音符的确有趣，但如果能录制并播放歌曲岂不更好。为了实现回放功能，需要记录弹奏的音符并加以保存。除了要记录弹奏的音高（声音文件），还要记录两个音符之间的时间长度，否则将无法表现两个连续快弹音符与两个间隔10秒的音符之间的差别。 我们需要维护两个列表，每弹奏一个音符，两个列表中都会各自添加一条记录：

notes：包含与演奏的音符相对应的声音文件名，按照演奏顺序排列；
times：记录音符演奏时的时间点。

提示：在继续之前，不妨复习一下在“总统测验”中所学到的关于列表的知识。
我们可以从Clock组件中得到计时信息，因此也可以用来正确地设定音符的回放速度。

## 添加组件

在设计器中添加一个Clock组件及“播放”和“重置”按钮，按钮放在HorizontalArrangement中：

1. 拖入一个Clock组件，它将出现在“不可见组件”区域，取消勾选TimerEnabled属性，因为我们希望在回放期间，计时器听从我们的调遣，适时地启动并完成计时；
2. 从layout组中拖出一个HorizontalArrangement组件放在按钮下面，Width属性设为“Fill parent”；
3. 从User Interface组中拖动一个按钮，改名为PlayButton，Text属性设为“播放”；
4. 拖出另一个按钮并放在PlayButton右侧，改名为ResetButton，Text属性设为“重置”。

图9-9中显示了应用在设计视图中的外观。


图9-9记录并回放声音的组件被添加到设计器中
## 记录音符及时间

回到块编辑器中，为组件添加正确的行为。我们需要维护两个列表：notes与times，每次用户按下一个按钮，就向列表中添加一项：

1. 从Variables抽屉中拖出一个initialize global name to块来定义一个新的变量；
2. 单击“name”将变量命名为“notes”；
3. 打开Lists抽屉，拖动一个make a list块，将其放置在变量notes的插槽中；

这样就定义了一个名为“notes”的空列表。重复上述步骤定义另一个变量，命名为“times”。块的样子如图9-10所示。


图9-10 设置变量来记录音符
## 块的功能

每演奏一个音符，需要保存两项数据：声音文件名（保存到notes列表），以及演奏瞬间的时刻（保存到times列表）。用Clock1.Now块来记录时刻，它返回当前时刻的时间值（例如，2011年3月12日上午8时33分14秒），精确到毫秒。这些数据可以通过Sound1.Source和Clock1.Now块获得，将分别被添加到notes及times列表中，如图9-11所示。


图9-11 将演奏的声音添加到列表中
例如，如果你演奏“哆来咪哆咪哆咪”[CDECECE]，你的列表中最终会有七条记录，可能是：

* notes：1.wav，2.wav，3.wav，1.wav，3.wav ，1.wav，3.wav
* times[日期省略]：12:00:01，12:00:03，12:00:04，12:00:05，12:00:06，12:00:07，12:00:08

当用户按下“重置”按钮时，我们希望清空这两个列表。由于用户看不到清空带来的任何变化，因此添加一个Sound1.Vibrate块，通过振动来告知用户按键生效了，这种设置对用户来说是非常友好的。图9-12显示了这一功能用到的块。


图9-12 为用户的“重置”操作提供反馈
## 音符的回放

作为一个思想实验，先来考虑如何实现音符的回放，而暂时忽略回放速度。我们可以（但不会）通过创建图9-13中的那块来实现这个暂时的目标：

* 变量count用来跟踪notes列表中当前正在播放的音符的索引（位置）；
* 新过程 PlayBackNote，用来播放当前音符，并移动到下一个音符；
* 编写PlayButton.Click事件处理程序，设置count为1，只要列表中有保存的音符，就调用PlayBackNote。
## 块的功能

这可能是你第一次看到能自我调用的过程。这件事乍一看好像不可能，但实际上这是计算机科学中一个非常重要的概念：强大的递归。 为了更好地了解递归的工作原理，我们来一步一步地探究，当用户演奏了三个音符（ 1.wav、 3.wav和6.wav），然后按下“播放”按钮时，都发生了什么。PlayButton.Click首先判断列表中是否保存了音符：由于notes列表长度3＞0，列表不空，因此设定count等于1，并调用PlayBackNote：
1. 在第一次调用PlayBackNote时，count= 1： 
 * Sound1.Source被设置为在notes中的第1项，即1.wav；
 * 调用Sound1.Play，播放1.wav；
 * 由于count值（1）小于notes的长度（3），因此count递增为2，并再次调用PlayBackNote；
图9-13 回放被记录下来的音符

2.   第二次调用PlayBackNote时，count=2：  
 * Sound1.Source被设置为notes中的第2项，即3.wav；
 * 调用Sound1.Play，播放3.wav；
 * 由于count（2）小于notes的长度（3），因此count递增为3，并再次调用PlayBackNote；
3.  第三次调用PlayBackNote时，count=3：

 *  Sound1.Source被设置为notes中的第3项，即6.wav；
 * 调用Sound1.Play，播放6.wav；
 * 由于count（3）不小于notes的长度（3），因此跳出if块，回放结束。

提示：虽然递归功能强大，但运用起来存在危险。来做一个思想实验：问问自己，如果程序员忘了在PlayBackNote块中插入count递增的块，会发生什么事情。
这里的递归是正确的，但这个例子中还有另一个问题：在两次调用Sound1.Play之间几乎没有时间间隔（程序运行的速度非常快），因此每个音符都被下一个音符截断，除了最后一个。所有音符（除了最后一个）都等不到播放完，Sound1的source属性就已经被改写为下一个音符，并由Sound1.Play播放出来。为了获得正确的行为，需要在两次调用PlayBackNote之间添加延迟功能。

## 播放适当延迟的音符

延迟的设定与两个音符之间的时间差有关，我们用clock来为这个时间差计时。例如，如果时间差为3,000毫秒（3秒），则将Clock1.TimerInterval设置为3000，并启动计时器；在计时结束时再调用PlayBackNote。对PlayBackNote的if块做出修改，如图9-14所示。创建Clock1.Timer事件并编写事件处理程序，来说明计时结束时将发生的事情。


图9-14 在音符之间加入延迟
## 块的功能

现在假设两个列表中记录了以下内容：

* notes：1.wav，3.wav，6.wav
* times：12:00:00，12:00:01，12:00:04

如图9-14所示，在PlayButton.Click中设置count为1，并调用PlayBackNote。 1. 第一次调用PlayBackNote时，count= 1：

* Sound1.Source被设置为notes中的第1项，即“1.wav”；
* 调用Sound1.Play播放1.wav；
* 因为count（1）小于notes的长度（3），于是Clock1.TimerInterval被设置为times列表中的第1项（12:00:00）与第2项（12:00:01）之间的时间差：1秒。Count递增到2，启用Clock1.Timer并开始计时；

Clock1.Timer开始计时，间隔1秒之后，计时结束，定时器暂时禁用，并调用PlayBackNote。 2. 第二次调用PlayBackNote时，count= 2 ：

* Sound1.Source被设置为notes中的第2项，即“3.wav”；
* 调用Sound1.Play播放3.wav；
* 因为count（2）小于notes的长度（3），于是Clock1.TimerInterval被设置为times列表中的第2项（12:00:01）与第3项（12:00:04）之间的时间差：3秒。Count递增到3，启用Clock1.Timer并开始计时；

Clock1.Timer计时开始，间隔3秒之后，定时器暂时禁用，并调用PlayBackNote。 3. 第三次调用PlayBackNote时，count= 3 ：

* Sound1.Source被设置为notes中的第3项，即“6.wav”；
* 调用Sound1.Play来播放6.wav；
* 由于count（3）不小于notes的长度（3），跳出if块，回放完成。
## 改进

下面是一些可供探讨的备选方案：

* 目前，在回放过程中，没有对用户点击ResetButton做任何限制，这将导致程序的崩溃（错误提示：select list item: Attempt to get item number 4 of a list of lengh 0。）（你知道原因吗？）修改PlayButton.Click，让ResetButton在回放期间禁用，回放完成后再重新启用。将PlayBackNote中的if块改为ifelse块，并在“else”中重新启用ResetButton。
* 类似问题也发生在PlayButton上，用户可以在回放过程中再次点击该按钮。（想象一下会发生什么。） 在PlayButton.Click中禁用PlayButton，并将其Text属性改为“播放中…… ”，并像ResetButton一样，在PlayBackNote的ifelse块中重新启用该按钮，并重置Text属性。
* 添加一个按钮来显示一首歌曲的名字，如“致爱丽丝”。当用户单击时，向notes及times列表中填写相应的值，将count设定为1，并调用PlayBackNote。有一个非常有用的块Clock1.MakeInstantFromMillis（用毫秒设置时间间隔），可以用来设定音符之间的延迟。
* 如果用户按下一个音符，然后去做别的事情了，几小时后回来，又按下另一个音符，尽管音符可能属于同一首歌，但这绝不是用户的意图。有两种方法可以改进程序：（1）在一个合理的时间间隔后，停止记录音符，如1分钟；（2）通过对Clock1.TimerInterval使用Math抽屉中的max块，来限制音符播放的时长。
* 通过改变按钮的外观，如Text、BackgroundColor或ForegroundColor属性，来形象地提示当前正在播放的音符。
## 小结

以下是本章涵盖的概念：

* 通过修改Sound组件的Source属性，可以用一个而非八个Sound组件来播放不同音频文件。记住要在应用初始化时加载声音文件，以免运行时加载所引起的问题。（见图9-6）；
* 列表（Lists）可以为程序提供存储功能，可以在列表中保存用户的操作记录，并在以后对存储内容进行提取和再处理。我们使用这个功能来录制及播放歌曲；
* Clock组件可以用来确定当前时间，两个时间值只差为我们提供了两个事件之间的时间间隔；
* Clock组件的TimerInterval属性可以在程序中设置，就像我们设置两个音符之间的时间间隔一样；
* 编写一个能自我调用的过程不仅是可能的，有时也是必要的。这种强大的技术称为递归。在编写递归过程时，一定要确保为程序的退出设定一个基本条件，它的重要性远大于为自我调用设定条件，否则程序将陷入无限循环。
 

## 附录

### 资源下载
  
* 1.wav
* 2.wav
* 3.wav
* 4.wav
* 5.wav
* 6.wav
* 7.wav
* 8.wav
