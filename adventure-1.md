#冒险一：你好，世界

在这本书里，我们会学习如何编写程序跟Minecraft的虚拟世界进行交互，这让我们可以做很多有意思的事。我们会使用Python编程语言来学习。这种用Python程序控制Minecraft世界的方式最初是为Minecraft树莓派版开发的。如果你没有树莓派但是有Windows或者苹果电脑版的Minecraft，也没问题-只需要一些额外的设置就可以开始编程了，后面会教你怎样做。

>Python是本书使用的编程语言。

这本书通过各种各样的冒险任务来教你如何为Minecraft进行编程。书里有很多东西可以增加游戏乐趣，你可以和伙伴们一起分享。你会发现一些能够瞬间移动游戏角色的方法，并且很快会发现建造整座城市，或者是和Minecraft里从未见过的建筑物都是件很简单的事。

Python编程语言自带一个叫做IDLE的代码编辑器，我们要用它来创建、编辑和运行冒险任务里的程序。

> Python编程语言在商业和教育领域都有广泛的应用。它非常强大又很容易学习，你可以在[这里](www.python.org)了解更多关于Python语言的信息。

程序员们在学习一种新的编程语言，或者一种新的做事方法时，总是从编写一个“hello, world”程序开始。这是一个很简单的程序，只会在屏幕上显示一行“hello, world”，用来确认需要的软件都已经安装好并且可以正常工作。

在第一个冒险任务里，我们需要设置好自己的电脑，从而在Minecraft的聊天栏里显示文字“hello, minecraft world”（如图1-1）。

要进行书中的Minecraft编程，你需要以下三种电脑之一：运行微软Windows操作系统的个人电脑（PC），运行Mac OS X的苹果电脑，或者运行Raspbian的树莓派卡片电脑。不同类型的电脑安装和设置软件的方式会不同，但是只要完成设置，在任意一种电脑上进行编程都是一样的。为了让设置过程更简单，我们可以在配套网站上下载新手包。新手包经过测试，保证书中所有的冒险任务都能正常运行。新手包里包含了一个README文件，建议先阅读一下。文件描述了新手包里包含的内容，以及创建的方法。根据这些信息你可以很方便的从头开始设置自己的电脑，尽管这种方式并不推荐。按照本书中的指引进行设置可以学到更多东西。

>**提醒**
>
>正确设置电非常重要，否则你可能会陷入迷茫。所以请仔细的按照指引进行。

##设置树莓派

##设置PC或者苹果电脑

不管使用PC还是苹果电脑，首先都要安装Minecraft已经并且确保能正常运行。如果还没有Minecraft和游戏账号，请访问www.minecraft.net购买游戏。遇到任何安装、运行和玩游戏的问题，都可以访问http://help.mojang.com可以获得帮助。

> 现在我们可以免费玩到网易代理的[国服版《我的世界》](http://mc.163.com/)。

要在PC和苹果电脑上给Minecraft编程，我们还需要开放源代码的Minecraft服务器[bukkit](http://bukkit.org)，以及[RaspberryJuice插件](http://dev.bukkit.org/bukkit-plugins/raspberryjuice)。

Bukkit是一款Minecraft服务器软件，玩家可以通过创建插件改变游戏的行为；Bukkit的RaspberryJuice插件让你可以像在树莓派上一样通过编程来操控Minecraft。我们要使用bukkit、RaspberryJuice插件和Python编程语言来完成本书中的编程任务。

>**定义**
>
>Bukkit是一款可以用插件改变游戏的Minecraft服务器软件。

>**定义**
>
>插件是一个运行在Bukkit内部的程序，可以让你改变minecraft游戏。

我们需要下载Python编程语言安装在电脑上。尽管版本3才是最新的，这本书会一直使用Python版本2，这是因为mojang提供的Python函数库（用来连接Minecraft）只能在Python 2下运行。这本书里的程序都在Python 2.7.6下测试过，建议使用这个版本的Python，但这不是必须的，只需要使用Python 2.x就可以。

>想了解更多关于Python的信息，请访问www.python.org。这里提供了Python下载。Python Wiki(wiki.python.org)上有很多信息、教程和Python社区的网站链接。

设置Windows PC和苹果电脑来进行Minecraft编程需要三个步骤：

1. 下载并解压新手包，里面有配置好的Bukkit服务器和RaspberryJuice插件，还有一个叫做MyAdevntures的文件夹，我们以后要把自己编写的程序保存在这个文件夹里。
2. 下载和安装Python编程语言。
3. 设置Minecraft以使用和Bukkit相同的版本并连接到Bukkit服务器。

>在本书配套网站（www.wiley.com/go/adventuresinminecraft）可以观看设置windows pc和苹果电脑的视频教程。

###在windows pc上安装新手包和python

如果使用windows pc，下一步要做的事情是：

1. 下载windows pc的新手包，并解压到桌面。
2. 下载python编程语言并安装到windows pc。

下载和解压新手包

按照下面步骤下载windows pc新手包并把其内容拷贝到桌面。把这些文件放在桌面方便以后寻找。

1. 打开电脑的网页浏览器（例如ie, chrome），访问配套网站，下载Windows新手包。
2. 下载完成后，打开文件。你可以通过“打开文件”选项或者在“下载”目录下双击AIMStarterKitPC.zip文件来完成。
3. zip压缩文件里有一个文件夹，叫做AdventuresInMinecraft。点击这个文件夹，按住鼠标按键不放，然后拖动到桌面，这样就可以把它拷贝到桌面。

下载和安装python

按下面步骤安装python和idle编辑器：

>如果你没有电脑的管理员账号，你需要先找到有管理员账号的人来输入密码，然后才能安装python。

1. 打开电脑的浏览器，访问www.python.org/download/releases/2.7.6，点击Windows x86 MSI Installer(2.7.6)链接。
2. 下载完成后，运行得到的python-2.7.6.msi文件，通过菜单“打开=》运行”，或者在下载目录双击文件。
3. 如果出现一个安全警告窗口，询问“你是否要运行该文件？”，点击“运行”。
4. 点击“Next”(下一步)进行安装。
5. 安装程序会让你选择一个安装位置。使用默认位置是最好的选择。点击next。记下你安装的位置，这样需要的时候你就能找到这些文件。
6. 不要修改customize python窗口的任何选项，直接点击next。
7. 如果user account control询问是否允许运行安装程序，点击yes。
8. 等待安装程序完成安装，然后点击finish。

###在苹果电脑上安装新手包和python

###在PC或苹果电脑上运行minecraft

>这里的指引将告诉你在以后的冒险任务中如何启动minecraft。如果你需要一些关于如何启动minecraft和连接到bukkit服务器的提示，只需要参考这里。

你已经安装好了所有需要的软件。但是还缺一点准备。你需要按下面步骤启动bukkit服务器并将minecraft连接到服务器：

1. 首先双击打开桌面上的aim文件夹。
2. 双击运行StartBukkit程序。这将会启动bukkit命令窗口。
3. 按任意键启动bukkit服务器。启动过程中，屏幕上会出现各种信息让你了解启动的进度。
4. bukkit启动完成后，会显示“Done”（图1-6）。

在本书的所有冒险中，minecraft和bukkit的版本都是1.6.4。这表示你需要修改minecraft启动器来运行1.6.4版本的游戏。用这个版本，而不是最新版本，本书中的代码和程序保证可以运行。你只需要设置一次，因为minecraft会记住你使用的版本。

>如果你想要使用新版本的minecraft，新手包里的README文件描述了如何创建自己的新手包。只有熟悉电脑设置、有编辑配置文件和运行java程序经验的高级用户才可以尝试这些。

按照下面步骤配置minecraft启动程序以使用1.6.4：

1. 启动minecraft。
2. 出现minecraft启动程序窗口之后，点击窗口底部左侧，用户名旁边的“edit profile”按钮。
3. 在use version下拉列表中，选择release 1.6.4，如图1-7所示。
4. 点击save profile。

现在，可以将minecraft连接到bukkit服务器了：

1. 点击minecraft启动程序中的play启动游戏。
2. 从minecraft主菜单，点击multiplayer。
3. 在play multiplayer菜单中点击direct connect。
4. 在Server address中输入localhost并点击jion server（如图1-8）。好了！你现在应该已经进入bukkit服务器上创建的minecraft世界了。你已经到达了。

###停止bukkit

当你完成了一天的冒险任务，你需要断开和bukkit服务器的连接。在minecraft中按esc键打开主菜单，然后点击disconnect就可以了。你现在可以在bukkit命令窗口中输入stop和回车，安全的关闭bukkit服务器。

你会在屏幕上看到一些新信息，告诉你在关闭的过程中发生了什么。在出现“closing all listening threads”之后，你会看到带有“SEVERE”的信息。别担心，这是这场的，不会影响你的电脑和minecraft程序。

>你可以在bukkit命令窗口中输入命令来控制minecraft游戏的很多方面，比如时间和天气。举个例子，如果你的minecraft世界里天色渐晚，而你还没有准备好应对黑夜，那就在命令窗口中输入set time 1来重新设置时间为早上。如果开始下雨，输入weather clear就可以让天气放晴。输入help可以获得一份完整的bukkit命令列表。

##创建程序

恭喜！你已经设置好你的电脑，并且minecraft也在运行了。你可能会觉得设置过程有一点枯燥，不过到这里已经完成了，只有在更换电脑之后才需要重新设置一次。现在该做些有趣的事了——编写你的第一个程序，“hello, minecraft world”。

>在以后的冒险中，指引只会告诉你启动idle。如果需要如何启动idle的提示，你可以参考本节的内容。

首先，你需要按照下面方法启动python并打开idle：

* 在树莓派上：双击桌面上的idle（或idle3）图标。
* 在pc上：点击 开始->所有程序->python 2.7->idle(python gui)。
* 在windows 8上：点击开始，点击idle(python gui)，或者通过搜索找到idle。
* 在苹果电脑上：在finder菜单栏点击go->applications->python2.7。双击idle。

现在，python shell窗口应该出现了。

>如果使用树莓派，python shell窗口可能要在点击图标之后几秒钟才出现。

>idle首次运行时，电脑可能会请求你允许运行程序，或者允许idle连接网络。这是正常的，点击同意就可以了。

python shell窗口启动之后，就可以在idle中创建新程序了。接下来你要创建的程序并不会有什么酷炫的效果。就像计算机程序员总是从编写“hello world”程序开始，你现在也要创建一个hello minecraft world程序来检查电脑上的软件是否都正确安装了。像下面这样做：

>以后的冒险任务会让你创建一个新程序并保存到MyAdventures文件夹。如果需要如何创建程序的提示，请参考这一节内容。

1. 在idle菜单中点击file->new file来创建新文件。（在树莓派上，new file可能叫做new window。）
2. 在菜单中点击file->save保存文件到myadventures文件夹。
3. 选择myadventures文件夹：
  * 在树莓派上：
  * 在pc上：
  * 在苹果电脑上：
4. 你需要给新文件起一个名字。输入文件名hellominecraftworld.py然后点击save。文件末尾的.py告诉电脑这个文件是一个python程序。
5. 现在该开始编程了！在idle编辑窗口中输入下面的代码来创建hello minecraft world程序。确保大小写都是正确的，因为python是大小写敏感的。
6. 在idle菜单中选择file->save保存程序。

import mcpi.minecraft as minecraft
mc = minecraft.Minecraft.create()
mc.postToChat("Hello Minecraft World")

>把程序保存在myadventures文件夹很重要！本书中所有的程序都应该保存在这里，因为这里有运行程序所需要的所有东西。

>python是一个大小写敏感的编程语言，也就是说你必须正确输入每一个字符的大小写。例如，python会认为Minecraft（大写）和minecraft（小写）是不同的。如果输入不正确会造成运行错误，那时你就不得不检查每一个步骤来寻找出错的地方。

>在下个冒险任务中你会了解到这些代码的含义和它们的功能。现在你只需要按照指示操作，让屏幕上出现hello minecraft world字样，确认所有软件都正常就好了。

##运行程序

>后面的冒险任务会告诉你运行一个程序，如果需要提示，请参考本节。

你已经成功的编写了一个程序！现在该来试试它能不能运行了。你需要按先免步骤让idle运行你的程序：

1. 在运行程序之前，minecraft必须已经打开并且在游戏中。如果没有，请按照前面的学到的方法启动minecraft。
2. 调整minecraft窗口的大小，让你可以同时看见idle窗口和minecraft。（如果使用pc或者苹果电脑，minecraft以全屏模式运行，按f11退出全屏模式，这样才能看到minecraft窗口。）这时候你应该看到下面几个窗口：
  * 在树莓派上：你应该有三个打开的窗口：python shell；idle代码编辑器，并打开了你的hellominecraft.py程序；以及minecraft。（图1-9）
  * 在PC上：你应该有四个打开的窗口：python shell；idle代码编辑器，并打开了你的hellominecraft.py程序；bukkit命令窗口；以及minecraft。（图1-10）
  * 在苹果电脑上：你应该有四个打开的窗口：python shell；idle代码编辑器，并打开了你的hellominecraft.py程序；bukkit命令窗口；以及minecraft。（图1-11）
3. 按esc打开minecraft菜单。用鼠标指针选择idle窗口。用鼠标指针选择idle代码编辑器。
4. 在idle菜单中点击“运行->运行模块”或者按F5键运行hellominecraftworld.py程序。
5. idle会自动切换到python shell窗口并运行程序。如果有错误，你会在这里看到红色的错误信息。如果有任何错误，请回到idle代码编辑器窗口仔细检查你输入的代码，并跟上面的代码进行比较来找到出错的地方。
6. 在回到minecraft窗口。发现什么了吗？你的辛勤工作已经产生了成效，“hello minecraft world”已经出现在了聊天栏。（看起来应该像图1-1那样。）

>如果之前的设置都正确，“hello minecraft world”应该出现在了聊天栏。（如图1-1）；如果没有，或者python shell窗口中出现了错误，在回顾一下本任务的指引。初始设置正确是很重要的。否则以后冒险任务里编写的程序都将无法运行。

##停止程序

>以后的冒险任务只会告诉你停止程序。如果需要如何停止程序的提示，请参考本节内容。

hello minecraft world程序运行之后，在屏幕上显示一行消息然后停止。不过有时候知道怎么强制一个程序停止是很重要的，因为，在以后的冒险任务中，你编写的程序不会停止运行，除非你让它们停止。要停止程序，选择python shell窗口然后点击菜单中的shell->restart shell，或者按住ctrl键，再按c键。

**解锁成就**：最困难的部分已经结束了！你已经启动并运行了minecraft，也编写了你的第一个程序，世界正在想你问好。

>在冒险任务2中，你会学到一些minecraft和python的简单编程技巧，它们让你能追踪游戏角色的位置。你会学到“游戏中的游戏”这个概念，你要编写一些简单的游戏，游戏行为会根据角色在minecraft世界里的位置而改变。