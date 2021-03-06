#冒险六：数据文件

只有在学会了处理大量数据之后，计算机编程才会变得真正有趣起来。程序就变成了一系列决定如何读入、处理和展示数据的规则——数据才是最重要的部分。

在这个冒险任务里，我们首先来学习如何创建描述迷宫的文本文件。然后会自动在Minecraft世界中建造这些迷宫让你和伙伴们去走。你可能会惊讶一小段Python程序竟然能建造如此庞大的结构。

我们之后会用这个简单的想法来开发一个叫做复制室的虚拟3D扫描和打印设备。在里面建造的任何东西都会被保存在文件里方便以后重建，并传送到Minecraft世界里的任何地方，甚至是在其他电脑上的Minecraft里！你将能够建立起自己的物体集，可以飞快的建造其中的物体，你的小伙伴们都会想要自己的复制机。

##从文件中读入数据

计算机程序一般来说很不智能——它们只能重复执行我们写的指令。如果不改变某些输入，那么每次运行程序都会做完全一样的事情。我们已经编写过的那些程序其实是交互性的，因为它们会根据游戏世界中发生的情况（也就是程序的输入）改变自己的行为。

###数据文件可以做的趣事

要让计算机程序在每次运行的时候做不同的事情，一种有趣的方式是把输入数据存储在文本文件里，让程序每次启动的时候去读取这个文件。然后我们就可以创建任意多的文本文件，甚至而可以加上一个菜单用来根据想做的事情选择不同的文本文件。这叫做“数据驱动”的程序，因为程序做什么主要是由外部的文本文件决定的。

自习思考一下，其实我们用过的大部分计算机程序都会使用数据文件。用来写作业的文字处理软件把我们的作业存储在数据文件里；用数码相机拍的照片也存储在文件里；图像编辑程序从这些文件里读取照片。Minecraft精彩的游戏世界背后也是使用数据文件来实现保存和加载等功能的，用来建造世界的各种方块的纹理包也是存放在文件里。在程序中使用数据文件是一种很灵活的方式，因为这样程序就可以做很多事情，不需要你每次要做不同的事情都去修改程序。我们也可以跟好友分享数据文件，只要他们也有同样的程序。

学习编写数据驱动程序程序的第一步是学习如何使用Python从计算机文件系统里打开和读取文本文件。

###编写一个提示器

我们通过编写一个简单的提示器程序来学习如何打开和读取文本文件。这个程序从事先准备好的文件里读取游戏提示，然后以随机的时间间隔在聊天栏显示一条提示。我们需要一个包含了提示信息的文本文件，所以第一个任务就是创建这个文件。我们可以在Python编辑器里创建这个文件，就像创建Python程序一样。

启动Minecraft，IDLE，如果使用PC或者Mac，还要启动服务器。到这里，你应该已经练习过启动这些程序。如果需要提示，请参考冒险任务1。

1. 在IDLE里，从菜单中选择File->New File新建一个文件。保存文件为tips.txt。
2. 列出4到5个游戏提示，每个占用一行。这里有一些例子，不过自己写的话会更有意思。在每一行的结尾都要按一下回车键，这样就会给每一行的后面加上换行符。（在这个冒险任务的后面，你会了解到更多关于换行符的知识。）
3. 保存文件。我们不能运行这个文件，因为它不是一个Python程序，而是一个Python程序会用到的文本文件。

> 定义
>
> **换行符**是计算机用来标记一行文本结尾的一个特殊的不可见字符。因为过去机械式打字机的原因，换行符有时候也叫做“回车”。当你想要在下一行打字的时候，就需要用到回车，这会让游标回到纸张的左边然后往下移动一行。

> 注意
>
> 不要在tips.txt文件里，或者任何用`mc.postToChat()`发送的消息里用分号。Minecraft API里有个小Bug。消息文本里如果有分号就会在分号处被截断。

现在我们有了一个包含游戏提示的文件——换句话说，就是输入数据——是时候写程序来处理输入数据了。这个程序会从文件里随机读取一条提示，等待一段随机时间之后把提示显示在Minecraft聊天栏里。这样在玩游戏的时候，聊天栏里就会弹出一些有用的提示。

1. 在IDLE菜单栏选择File->New File创建一个新文件，保存为tipChat.py。
2. 导入需要的模块：

    ```
    import mcpi.minecraft as minecraft
    import time
    import random
    ```

3. 连接到Minecraft游戏：

    ```
    mc = minecraft.Minecraft.create()
    ```

4. 设置一个常量表示文件名，这样以后如果要读取另外一个文件就很容易修改：

    ```
    FILENAME = "tips.txt"
    ```

5. 以读方式打开文件（更完整的解释请阅读后面的“深入代码”）：

    ```
    f = open(FILENAME, "r")
    ```

6. 把文件的所有行都读入到一个叫做tips的列表里。我们在冒险任务4和5里已经用过列表了：

    ```
    tips = f.readlines()
    ```

7. 读完之后关闭文件。处理完文件之后关闭是个好习惯，因为文件在已经打开的情况下是不能被其他程序（比如我们之前用来输入提示文本的编辑器）使用的：

    ```
    f.close()
    ```

8. 编写游戏主循环，每次等待3秒到7秒之间的一个随机时间：

    ```
    while True:
        time.sleep(random.randint(3,7))
    ```

9. 从列表tips里选择一个随机的消息显示到聊天栏。我们需要使用`strip()`函数去除掉不需要的换行符。我们后面会更进一步的解释换行符，所以现在不理解没关系：

    ```
        msg = random.choice(tips)
        mc.postToChat(msg.strip())
    ```

保存并运行程序。发生了什么？在玩家畅游Minecraft世界的时候，每过一段时间聊天栏里都会弹出一条消息，就像图6-1一样。注意Miecraft聊天栏是怎么在屏幕上展示这些消息的，它们会随着时间逐渐消失。

> 挑战
>
> 根据你自己玩Minecraft的经验在tips.txt里添加提示信息。把tips.txt和tipChat.py程序给一个刚开始玩Minecraft的朋友，让他用这个工具来学习如何在游戏世界里做有趣的事情。你甚至可以写一系列关于不同主题的、针对不同水平玩家详尽程度不同的tips.txt文件，然后发布到一个小网站上，让其他人来使用你的tipChat.py程序。

------

<u>深入代码</u>

在tipChat.py程序里，下面这行代码需要解释一下：

```
f = open(FILENAME, "r")
```

函数`open()`打开一个文件，文件名保存在常量`FILENAME`里——但是后面的"r"是什么意思？打开文件时，我们要告诉`open()`函数需要怎样访问文件。在这里，"r"表示只需要读取文件。如果我们不小心尝试去写这个文件，就会得到一个错误消息。这可以防止文件被意外破坏。如果想要打开文件并向里面写数据，可以改用"w"（如果想同时读写一个文件，用"rw"）。

其次，`f`是什么？`f`只是另外一个变量，不过里面保存了一个文件句柄。文件句柄是用来“控制”文件的——是对计算机文件系统里文件的一种虚拟的引用。需要操作一个文件的时候，就用`f`来代表它。这是很方便的，因为跟我们用过的其他变量一样，我们可以像下面这样用多个文件来同时打开多个文件：

```
f1 = open("config.txt", "r")
f2 = open("levels.txt", "r")
f3 = open("score.txt", "rw")
```

在后面的程序里就可以使用变量`f1`、`f2`、`f3`来表明想要读写哪个文件。

------

##用数据文件建造迷宫

既然已经知道怎么读取数据文件了，我们接下来要更大胆一点。在之前的冒险任务里，我们已经知道在Minecraft世界里放置方块是很容易的。为什么不直接用数据文件里存储的方块列表来建造呢？这就是我们接下来要做的事情：在Minecraft里用存储在外部文件的数据建造一个完整的3D迷宫！但是，我们首先需要决定迷宫数据在文件里如何表示。

我们设计的迷宫是3D的，因为他们建造在3D的Minecraft世界里，但实际上他们只是用3D方块建造的2D迷宫。这表示数据文件需要记录迷宫里每个方块的x和z坐标，所以迷宫会是长方形的。

我们还需要决定方块本身在数据文件里怎样表示。我们需要使用墙和方块。为了简单，可以用1表示墙，0表示空气。我们稍后可以修改Python程序来改变用来当作墙的方块类型。

###理解CSV文件

在这个程序里，我们要使用一种叫做CSV文件的特殊文本文件，以便在文件里存储迷宫数据。

> 定义
>
> CSV的全称是“逗号分隔的数值”（comma-separated values）。数值被存储在普通的文本文件里，并用逗号进行分隔。CSV文件可以用来表示一个简单的表格或者数据库。表格中的每一行，或者数据库中的每一条记录用文件中的一行来表示，行内以逗号分隔的一段文本表示一列或者一个字段。某些CSV文件用第一行来表示表格的标题或者数据库的字段名。所有流行的电子表格软件和数据库软件都支持用CSV格式导入或导出数据。
>
> CSV是一种很简单但是应用广泛的格式。这里有一个例子，文件里存储了某个Minecraft玩家数据库里一张表格的部分内容。在这个CSV文件里，第一行是字段名，其他行都是用逗号分隔的数据：
>
> ```
> Name,Handle,Speciality
> David,w_geek,Coding in Python
> Roma,physics_gurl,Designing big buildings
> Ryan,mr_teck,Minecraft robots
> Craig,rrrrrrrr,TNT expert
> ```
>
> 这个CSV文件有一行标题，包含了三个字段名：Name、Handle和Speciality。有四行数据，每行包含三个字段的数值。

> david says
>
> 在设计复杂的软件结构式，有时候使用其他辅助工具是很有用的。作为软件工程师，我总是觉得电子表格在设计和表示数据时很有用。电子表格提供了一种非常好的展示数据表格的方式，还可以导出为CSV文件并在程序中读入。你可以试试用自己最喜欢的电子表格软件来设计一个迷宫，把列调得很窄，用公式来实现输入0时显示百块，输入1时显示黄块。这样就可以把迷宫可视化，完成后可以到导出maze.csv，然后运行本节的程序，看看你和伙伴们能不能走出这个迷宫。

迷宫数据文件不需要标题行，因为文件的每一列都代表相同的数据类型：0表示空地，1表示墙。下面给出了一个例子，你可以直接从配套网站上下载这个数据文件：

1. 点击File->New File新建一个文本文件。点击File->Save As保存文件为maze1.csv。
2. 输入下面几行文本，仔细一点，注意每一行的列数都必须一样。如果你仔细看一下这些数据，就已经能看出迷宫的结构了！一共有16行，每行16个数字，你可以用铅笔划掉已经输入的行，这样就不会忘记自己输入到哪一行了。

    ```
    1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1
    0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1
    1,1,1,1,1,1,1,1,1,0,1,0,1,1,0,1
    1,0,0,1,0,0,0,0,1,0,1,0,1,0,0,1
    1,1,0,1,0,1,1,0,0,0,0,0,1,0,1,1
    1,1,0,1,0,1,1,1,1,1,1,1,1,0,1,1
    1,1,0,0,0,1,1,1,1,1,0,0,0,0,1,1
    1,1,1,1,1,1,0,0,0,0,0,1,1,1,1,1
    1,0,0,0,0,1,0,0,0,0,0,0,0,0,0,1
    1,0,1,1,1,1,0,0,0,0,0,1,1,1,1,1
    1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1
    1,0,1,1,1,1,1,1,1,1,0,1,1,1,1,1
    1,0,1,0,0,0,0,0,0,1,0,0,0,0,0,1
    1,0,1,0,1,1,1,1,0,1,1,1,1,1,0,1
    1,0,0,0,0,0,0,0,0,1,0,0,0,0,0,1
    1,1,1,1,1,1,1,1,1,1,0,1,1,1,1,1
    ```

3. 保存文件。等我们写好了读取并处理这些数据的Python程序之后，就要用到它了。

###建造迷宫

有了描述迷宫的数据文件之后，接下来就是最后一步，编写一个Python程序从文件中读取数据然后在Minecraft里用方块建造迷宫。

1. 点击File->New File新建文件，点击File->Save As保存文件为csvBuild.py。
2. 导入需要的模块。你需要通常的Minecraft模块，还需要block模块用来跟方块进行交互：

    ```
    import mcpi.minecraft as minecraft
    import mcpi.block as block
    import time
    ```

3. 连接到Minecraft游戏：

    ```
    mc = minecraft.Minecraft.create()
    ```

4. 定义一些常量。`GAP`是用作空地的方块类型。通常用空气，不过你也可以自己尝试用不同的方块类型来让迷宫变得更有趣。`WALL`是用来建造迷宫墙的方块类型，这个要小心选择，因为有些方块类型不适合用作墙。`FLOOR`常量用来建造迷宫的地板。

    ```
    GAP = block.AIR.id
    WALL = block.GOLD_BLOCK.id
    FLOOR = block.GRASS.id
    ```

    不要用沙子（SAND）做地板。如果这样，在某些情况下，地板可能会从steve的脚下流走！

5. 打开包含迷宫数据的文件。用常量FILENAME来存储文件名，这样以后如果要使用其他文件，修改起来更方便：

    ```
    FILENAME = "maze1.csv"
    f = open(FILENAME, "r")
    ```

6. 获得角色的位置，然后算出迷宫角落的坐标。在角色坐标的x和z上加一，这样迷宫就不会建在角色的头顶上了。我们可以调整y坐标把迷宫建在空中：

    ```
    pos = mc.player.getTilePos()
    ORIGIN_X = pos.x+1
    ORIGIN_Y = pos.y
    ORIGIN_Z = pos.z+1
    ```

7. z坐标在处理每一行数据的时候都会变化，这里让它从迷宫的起点开始。在后面的程序中会修改z：

    ```
    z = ORIGIN_Z
    ```

8. 循环处理每一行数据。参考“深入代码”一节，里面解释了`readlines()`函数的作用。for循环会逐行处理文件里的每一行。每次循环开始的时候，变量`line`里都存储了从文件中读到的下一行数据：

    ```
    for line in f.readlines():
    ```

9. 用逗号把一行数据分割开。函数`split()`的作用在“深入代码”里有解释。记住这里所有的代码都是`for`循环的一部分，需要缩进一级：

    ```
        data = line.split(",")
    ```

10. 仔细看接下来的步骤：这里又有另外一个`for`循环。这叫做“嵌套循环”，因为一个循环嵌入在了另一个循环里。在建造迷宫每一行的时候都需要重新设置x坐标，这必须在用于建造整行的`for`循环之前进行：

   ```
       x = ORIGIN_X
       for cell in data:
   ```

11. 一行数据里的每个数字其实都是以字符串的形式读进来的，所以下面的程序必须在数字外面加上引号（""）才能正常工作。这个`if`/`else`语句根据从CSV文件里读入的数字决定是要建造空地还是墙。是0就建造空地，否则就建造墙。请确保缩进正确：if语句缩进两级，因为它是`for cell in data`循环的一部分，而后者又是`for line in f.readlines()`循环的一部分。变量b让程序变得更简洁。（试试不用b重写这段程序你就知道我的意思了。）

    ```
            if cell == "0":
                b = GAP
            else:
                b = WALL
            mc.setBlock(x, ORIGIN_Y, z, b)
            mc.setBlock(x, ORIGIN_Y+1, z, b)
            mc.setBlock(x, ORIGIN_Y-1, z, FLOOR)
    ```

12. 在`for cell`循环的最后修改x坐标。这一行的缩进跟`mc.setBlock()`是一样的，因为它也是`for cell in data`循环的一部分。

    ```
            x = x + 1
    ```

13. 在处理每一行的循环结尾修改z坐标（这里只需要缩进一级，因为它是`for line in f.readlines()`循环的一部分）。

    ```
        z = z + 1
    ```

保存程序，仔细检查确保缩进正确。如果缩进不对，这个程序是不能正常工作的，我们可能会得到一些奇形怪状的迷宫！

运行程序，一个奇妙（并且也很难走通）的迷宫就会出现在你面前。进去走走，看能不能找到出口，不要破坏墙，也不要用飞行模式哦！图6-2是在地面上看迷宫的样子。

如果卡住了，可以作弊一下，飞到空中俯瞰一下迷宫全貌，像图6-3一样。

> 定义
>
> **嵌套循环**是在另一个循环里的循环。前者叫做外层循环，后者叫做内层循环。在Python里，内层循环要增加一级缩进。循环可以嵌套任意多层。

> david says
>
> 如果意外在迷宫数据文件的结尾留了一些空行，它们也会被程序读入并处理。幸运的是，这不会让程序崩溃，但是有可能会在迷宫的后面看到一些多余的方块。

------

<u>深入代码</u>

在之前的冒险任务中，我们已经试验过Python的列表，并且知道了列表就是0或多个用数字做下标的数据项。Python语言里的很多内置函数都提供列表形式的数据。在迷宫建造程序里，我们有又遇到了两个这样的函数。

第一个是这行代码：

```
for line in f.readlines():
```

变量`f`是一个文件句柄——用来记录和访问已打开的文件。通常，Python的File类型有内置的`readlines()`函数。这个函数读取文件的所有行并加入到一个列表中。我们在以前的冒险任务中也学到，如果使用一个列表，`for`语句会循环处理列表中的每一个数据项。

`for`语句做的事情是，把文件中的所有行读入一个列表，然后逐一处理。每次循环开始，变量`line`（循环控制变量）中都保存下一行的数据。

如果有一个像下面这样的三行文件：

```
line one
line two
line three
```

那么`f.readlines()`会返回下面这样一个列表：

```
['line one', 'line two', 'line three']
```

程序中还有一个地方用到了Python的列表，你可能还没有意识到。就是下面这一行：

```
    data = line.split(",")
```

变量`line`保存的是从CSV文件中读入的一整行数据。所有的字符串变量都有一个内建的函数`split()`可以把它分割成列表。括号里的","告诉`split()`函数根据什么进行分割。

如果有`line`变量里保存的是用逗号连接的三个单词，像下面这样：

```
line = "one,two,three"
data = line.split(",")
```

那么`data`变量就会是一个包含三个数据项的列表，像这样：

```
['one','two','three']
```

------

> 挑战
>
> 在另一个CSV文件里设计自己的迷宫，加入曲折的通道、死胡同和环路来增加难度。让你的朋友们来挑战这些迷宫。你可能需要在迷宫里面以及出口设置一些随机的宝藏（例如钻石方块）以便你的朋友证明自己走完了整个迷宫，甚至可以建造一个占据大部分Minecraft世界的巨型迷宫！你打算怎样在迷宫数据文件里存储宝藏的位置呢？

> what happens
>
> 我在设计本章的程序时，不得不稍微修改一下迷宫程序，因为有时候迷宫太难走了。举个例子，如果去掉程序中生成地板的一行代码会怎样？试试用其他类型的方块建造迷宫的墙壁，比如仙人掌（CACTUS）和流水（WATER_FLOWING），看看会有什么变化。我尝试了很多个不同的版本才最终为墙壁和地板找到了满意的方块类型。

> 挑战
>
> 这个程序里隐藏了一个bug需要你找出来。如果某一行的最后一个数字是0，由于某种原因，程序还是会在哪里建造一个黄金方块作为墙。为什么会这样呢？试着解决这个问题。提示：我们在tipChat.py中已经解决过类似的问题。

> 提示
>
> 在这个冒险任务里，我们使用了`readlines()`和`split()`两个函数来处理CSV文件。Python是一个庞大且成熟的编程语言，有丰富的内建功能。你可以了解一下内置的CSV模块，可以通过import csv来使用。这个模块的功能要比我们这个程序里的方法强大很多。不过，我更喜欢用简单的东西来编写程序，这能帮助我理解背后的原理，这也是我在这本书里使用`readlines()`和`split()`的原因。

##制造3D方块打印机

建造迷宫是很有趣的，不过掌握了这些数据文件的基础之后，我们还可以做更多的事情！为什么要止步于只用两种方块建造单层结构呢？我们现在要以这个迷宫程序为基础来建造自己的3D扫描仪和打印机，这让我们可以一键复制树木、房屋以及在Minecraft里可以建造的任何东西，并在其他地方重建它们。这其实只是一个方块建造程序，但是我喜欢把它叫做3D打印机，因为我们可以看到方块一行一行的被建造出来，就像电脑打印机在纸上一行一行的打印文字，或者真正的3D打印机一层一层的建造立体结构一样。

我们现在写程序已经很熟练了，并且每个程序都比之前的规模更大。跟之前的冒险任务里用多个函数来组成程序一样，我们也要一步一步的来实现这个程序。

> david says
>
> 把不同的功能编写城函数，然后把它们组合成一个大程序，这是职业程序员常用的开发方法。它的基础是大程序只是一些小程序的集合。如果整个程序从一开始的设计是正确的，那么其中每个子功能详细内容可以在编写和测试程序的过程中逐渐补充。我总是喜欢随手准备一些样例程序，这样如果有人来问“喔，你写的这个程序是干什么的？”，我马上就可以给他们看一些可以运行的例子。

###手工建造一个用来3D打印的测试物体

在迷宫程序中，数据存储在CSV文件里，文件里的每一行代表Minecraft世界里的一行方块。行中的每一列（逗号分隔）代表一个方块。这是一个二维结构，因为它给矩形区域里的每一对x和z座标记录一个方块类型。我们建造的迷宫只是平面（二维）迷宫，因为它们只有一层。它们不是立体的，只是用3D方块建造在了立体的Minecraft世界里。

要建造3D形体，我们还需要在y维度上进行建造。如果你把3D物体当作是多个层次，那么这个3D建造程序跟迷宫程序就没有太大的不同。对于一个5\*5\*5体积的立方体，我们要做的只是存储5个层次的信息。

这里有个问题，我们的程序要处理完整个文件之后才知道一共有多少行。我们可以先假定一个行数，不过设计一个可以适用于任何尺寸物体的灵活的CSV文件格式会更好。

为了解决这个问题，我们会在文件第一行增加一些元数据来描述物体的大小。

> 定义
>
> **元数据**是用来描述数据的数据。如果文件里的数据内容是要建造的方块的类型，那么元数据可以是对象的大小、名字或设计者姓名等。就像电脑里存储的照片元数据描述了拍照的日期时间、相机名称一样，3D打印机的元数据也会描述一些关于3D对象的额外信息。

我们现在要创建一个微型的3D洞穴作为例子来测试程序的3D打印能力。这个物体看起来就像图6-4所示的那样，可以只直接从配套网站下载或者像之前一样手动输入。

> 提醒
>
> 请确保在不同层次之间加入一个空行。它们可以帮你找到每一层开始的位置，我们的程序也需要用到它们，否则就不能正常工作。

> 提示
>
> 在下面步骤里可以尽量使用编辑器菜单里的复制和粘贴，这可以减少输入量，也可以避免错误输入错误。

1. 点击File->New File新建一个文件，保存为object1.csv。
2. 输入第一行描述物体大小的元数据。测试物体的大小是5\*5\*5。第一个数字是x方向的尺寸（宽度），第二个数字是y方向的尺寸（高度），第三个数字是z方向的尺寸（长度）：

    ```
    5,5,5
    ```

3. 输入测试物体的第一层，注意第一行数据前面有一个空行。最底层的所有方块都必须被填充，所以这里的数字都是1：

    ```
    1,1,1,1,1
    1,1,1,1,1
    1,1,1,1,1
    1,1,1,1,1
    1,1,1,1,1
    ```

4. 我们要建造一个正面开口的立方体，接下来的三层都是这种形式。注意，下面的数据前面有一个空行：

    ```
    1,1,1,1,1
    0,0,0,0,1
    0,0,0,0,1
    0,0,0,0,1
    1,1,1,1,1
    ```

5. 用复制粘贴的方法在这一层下面增加两层，确保每个数字方阵前面都有一个空行。
6. 最后，把物体的顶部封闭起来。复制第三布中的数据并粘贴，同样要保证前面有一个空行。

保存文件，不要运行——这是个给Python程序用的数据文件，而不是程序，所以不能运行。

###编写3D打印程序

既然已经有了测试数据，现在就应该编写3D打印程序在Minecraft世界里建造这个中空的洞穴了。这个程序跟之前的迷宫程序很像，不过它有三层循环，x、y和z三个维度各有一层循环。

> david says
>
> 这可能是整本书里要编写的最详细的程序之一了。每一个步骤都要看仔细，最后的结果会让你惊讶！如果碰到问题，请仔细检查缩进。记住，这本书里所有程序都可以从配套网站上下载。

1. 点击File->New File新建一个文件，点击File->Save As保存为print3D.py。
2. 导入需要的模块：

    ```
    import mcpi.minecraft as minecraft
    import time
    import random
    ```

3. 连接到Minecraft游戏：

    ```
    mc = minecraft.Minecraft.create()
    ```

4. 新建一个表示数据文件名的常量。这样以后如果要读取不同的文件修改起来会比较容易：

    ```
    FILENAME = "object1.csv"
    ```

5. 定义print3D()函数。我们在本章最后的项目中还会用到这个函数，所以请确保命名正确：

    ```
    def print3D(Filename, originx, originy, originz):
    ```

6. 就像在迷宫建造程序中一样，以读方式打开文件并把所有行读入到一个Python列表里：

    ```
        f = open(Filename, "r")
        lines = f.readlines()
    ```

7. 列表中的第一个数据项下表为0。这是文件里的第一行，保存了元数据。用`split()`函数把这一行分成多个部分。然后把这三个数字保存到变量`sizex`、`sizey`和`sizez`里。这里必须使用`int()`函数，因为从文件里读入数据时，得到的是字符串，但是我们需要数值以进行之后的计算：

    ```
        coords = lines[0].split(",")
        sizex = int(coords[0])
        sizey = int(coords[1])
        sizez = int(coords[2])
    ```

8. 设置一个变量`lineidx`用来记录当前处理的行在`lines[]`数组中的下标。因为我们的程序要扫描整个`list`列表，读取3D物体不同层的数据，所以我们不能使用这个变量做循环控制变量：

    ```
        lineidx = 1
    ```

9. 第一层`for`循环要扫描从文件中读取的所有垂直层。文件前面的行用来建造物体底部的层次。我们可以在这里加一个`postToChat()`语句，这样就可以在Minecraft里看到打印的进度：

    ```
        for y in range(sizey):
            mc.postToChat(str(y))
    ```

10. 把`lineidx`变量加1以跳过文件中层与层之间的空行。记住这些空行存在的意义仅仅是为了方便人阅读，所以程序里要跳过它们。

   ```
           lineidx = lineidx + 1
   ```

11. 开始一个嵌套循环，读入文件中的下一行并按照都好分割。注意这里的缩进，`for`需要缩进两级（一级是因为在函数体里，另一级是因为在`for y`循环里），里面的代码要缩进三级。

    ```
            for x in range(sizex):
                line = lines[lineidx]
                lineidx = lineidx + 1
                data = line.split(",")
    ```

12. 现在写第三个`for`循环，扫描刚刚读入的行里的每一个方块并在Minecraft里建造它们。`for z`循环需要缩进三级，循环里面的语句要缩进四级，这里要非常仔细，不然有可能会造出一些奇形怪状的东西！

    ```
                for z in range(sizez):
                    blockid = int(data[z])
                    mc.setBlock(originx+x, originy+y, originz+z, blockid)
    ```

13. 最后，输入主程序的语句，不需要缩进。这些代码读取玩家角色的坐标，然后调用`print3D()`函数在他身边“打印”出我们的3D物体：

    ```
    pos = mc.player.getTilePos()
    print3D(FILENAME, pos.x+1, pos.y, pos.z+1)
    ```

保存程序并运行看一下效果！走到另一个地方再运行程序。每次运行程序，都会在玩家附近建造一个中空的石头洞穴！图6-5展示了快速建造大量石头洞穴有多么容易。

> 挑战
>
> 创建一些包含3D物体的新CSV文件，修改print3D.py程序里的`FILENAME`常量并重新运行来建造你的物体。随着物体结构越来越负载，你会需要想一些新的办法来设计这些物体，然后才能把数据输入到文件里。在电子表格里绘制物体是一种方式。或者干脆买一盒塑料积木在现实世界中搭建好你的物体，然后再把它分解成层并变成数字输入到CSV文件。

##建造3D方块扫描仪

我们的3D打印程序已经很强大了。你可以建起一个巨大的CSV文件库来保存想要建造的物体，然后当需要建造的时候，只要修改`FILENAME`常量并运行print3D.py程序就可以在Minecraft世界里随处建造你的物体。

不过，只靠手工输入CSV文件的话，建造更大更复杂的物体会很困难。而Minecraft本身就是建造复杂物体的最佳程序——所以，为什么不用Minecraft来帮你创建CSV数据文件呢？现在，我们遇到一棵树并拥抱它，然后记下它的样子，就可以在Minecraft里的任何地方复制一模一样的树了。幸运的是，3D扫描的工作过程差不多是3D打印反过来，所以这可能没有你想想的那么困难。

1. 点击File->New File新建一个文件，点击File->Save As保存为scan3D.py。
2. 导入需要的模块：

    ```
    import mcpi.minecraft as minecraft
    import time
    import random
    ```

3. 连接到Minecraft游戏：

    ```
    mc = minecraft.Minecraft.create()
    ```

4. 声明一些常量用来表示CSV文件名和扫描区域的大小：

    ```
    FILENAME = "tree.csv"
    SIZEX = 5
    SIZEY = 5
    SIZEZ = 5
    ```

5. 定义一个`scan3D()`函数。后面的程序还会用到这个函数，所以请确保命名正确：

    ```
    def scan3D(Filename, originx, originy, originz):
    ```

6. 打开文件，不过这次要在`open()`函数里使用"w"模式：

    ```
        f = open(Filename, "w")
    ```

7. 把x、y、z方向上的尺寸写入第一行当作元数据。结尾"\n"的含义和用处参见“深入代码”。

    ```
        f.write(str(SIZEX) + "," + str(SIZEY) + "," + str(SIZEZ) + "\n")
    ```

8. 扫描程序是打印程序的逆向过程，所以一样需要三层嵌套循环。下面把这部分程序一次给出，这样更容易看清每行的缩进。关于如何生成逗号分隔的文本行，请参考“深入代码”：

    ```
        for y in range(SIZEY):
            f.write("\n")
            for x in range(SIZEX):
            line = ""
            for z in range(SIZEZ):
                blockid = mc.getBlock(originx+x, originy+y, originz+z)
                if line != "":
                    line = line + ","
                    line = line + str(blockid)
            f.write(line + "\n")
        f.close()
    ```

9. 最后是主程序，不需要缩进。这里只需要获取角色的位置，然后计算一个以角色为中心的立方体空间，然后调用`scan3D()`函数扫描这个空间并保存到CSV文件。

    ```
    pos = mc.player.getTilePos()
    scan3D(FILENAME, pos.x-(SIZEX/2), pos.y, pos.z-(SIZEZ/2))
    ```

保存程序。在运行之前请再次检查缩进是否正确。

找一棵树并“拥抱”它（尽可能靠近它的树干），运行scan3D.py程序。看看发生了什么？

有事情发生么？记住这个程序会扫描一个物体保存到CSV文件，所以我们需要看一下tree.csv文件来看看里面保存的数据。在编辑器菜单中选择File->Open打开tree.csv文件。图6-6展示了我自己扫描一棵树得到的tree.csv文件的一部分。因为扫描的范围比较小，我们可能常常只能扫描到半颗树，不过可以修改常量`SIZE`来扫描更大的区域。

> david says
>
> 注意角色面朝额方向不一定是扫描的方向，请观察一下周围的区域。3D扫描程序会扫描角色周边的范围，通过逐渐增加坐标值的方式。所以，如果角色站在坐标为0,0,0的位置，3D扫描程序就会从那里开始扫描到坐标更大的位置，比如4,4,4。复习一下冒险任务2里的图表，在那里我们已经了解过坐标的有关知识，并且知道了坐标的各个值分别表示哪个方向。

> 提示
>
> IDLE在打开和保存文件的窗口中通常之显示扩展名为.py的文件，但是只要点击Files of Type->All Files就可以看到.csv文件。

> 挑战
>
> 现在我们有了3D扫描程序和3D打印程序，修改3D打印程序是常量FILENAME的值为tree.csv，然后在Minecraft世界里转悠一下，并且反复运行print3D.py。这样就可以在任何位置打印我们扫描的那颗树。试试在天空中或者水里打印一些树，看会发生什么。用这种方法是不是很快就能拥有一片森林？

------

<u>深入代码</u>

在scan3D.py程序里，我们在调用`write()`函数的时候使用了一个特殊字符，需要解释一下：

```
f.write("\n")
```

字符“\n”是一个可以在引号中使用的不可打印的特殊字符。它的含义是“转移到下一行”。字母n代表了“newline”（新的一行）。反斜线“\”用来区分这个字符和普通的字母n。

在scan3D.py里，我们设计的文件格式在物体的两层中间有一个空行，便于其他人来阅读和编辑CSV文件。`f.write("\n")`就是用来增加空行的。

在这之后，用来计算变量line的一些代码也值得一提。下面是这段代码的关键部分：

```
for x in range(SIZEX):
    line = ""
    for z in range(SIZEZ):
        blockid = mc.getBlock(originx+x, originy+y, originz+z)
        """
        if line != "": # line is not empty
            line = line + ","
        """
        line = line + str(blockid)
```

下划线的部分是用来组合逗号分隔的值时很常用的一种编程模式。当x循环开始时，变量`line`被赋值为空字符串。每当z循环产生一个新数值时，首先检查`line`是不是空字符串，如果不是的话就增加一个逗号。最后再把这个方块的`blockid`添加到`line`的后面。这个if语句用来防止`line`的最前面出现逗号，否则print3D.py再把数据读回来的时候就会出问题。

------



##建造复制机

我们现在已经具备了设计和建造3D复制机的所有基础。有了它，我们就可以在Minecraft世界里把神奇的复制室变成现实。我们可以在那里建造任何东西，然后保存这些东西到文件，把它们重新加载到复制室进行修改或者复制到Minecraft世界的各个地方。最后，我们还可以离开Minecraft世界并让复制室彻底消失，不留任何痕迹。

像之前的冒险任务一样，我们会使用现有的程序，把它们组合在一起成为一个更大的程序。这个程序有多种功能，所以我们需要添加一个菜单方便进行控制。

###编写复制机程序的框架

首先要写一个把所有功能组合在一起的程序框架。我们先用一些调用时只打印函数名的空函数来编写这个框架，然后逐渐用其他程序里已经有的函数来填充它们。你可能还记得这跟冒险任务4里编写寻宝游戏的方法是一样的。

1. 点击File->New File新建一个文件，点击File->Save As保存为duplicator.py。
2. 导入需要的模块：

    ```
    import mcpi.minecraft as minecraft
    import time
    import random
    import glob
    import time
    import random
    ```

3. 连接到Minecraft游戏：

    ```
    mc = minecraft.Minecraft.create()
    ```

4. 设置一些常量来表示复制机的大小。不要把这些变量值设的太大，不然扫描和打印物体的时间就会变得很长。另外还要设置复制室的初始位置：

    ```
    SIZEX = 10
    SIZEY = 10
    SIZEZ = 10
    roomx = 1
    roomy = 1
    roomz = 1
    ```

5. 给程序需要的所有功能定义对应的空函数。我们很快就会填写这些函数的具体内容：

    ```
    def buildRoom(x, y, z):
        print("buildRoom")

    def demolishRoom():
        print("demolishRoom")

    def cleanRoom():
        print("cleanRoom")

    def listFiles():
        print("listFiles")

    def scan3D(Filename, originx, originy, originz):
        print("scan3D")

    def print3D(Filename, originx, originy, originz):
        print("print3D")
    ```

6. 空的菜单函数比较特别，因为它在执行结束时会返回一个数值。目前，我们可以返回一个随机数以便于对程序的早期版本进行测试，不过我们很快会编写一个名副其实的菜单。我们现在写的只是一个用于测试程序结构的空菜单，不过很快就会填上真正的菜单：

    ```
    def menu():
        print("menu")
        time.sleep(1)
        return random.randint(1,7)
    ```

7. 编写游戏主循环，显示菜单并调用某个功能对应的函数。变量`anotherGo`的用法在“深入代码”里有详细的讲解：

    ```
    anotherGo = True
    while anotherGo:
        choice = menu()

        if choice == 1:
            pos = mc.player.getTilePos()
            buildRoom(pos.x, pos.y, pos.z)

        elif choice == 2:
            listFiles()

        elif choice == 3:
            Filename = raw_input("Filename?")
            scan3D(Filename, roomx+1, roomy+1, roomz+1)

        elif choice == 4:
            Filename = raw_input("Filename?")
            print3D(Filename, roomx+1, roomy+1, roomz+1)

        elif choice == 5:
            scan3D("scantemp", roomx+1, roomy+1, roomz+1)
            pos = mc.player.getTilePos()
            print3D("scantemp", pos.x+1, pos.y, pos.z+1)

        elif choice == 6:
            cleanRoom()

        elif choice == 7:
            demolishRoom()

        elif choice == 8:
            anotherGo = False
    ```

保存并运行程序。有什么结果？图6-7展示了在我的电脑上运行程序的结果。程序在告诉我们它在做什么——它随机选择菜单中的一项，然后调用处理这个菜单项的函数。我们现在能看到的只是每个函数打印出自己的函数名。你在自己的电脑上看到的单次序列可能跟图6-7不一样，这是因为`menu()`函数在每次运行的时候会随机选择一个项目。

> 定义
>
> **返回值**是函数向调用者传递某个值的首选方式。
>
> 举个例子，当我们需要一个随机数时，调用`randint()`函数。这个函数会返回一个值，我们可以像这样把它保存在变量里：`a = random.randint(1,100)`。Python里的返回值让我们可以用同样的方式从自己的函数里传递回一个值。

> 提示
>
> 函数`raw_input()`读取从键盘输入的一行文字。如果在括号里填上一个字符串，它会被用作提示符。所以，`name = raw_input("What is your name?")` 这行代码会提出一个问题并读取你的答复。
>
> 当使用`raw_input()`读取键盘输入时，会返回一个字符串。如果想要输入一个数字（例如在菜单系统里），需要使用`int()`函数把字符串转换成数字。有些Python程序员喜欢在一行代码里完成这些工作，像这样：`age = int(raw_input("What is your age?"))`。

> 提醒
>
> 在这本书里我们使用的是Python 2。如果换另外一台电脑，可能安装的是Python 3。二者之间有一些区别，其中之一是在Python 3里，我们需要使用`input()`来替代Python 2里的`raw_input()`。这本书使用Python 2，因为Minecraft API只能在Python 2下正常工作，要在Python 3中使用需要一些修改。

------

<u>深入代码</u>

我们在其他的冒险任务中已经使用过**布尔型**变量，不过duplicator.py程序里的变量`anotherGo`的用法还是值得谈一谈的。这里是关键的代码：

```
anotherGo = True
while anotherGo:
    choice = menu()
    if choice == 8:
        anotherGo = False
```

这是一种常见的程序设计模式，循环至少执行一次，询问你是否需要再执行一次，如果不需要，就退出循环。在Python里有很多不同的方式都能达到这个目标，不过使用布尔型变量是一种很好的方法，因为这样程序的逻辑会很清晰。

Python有一种`break`语句可以从循环中跳出，不过我们在这本书里不会用到。你可以上网查些资料看如何能用`break`语句改写这段程序，这样就不需要布尔型变量了。

------



> 定义
>
> **布尔型**变量只能保存两种值——真（True）或假（False）。这是以数学家乔治·布尔来命名的，他在19世纪为形式逻辑做出了很多贡献，其中很多成果奠定了现代计算理论的基础。[这里]( http://en.wikipedia.org/wiki/George_Boole)可以了解到更多布尔的故事。

###显示菜单

菜单系统是一个很有用的功能，特别是当我们的程序有很多选项可以选择的时候。菜单系统打印所有选项，然后进入循环，等待输入一个指定范围内的数字。如果输入数字不再范围内，它会重新打印菜单让你重试。

1. 用下面代码替换原来的菜单函数，请确保缩进正确：

    ```
    def menu():
        while True:
            print("DUPLICATOR MENU")
            print(" 1. BUILD the duplicator room")
            print(" 2. LIST Files")
            print(" 3. SCAN from duplicator room to File")
            print(" 4. LOAD from File into duplicator room")
            print(" 5. PRINT from duplicator room to player.pos")
            print(" 6. CLEAN the duplicator room")
            print(" 7. DEMOLISH the duplicator room")
            print(" 8. QUIT")
            
            choice = int(raw_input("please choose: "))
            if choice < 1 or choice > 8:
                print("Sorry, please choose a number between 1 and 8")
            else:
                return choice
    ```

保存并运行程序。现在的程序跟之前用空`menu()`函数时有什么不同？图6-8展示了真正的菜单的样子。

> david says
>
> 先用菜单系统和空函数来编写整个程序的框架，然后逐渐填充每个函数的内容，并重新进行测试，这种工作方式跟现代软件工程师开发计算机程序的方式是一样的。把程序分成多个小的组成部分逐步编写是一个很好的习惯，先进行一些修改，然后再测试修改是否正确，直到完成整个程序。这也意味着你可以随时给给任何一个拍你肩膀的人展示你正在编写的程序。

###建造复制室

复制室要用玻璃方块来建造，并且前方要有一个开口，这样才可以随时进去添加或者删除方块。

用下面代买替换`buildRoom()`函数。注意看使用`setBlocks()`的那几行很长的代码，我这里没有使用箭头，因为Python允许把这些代码分成多行（阅读“深入代码”一节来理解为什么有时候代码可以分成多行而有时候又不行）：

```
def buildRoom(x, y, z):
    global roomx, roomy, roomz

    roomx = x
    roomy = y
    roomz = z
    mc.setBlocks(roomx, roomy, roomz, 
    roomx+SIZEX+2, roomy+SIZEY+2, roomz+SIZEZ+2,  
    block.GLASS.id)
    
    mc.setBlocks(roomx+1, roomy+1, roomz, 
    roomx+SIZEX+1, roomy+SIZEY+1, roomz+SIZEZ+1, block.AIR.id)
```

保存程序并再次测试。测试选项1看是否能正确建造复制室。换到另外一个地方再次选择选项1看会怎样。图6-9展示了刚刚建造的复制室。

------

<u>深入代码</u>

Python使用左侧缩进（空格或制表符）来表示哪些语句属于同一组。我们使用`if`/`else`语句，或者函数时，都是在用缩进来组织程序语句。Python的这种用缩进组织代码块的方法并不是很常见，其他编程语言例如C和C++使用特殊字符{和}来组织程序语句。所以写Python程序时必须非常小心缩进问题，否则程序可能不会正常工作。

大多数时候，Python不允许把一行长的代码分成多行。不过在Python里面有两种方式来处理很长的代码行。

首先，我们可以使用续行符——如果一行的最后一个字符是反斜线（\），那么下一行的代码就是本行的延续。

```
a = 1
if a == 1 or \
  a == 2:
    print("yes")
```

另一种把代码分成多行的方式是在一些本行明显没有结束的地方换行。例如，在设置一个列表的初始值时，或者调用函数时，Python可以从前面的部分代码中知道这一行还没有结束，还有后续代码。下面有两个例子里Python可以把一行长代码分成多个短行，因为前面的括号告诉Python这一行还没有结束，直到出现另一个与之匹配的反括号：

```
names = ["David",
         "Steve",
         "Joan",
         "Joanne"
        ]

mc.setBlocks(x1, y1, z1,
x2, y2, z2, block.AIR.id)
```

------

###拆除复制室

复制程序运行了很多次之后，就会在Minecraft世界里留下很多复制室。只有最后创建的一个才是真正有用的。过一段时间之后就会有太多的复制室，你很难找到正确的那一个。要解决这个问题，我们需要给程序添加一个拆除复制室的功能，这样才不会搞的地图上都是没用的复制室。

拆除复制室的方法跟冒险任务3里用clearSpace.py一样。我们只需要知道复制室外层的坐标。因为复制室在各个方向上的尺寸只比`SIZEX`、`SIZEY`和`SIZEZ`定义的被复制空间大一个方块，因此很容易计算外层的坐标。

把`demolishRoom()`函数修改成下面这样：

```
def demolishRoom():
    mc.setBlocks(roomx, roomy, roomz, roomx+SIZEX+2, roomy+SIZEY+2, roomz+SIZEZ+2, block.AIR.id)
```

保存并再次运行程序。现在建造和拆除复制室变得很容易。只需要选择菜单中的选项1和7就可以。

###扫描复制室里的物体

我们在scan3D.py程序里已经写过这部分代码了，只需稍微修改一下就可以使用。

1. 用下面代码替换`scan3D()`函数。这段代码跟scan3D.py程序几乎一样，只是增加了粗体显示的几行代码用来在聊天栏显示扫描进度。扫描一个大房间可能需要很长时间，所以最好能够提示一下玩家程序完成了多少。我们可以直接从原来的程序里面复制代码粘贴过来以节省输入的时间：

    ```
    def scan3D(Filename, originx, originy, originz):
        f = open(Filename, "w")
        f.write(str(SIZEX) + "," + str(SIZEY) + "," + str(SIZEZ) + "\n")
        for y in range(SIZEY):
            mc.postToChat("scan:" + str(y))
            f.write("\n")
            for x in range(SIZEX):
                line = ""
                for z in range(SIZEZ):
                    blockid = mc.getBlock(originx+x, originy+y, originz+z)
                    if line != "":
                        line = line + ","
                    line = line + str(blockid)
                f.write(line + "\n")
        f.close()
    ```

2. 保存程序并再次测试，进入复制室里建造一些东西，然后选择菜单里的选项3。打开扫描程序创建的文件，检查物体是不是已经被正确扫描。图6-10展示了文件的部分内容。注意里面有很多0；这是因为程序把空气（AIR）方块也扫描了进去。

###清除复制室

想要彻底清理干净复制室？有时候清空复制室从头开始是很有用的。我们可以通过拆除和重建复制室来实现，不过再加一个清除功能也很简单，跟`demolishRoom()`函数相比只有坐标不同。

1. 把`cleanRoom()`函数修改成下面这样。注意起始坐标都比复制室的边缘要大，而结束坐标则没有`demolishRoom()`函数那么大。这样就可以清除内部空间而不破坏复制室的墙壁：

    ```
    def cleanRoom():
        mc.setBlocks(roomx+1, roomy+1, roomz+1, roomx+SIZEX+1, roomy+SIZEY+1, roomz+SIZEZ+1, block.AIR.id)
    ```

2. 保存并重新运行程序。在复制室里建造一些东西，然后选择菜单选项6测试是否能清空复制室里的东西。

###在复制室里打印

打印（复制）复制室里的对象很容易，因为我们已经编写过的print3D.py程序可以完成这个功能，这里也是一样。下面给出完整的代码：

```
def print3D(Filename, originx, originy, originz):
    f = open(Filename, "r")
    lines = f.readlines()
    coords = lines[0].split(",")
    sizex = int(coords[0])
    sizey = int(coords[1])
    sizez = int(coords[2])
    lineidx = 1
    for y in range(sizey):
        mc.postToChat(str(y))
        lineidx = lineidx + 1
        for x in range(sizex):
            line = lines[lineidx]
            lineidx = lineidx + 1
            data = line.split(",")
            for z in range(sizez):
                blockid = int(data[z])
                mc.setBlock(originx+x, originy+y, originz+z, blockid)
```

保存并重新运行程序。在复制室里建造一些东西，然后走到其他地方，在菜单里选择选项5。复制室里的东西就会被扫描并打印到角色所在位置的旁边。我们应该可以到Minecraft世界的任何地方反复的复制这些东西。试试在空中或者水下复制，看会发生什么。图6-11展示了运行程序的结果。

###列出文件清单

最后一个任务是写一个小程序列出文件系统里所有的CSV文件。直接用文件管理器也可以，不过把这个功能加到程序里会更好：

1. 修改`listFiles()`函数，使用`glob.glob()`函数来读取文件列表并打印出来。代码的解释请看“深入代码”环节。

    ```
    def listFiles():
        print("\nFILES:")
        Files = glob.glob("*.csv")
        for Filename in Files:
            print(Filename)
        print("\n")
    ```

2. 保存并重新运行程序。扫描一些物体到CSV文件里，然后选择菜单里的选项2，确认这些文件是不是都列出来了。图6-12是我在自己电脑上运行的结果。

------

<u>深入代码</u>

`glob.glob()`——作为函数名看起来有点滑稽，滑稽到我们要输入两次！

glob这个名字出现两次是因为，第一次是程序前面导入的模块的名字（glob模块）。第二次是glob模块里glob函数的名字。

不过，glob函数的功能是什么？为什么要起名叫glob呢？

glob是“glob command”（全局命令）的简称，来自于早期Unix操作系统的设计。在[这个](http://en.wikipedia.org/wiki/Glob_(programming))维基百科页面可以看到glob这个名字背后的历史。

它的功能是收集一个满足指定模式（或者**通配符**）的文件的列表。当调用`glob.glob("*.csv")`时，Python搜索文件系统里当前目录，生成一个所有以“.csv”结尾的文件的列表。

如果在MyAdventures目录下面有one.csv、two.csv和three.csv三个文件，使用`glob.glob("*.csv")`会返回下面这样一个Python列表：

```
['one.csv', 'two.csv', 'three.csv']
```

`for`循环会逐个处理列表中的每一个数据项，因此下面这样的代码非常有用：

```
for name in glob.glob("*.csv"):
    print(name)
```

------

> 定义
>
> **通配符**是一类可以用来选择很多相似的名字或者单词的特殊字符。它的作用类似于扑克牌里的“王”或者白搭牌，可以代表任何东西。
>
> 在Python里，通配符通常用来选择文件系统里名字相似的文件，例如“所有的CSV文件”。这可以用`glob.glog("*.csv")`来实现，其中的*就是通配符，函数`glob.glob()`会返回文件系统中所有以.csv结尾的文件。

> 挑战
>
> 在duplicator.py程序里，如果输入一个不存在的文件名，程序就会出错崩溃，把复制室留在Minecraft世界里。上网查一下如何检测文件是否存在，把程序修改得更加健壮，避免崩溃。
>
> Martin为我测试duplicator.py程序的时候，他发现了一个bug——房间实际上要比我们设想的尺寸更大一点。这表示如果建造的物体刚好占满整个房间，那么扫描的时候最外面一圈就会被丢掉，而打印的时候也会缺少最外层的一圈。如果这正好是一所房子（或者其他东西）的外层，就会很不方便。试试看你能不能找到这个问题的原因并解决它。

##关于数据文件的进一步冒险

在这个冒险任务里，我们学会了如何读写数据文件。这给了我们保存和恢复Minecraft世界某一部分的能力，甚至可以从其他来源（例如网站）获取大量真实世界的数据。我们自己建造了拥有各种曲折通道和死胡同的3D迷宫，最后还开发了一个具有完整菜单系统的3D扫描和打印程序。编写菜单这项技术在其他很多程序里也非常有用。

* 互联网上有很多真实数据源。我在为本书搜集资料的时候发现的一个数据源是[这里](http://data.nottinghamtravelwise.org.uk/parking.xml)的诺丁汉城市议会停车场数据。这个数据妹5分钟更新一次，并且会跟踪车辆的进出。我编写了一个处理这些数据并打印出有用信息的Python程序，放在了[我的github页面](https://github.com/whaleygeek/pyparsers)上。试试看你能不能利用这些编写一个游戏在Minecraft世界里重建停车场和里面的汽车。例如一个停车挑战游戏，玩家要在有限的时间里从Minecraft世界里找到一个空闲的停车位。
* 关于3D迷宫的一些信息在[这个](http://www.astrolog.org/labyrnth/algrithm.htm)神奇的网页上都有详细的解释。网页上有很多用普通文本文件存储的多层3D迷宫的例子。浏览一下这个网页，看你能否找一个多层迷宫文件，利用在3D复制机程序里学到的技术改造你的迷宫程序，来建造巨大的多层迷宫。你甚至可以参考网页上的建议来尝试编写一个自动生成迷宫的Python程序。

##下一个冒险任务

在冒险任务7里，我们将要学习如何用Python程序来建造复杂的2D和3D物体。我们会学习如何用Minecraft时钟来计时，用少量Python代码来建造多边形和多面体——然后开始一次激动人心的金字塔探险！