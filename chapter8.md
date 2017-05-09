第8章 总统测验
“总统测验”是一个关于美国前总统的问答游戏。虽然测验的内容与总统有关，但你可以把它当作模板，来实现对任何题目的测验。

在前几章中，你已经了解了一些编程的基本概念。现在，准备好面对更大的挑战吧。你会发现，无论是编程技巧，还是抽象思维，这一章都要求你有一个概念性的飞跃。特别需要强调的是，本章将使用两个列表变量来存储数据——应用中的问题和答案，使用索引变量来跟踪用户正在回答的题目。在本章结束时，对于创建测验类应用和其他需要使用列表的应用，你已经掌握了必要的知识。


本章假设你已经熟悉了App Inventor的基础知识：使用组件设计器构建用户界面，用块编辑器来定义事件处理程序并为组件添加行为。如果你还不熟悉，在继续学习之前，请复习前面几章。

在测验中，用户通过单击“下一题”按钮，连续地回答问题，并收到回答是否正确的反馈。

学习要点


图8-1 “总统测验”在手机中
如图8-1所示，本章覆盖以下内容：

定义列表变量：用来存储问题和答案；
使用索引遍历列表，用户每次点击“下一题”按钮时，显示下一个问题；
使用条件语句（if）控制行为：只有在特定条件下才能执行某些操作。在用户测验到最后一题时，将使用if块来处理程序；
每一道题对应一张不同的图片，要实现图片的切换。
准备开始

登陆App Inventor网站，创建新项目“PresidentsQuiz”，并设置屏幕的标题为“总统测验”，连接测试设备。从appinventor网站下载测验中用到的图片：roosChurch.gif，nixon.gif，carterChina.gif和atomic.gif。在下一节中将这些图片加载到项目中。

设计组件

“总统测验”应用的界面很简单：显示问题并允许用户来回答。图8-2显示了应用在组件设计器中的截图，按图来创建组件。


图8-2 组件设计器中的“总统测验”
首先将下载的图片加载到项目中：单击Media区域的Upload File按钮，选择一个文件（如roosChurch.gif），其他图片也是如此。然后添加表8-1中列出的组件。

表8-1 “总统测验”应用所需组件
组件类型	面板中分组	命名	作用
Image	User Interface	Image1	与问题一同显示的图片
Label	User Interface	QuestionLabel	显示正在回答的问题
HorizontalArrangement	Layout	HorizontalArrangement1	放置答案输入框及“提交”按钮
TextBox	User Interface	AnswerText	用户在此输入答案
Button	User Interface	AnswerButton	用户点击之后提交答案
Label	User Interface	RightWrongLabel	显示“正确”或“不正确”的反馈
Button	User Interface	NextButton	用户点击进入下一题
按照下面提示设置组件属性：

Image1：Picture为roosChurch.gif（最先出现）；Width为“Fill parent”，Height为200；
QuestionLabel：Text为“问题…”（在块编辑器中输入第一个问题）；
AnswerText：Hint为“输入回答”，Text为空，放置到HorizontalArrangement1中；
AnswerButton：Text为“提交”，放置到HorizontalArrangement1中；
NextButton：Text为“下一步”；
RightWrongLabel：Text为空。
为组件添加行为

编程来实现以下行为：

应用启动时，显示第一个问题以及相应的图片；
点击“下一题”按钮时，显示第二题，再次点击，显示第三题，以此类推；
当显示最后一题时，点击“下一题”按钮将回到第一题；
在用户回答问题之后，反馈回答是否正确；
首先按照表8-2的提示，定义两个列表变量：QuestionList用来保存问题，AnswerList用来保存答案。图8-3显示在块编辑器中创建的两个列表。

表8-2 用于保存问题和答案的列表变量
块的类型	所在抽屉	作用
Initialize global QuestionList to	Variables	保存问题的列表（更名为QuestionList）
Initialize global AnswerList to	Variables	保存答案的列表（更名为AnswerList）
make a list	Lists	为QuestionList插入列表项
问题内容（三个）	Text	问题
make a list	Lists	为AnswerList插入列表项
答案内容（三个）	Text	答案

图8-3 问题及答案列表
定义索引变量

在整个测试过程中，每次用户点击“下一题”按钮，都要跟踪用户正在回答的问题。定义变量currentQuestionIndex作为QuestionList和AnswerList的索引值。表8-3列出了所需的块，图8-4显示了变量的定义。

表8-3 创建索引
块的类型	所在抽屉	作用
Initialize global currentQuestionIndex to	Variables	保存当前问题（与答案）的索引（位置）
数字1	Math	将currentQuestionIndex的初始值设为1（第一题）

图8-4 索引变量的初始值为1
显示第一个问题

有了这些变量，就可以为应用设定交互行为。无论是何种应用，渐进式的开发是非常重要的，而且每一步只定义一个行为。我们首先考虑与问题相关的行为，具体而言，在应用启动时显示列表中的第一道题，稍后再来处理图片的事情。

代码块的设定应该与列表中的具体问题无关，这样，如果需要更换问题或创建新的测验类应用时，只需改变列表中的具体问题，而不必修改事件处理程序。

鉴于上述考虑，对于第一道题，不要直接引用“哪位总统在大萧条时期实施了‘新政’？”这样的题目内容，而是引用“QuestionList的第一个插槽”这样抽象的形式（与具体问题无关）。这样，即使第一个插槽中的问题改变了，这些程序块仍然有效。

select list item块用来选择列表中的项，使用中要求指定list（列表）及index（索引）（列表中的位置）。如果列表中有三个项，可以输入1、2或3作为索引。

第一个行为是，在应用启动时选择QuestionList中的第一道题，将其写入QuestionLabel；还记得“Android，我的车在哪儿？”的应用吧，如果想让某件事发生在应用启动时，可以将有关指令放在Screen1.Initialize事件处理程序中，表8-4中列出所需的块。

表8-4 应用启动时加载第一个问题所需的块
块的类型	所在抽屉	作用
Screen1.Initialize	Screen1	应用启动时触发该事件
set QuestionLabel.Text to	QuestionLabel	将第一道题内容写入QuestionLabel
select list Item	Lists	从QuestionList中选择第一道题
get Global QuestionList	Variables	从其中选择问题的列表
数字1	Math	用索引值1来选择第一道题
块的作用
应用启动时触发Screen1.Initialize事件。如图8-5所示，变量QuestionList中的第一项被选中，并被写入QuestionLabel.Text。因此，应用启动时，用户会看到第一道题。


图8-5 应用启动时选择并显示第一道题

测试：连接装有AI伴侣的设备，或点击“connectEmulator”打开Android模拟器。当应用启动后，你是否看到QuestionList中的第一道题：“哪位总统在大萧条时期实施了'新政'？”

遍历所有问题

现在为“下一题”按钮的行为编程。之前定义的currentQuestionIndex用来记住用户正在回答的问题，现在设定当用户单击“下一题”时，为currentQuestionIndex加1（即，从1变为2，或从2变为3，依此类推），并根据currentQuestionIndex的值来选择并显示新的问题。挑战一下你自己，看看是否可以自己搭建这些块。完成之后，与图8-6进行对照。


图8-6 显示下一题
块的作用
第一行的块让变量currentQuestionIndex递增。如果当前值为1则加到2；如果是2则加到3，以此类推。一旦currentQuestionIndex值改变，应用将以此来选择新的问题并显示。首次单击“下一题”时，currentQuestionIndex从1变为2，应用将选择并显示QuestionList中的第二道题：“哪位总统在1979年实现中美建交？”；第二次单击“下一题”时，currentQuestionIndex从2变为3，应用将选择并显示QuestionList中的第三道题：“哪位总统因水门事件而辞职？”


提示：花一分钟的时间来比较一下NextButton.Click与Screen.Initialize两个事件处理程序的差别。在Screen.Initialize中，用具体数字1来选择列表项；而在NextButton.Click中，用索引变量currentQuestionindex来选择列表项，即选择第currentQuestionindex项，而非第一或第二第三项，因而点击“下一题”将选中不同的项。这是索引最常见的用法——增加索引值来找到并显示列表项。

问题是，索引的每次递增，都会转到下一题，那么当测验到最后一题时，怎么办呢？即：当currentQuestionIndex=3时点击“下一题”，currentQuestionIndex将从3变为4，应用将从问题列表中选择第currentQuestionIndex项，即第4项，而列表QuestionList中只有3项，此时Android设备将不知所措并强行退出应用。那么应用如何知道已经测验到最后一题了呢？


测试：测试“下一题”按钮，看看应用运行是否正常。在手机上按“下一题”按钮，是否显示第二题“哪位总统在1979年实现中美建交？”？应该是的；再按“下一题”，应该出现第三题。但如果再次点击，就会看到错误提示：“Attempting to get item 4 of a list of length 3.（试图从只有3个项的列表中获取第4项。）”这就是程序的bug！知道原因吗？在继续阅读之前试试看自己解决它。

当点击“下一题”按钮时，应用要问一个问题，并根据问题的答案执行不同的操作。既然已知QuestionList中包含三个问题，问题可以这样来问：“currentQuestionIndex是否＞3？”如果是，将currentQuestionIndex设回1，这样就回到了第一道题。表8-5中列出了所需的块。

表8-5 检查索引值是否到了列表的结尾所需的块
块的类型	所在抽屉	作用
if	Control	判断用户是否正在做最后一题
=	Math	检查currentQuestionIndex的值是否为3
get global currentQuestionIndex	Variables	放入“=”左边的插槽
数字3	Math	放入“=”右边的插槽
set global currentQuestionIndex to	Variables	设为1来转回到第一道题
数字1	Math	设置索引值为1

测试：单击手机上的“下一题”按钮，会照常出现第二题“哪位总统在1979年实现中美建交？”，继续点击“下一题”，将显示第三题。下面是你真正想测的：如果再次点击，将出现第一题（“哪位总统在大萧条时期实施了‘新政’？”）。


图8-7 检查索引值递增
单击“下一题”时，索引照旧会递增。但程序会检查是否currentQuestionIndex＞3（问题的数量）。如果大于3，则将currentQuestionIndex重新设置为1，并显示第一题；如果≤3，则不执行if块内的程序，并照常显示当前问题。


图8-8 检查测验是否到了最后一题（第三题）
让测验易于修改

如果NextButton.Click中的块能够正常运行，恭喜你，你正在成为一名合格的程序员！但是，如果想在测验中添加新题目（及答案），该怎么办？这些块还能正常运行吗？为了验证这一点，先在QuestionList中添加第四道题，并在AnswerList中添加第四个答案，如图8-9。


图8-9 向两个列表中分别添加一项

测试：多次单击“下一题”按钮，你发现无论点击多少次，第四题始终不出现。知道问题所在吗？在继续阅读之前，尝试做些修改，以便让第四题出现。

问题出在“最后一题”的判断条件太具体：currentQuestionIndex＞3。如果把3改为4，程序正常了，但问题是，每次增减问题和答案时，都要记着修改判断条件。计算机程序中的这种强相关性最容易导致错误，特别是当程序变得复杂时。好的对策是让程序的设计与列表中的问题数量无关。这种通用性，对于程序员来说，当你想创建其他专题的定制测验时，可以让程序的移植更加容易。尤其是在处理动态列表时，这样做是必须的，例如，测验中允许用户添加新问题（见第10章）。一个通用性好的程序不该与3这样的具体数字相关联，因为这只对那些有三个问题的测验有效。对currentQuestionIndex的判断条件应该是QuestionList列表的长度（项数），而不是具体数字。当条件更具通用性时，即使是添加或删除QuestionList中的项，程序也能正常运行。现在修改NextButton.Click事件处理程序，替换掉具体数字3。表8-6中列出了所需要的块。

表8-6 检查列表长度所需的块
块的类型	所在抽屉	作用
length of list	Lists	询问列表QuestionList中有多少个列表项
get global QuestionList	Variables	插入length of list块的list插槽中
块的作用
If块中将currentQuestionIndex值与QuestionList的列表长度进行比较，如图8-12所示。如果currentQuestionIndex为5，而QuestionList的长度为4，则currentQuestionIndex将被重新设置为1。值得注意的是：由于程序块不再与3或任何具体数字相关联，因此无论列表中有多少项，程序都将正常运行。


图8-10 采取更加通用的方式检查列表的结尾

测试：当单击“下一题”按钮时，程序是否在四个问题间循环？在第四题后是否又回到第一题？

为每道题切换图片

现在程序已经可以遍历所有的问题（而且代码更加聪明灵活，也更抽象），下面来设置图片。眼下无论显示什么问题，图片都是同一个，我们希望当用户单击“下一题”时，图片与问题相匹配。此前在Media中载入了四张图片，现在用图片的文件名来创建第三个列表PictureList。然后修改NextButton.Click事件处理程序，同时切换问题与图片。（想到currentQuestionIndex就说明你已经开窍了！）首先创建列表PictureList，用图片文件名初始化列表，要保证列表中的文件名与先前加载的图片文件名完全相同。图8-11显示了PictureList块的样子。


图8-11 PictureList中用图片文件名来充当列表项
下面来修改NextButton.Click事件处理程序，以便图片可以随问题索引的改变而改变。Image组件的Picture属性用于指定要显示的图片。表8-7中列出了修改NextButton.Click所需的块。

表8-7 显示与问题相匹配的图片所需的块
块的类型	所在抽屉	作用
set Image1.Picture to	Image1	改变图片
select list Item	Lists	选择一个与当前问题相匹配的图片
global PictureList	Variables	从列表中选择一个文件名
get global currentQuestionIndex	Variables	选择第currentQuestionIndex项
块的作用
rrentQuestionIndex同时充当QuestionList和PictureList两个列表的索引，这要求正确设置各个列表，如，第一题对应第一个答案及第一张图，第二题对应第二个答案及第二张图，依此类推，这样一个索引值可用于三个列表，如图8-12所示。举例说明：第一张图roosChurch.gif是罗斯福总统的图（与英国首相丘吉尔在一起），而“罗斯福”是第一个问题的答案。

图8-12 每次选择与问题匹配的第currentQuestionIndex张图片

测试：多次点击“下一题”，每次点击是否出现不同的图片？

检查用户答案

现在应用已经可以遍历所有的试题及答案（及匹配答案的图片），这是列表应用的极好案例。但真实的测验要对用户的回答判断正误。下面添加一些块来告诉用户他的回答是否正确。用户在AnswerText中输入答案，并点击AnswerButton提交答案；程序用Ifelse块将用户输入与标准答案作比较，并用RightWrongLabel显示比较结果。表8-8列出了程序中用到的块。

表8-8 用于显示答案是否正确的块
块的类型	所在抽屉	作用
AnswerButton.Click	AnswerButton	点击AnswerButton按钮时触发该事件
ifelse	Control	如果回答正确，做一件事，否则做另一件事
=	Math	判断回答是否正确
AnswerText.Text	AnswerText	包含了用户的回答
select list Item	Lists	从AnswerList列表中选择当前问题的答案
get global AnswerList	Variables	答案的列表
get global currantQuestionIndex	Variables	当前用户正在回答的问题的索引值
set RightWrongLabel.Text to	RightWrongLabel	显示回答是否正确
“正确”	Text	回答正确时显示
set RightWrongLabel.Text to	RightWrongLabel	显示回答是否正确
“不正确”	Text	回答错误时显示
块的作用
在图8-14中，Ifelse块用来检验用户的输入（AnswerText.Text）是否等于AnswerList中的第currentQuestionIndex项。如果currentQuestionIndex=1，程序将用户的回答与AnswerList中的第一项“罗斯福”作对比，同样，如果currentQuestionIndex=2，则与AnswerList中的第二项“卡特”作对比，等等。如果对比结果相同，则执行then块，即RightWrongLabel显示“正确！”；如果对比结果不同，执行else块，即RightWrongLabel显示“不正确！”。


图8-13 检查用户的回答，并告诉用户答案是否正确

测试：尝试回答一道题，程序会显示你的回答是否正确。分别试验正确和错误的回答。你会注意到，回答正确，意味着你的输入必须与AnswerList中的答案完全匹配（包括大小写、标点或空格）。继续测试后面的问题，并确认运行正常。


图8-14 用户界面上的小问题
应用运行正常，但你会看到，当单击“下一题”时，虽然图片和问题都切换到下一题，但“正确！”或“不正确！”的文本以及前一题中输入的回答仍然显示在屏幕上，如图8-14所示。尽管这一点无伤大雅，但用户肯定会发现这类的界面问题。将RightWrongLabel及AnswerText清空，需要在NextButton.Click事件处理程序中添加几个块，表8-9列出了所需的块。

表8-9 清除RightWrongLabel及AnswerText的块
块的类型	所在抽屉	作用
set RightWrongLabel.Text to	RightWrongLabel	需要清空内容的label
“”	Text	当用户点击“下一题”时，删除对上一题回答的反馈
set AnswerText.Text to	AnswerText	用户对上一题的回答
“”	Text	当用户点击“下一题”时，删除对上一题的回答
块的作用
用户单击“下一题”时，图8-15中的前两行用于清空RightWrongLabel和AnswerText。


图8-15 当转入下一题时，清空上一题的答案及对答案的反馈

测试：回答一个问题，然后点击“提交”，再单击Next按钮，上一题的答案及反馈是否消失了？

完整的应用：总统知识测验

图8-16与8-17显示了“总统测验”应用中块的最终配置。


图8-16 “总统测验”应用中块的最终配置（之一）

图8-17 “总统测验”应用中块的最终配置（之二）
改进

一旦测验应用开始正常运行，你也许会乐于做一些改进，例如：

现在应用中只显示与问题有关的图片，也可以尝试播放录音或视频片段。在使用声音上，你甚至可以发展出一款“辩声识曲（Name That Tune）”的应用；
本测验对正确答案的要求过于严格，有几种改进方法：一是使用text.contains块，来检查是用户的输入中是否包含了真正的答案；另一种方法是给每道题提供多个答案，通过遍历（foreach）来检查是否与标准答案相匹配；你还可以想办法处理掉那些用户输入的多余空格，或者不做大小写区分，等等；
将测验改为多选题，这需要用另一个列表来保存每个问题的可选答案。答案也可能是一个二级列表，第二级列表中保存着特定问题的可选答案。使用ListPicker组件，让用户来选择答案。更多关于lists的内容请参见第19章。
小结

下面是本教程中所涉及到的概念：

将应用程序划分为数据（通常保存在列表中）及事件处理程序两个部分；使用Ifelse块来做条件判断，有关条件语句的更多信息，请参见第18章；
在事件处理程序中，程序块只能引用抽象的名称来指代列表项及列表长度，以便当列表数据发生变化时，程序还可以正常运行；
索引变量可以跟踪当前项在列表中的位置，当索引递增时，要小心列表的末尾，使用if块来处理应用中的行为。
