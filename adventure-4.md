#冒险四：跟方块交互

有一种方式可以让Minecraft游戏变得更有趣，就是让游戏对玩家周围发生的情况做出相应的改变。当我们在游戏世界中活动时，所面临的选择会根据之前做过的事情而变化，每次玩游戏的经历都会有所不同。Minecraft API支持跟方块互动，可以检查游戏角色脚下方块的类型，以及探测某个方块何时被用剑击打。

在这个冒险任务里，我们首先会通过建造一座魔法桥来学习跟方块交互的基础知识。这座桥很特别，因为它会在玩家走上水面或空中的时候自动出现在脚下以保证安全。我们的游戏世界很快就会架满各种各样桥。这个程序的第2个版本会使用Python里的list（列表）记住在哪里建造了桥，并且当玩家走到安全的地方之后，就会让桥消失。

最后，我们会学习如何感知方块被击中，然后制作一个紧张刺激的寻宝游戏，利用魔法桥去寻找和收集随机出现在天空中的宝物，然后回到任务起始点并获得一个分数。

在这个冒险任务里，我们要像真正的软件工程师一样工作，通过编写一个又一个的函数并把它们组合在一起形成一个令人激动的大型程序。系好安全带，看清指示。这将是一次激动人心的天空之旅！

##探测脚下的方块

在冒险任务2里已经学到了可以使用`getTilePos()`读取坐标来跟踪角色的位置。得到的坐标代表玩家角色当前在Minecraft世界中x、y、z三个方向上的位置。我们已经用过这些坐标来探测玩家是否正站在门垫上，或者结合地理围栏技术来判断他是否在某个区域里。

不过，仅仅根据知道位置对我们来说还不够灵活，特别是当程序变得复杂的时候，除非我们的程序有一副详细的地图记录了Minecraft世界中每个方块的位置。

幸运的是，Minecraft API提供了一个`getBlock()`函数。这个函数让我们能够访问到Minecraft在内存中的完整地图，并且只要有坐标，我们就可以用它来获得任何方块的信息，不仅仅是玩家身边的，而是整个Minecraft世界中任何位置的任何方块。

在冒险任务3里，我们也知道了可以改变Minecraft世界中的任何方块的类型。`setBlock()`和`getBlock()`可以结合起来使用，所以如果你用`setBlock()`设置一个方块的类型，紧接着对同一方块使用`getBlock()`，就可以得到同样的类型代码。

马上我们就要在Minecraft里制作另一个刺激的游戏，不过这个程序会很大。编写大型程序的最佳方式是先编写一些小的程序，等它们都可以正常工作了，再组合在一起。让我们先来实践一下这种策略，写一个简单的程序，检测玩家是不是站在安全的地方。

###检查脚下

启动Minecraft和IDLE，如果使用PC或苹果电脑，也要启动服务器。到目前为止，你应该已经进行过多次启动这些程序的操作了，如果需要操作提示请参考冒险任务1。现在我们要编写一个程序来获得玩家角色位置的准确信息。我们需要知道他是否正站在地面上，然后才能决定要不要建造魔法桥。

1. 在菜单中选择File->New File打开一个新的编辑窗口。保存程序为safeFeet.py。记住程序必须保存在MyAdventures文件夹里，否则不能正常工作。

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

4. 这个程序将来会成为另一个更大的程序的一部分，所以我们要把这里的代码写在一个`safeFoot()`函数里。这样以后重复使用这段代码会比较容易。这个函数首先要做的事情是获得玩家角色的位置：

    ```
    def safeFeet():
        pos = mc.player.getTilePos()
    ```

5. `getBlock()`可以得到指定坐标位置方块的类型代码。`pos.x`、`pos.y`和`pos.z`是角色的坐标，所以要用`pos.y-1`来获得角色脚下的坐标：

    ```
        b = mc.getBlock(pos.x, pos.y-1, pos.z)
        # note: pos.y-1 is important!
    ```

6. 现在可以用一个简单的办法来检查玩家角色是不是安全。如果他正站在空气或者水面上，那就不安全。否则就是安全的。检查不安全的情况要简单的多，因为安全的方块类型有几百个。下面这行代码比较长，注意要输入在同一行：

    ```
        if b == block.AIR.id or b == block.WATER_STATIONARY.id or b == block.WATER_FLOWING.id:
    ```

7. 如果方块的类型是不安全的，在聊天局域栏里发送一条提示角色不安全的消息。否则他就是安全的。

    ```
            mc.postToChat("not safe")
        else:
            mc.postToChat("safe")
    ```

    请却确保程序的缩进正确，否则就不能正常工作。记住函数体里的所有代码都要缩进一级，`if`和`else`内部的代码还要再缩进一级。`if`和`else`语句应该对齐，因为它们是相关的，并且逻辑上属于同一层次。

8. `safeFeet()`函数到这里就结束了。在后面留一个空行提醒自己这是函数的末尾，之后写没有缩进的`while`游戏循环。像之前的程序一样，先加入一小段延迟，然后调用新的`safeFeet()`函数。

    ```
    while True:
        time.sleep(0.5)
        safeFeet()
    ```

在菜单中选择File->Save保存程序，点击Run->Run Module运行。

在Minecraft世界里走走看，你会看到“safe”（安全）或者"not safe"（不安全）的字样适时的显示在聊天栏里，就像图4-1那样。试试飞向空中或者下到海里看会发生什么。

> david says
>
> safeFeet.py程序和其他程序一样，需要在主游戏循环中插入一小段延迟。这并不是必须的，以后我们会遇到一些程序，如果加入延迟就会变得太慢以至于无法正常工作。不过，在给聊天栏发送消息的时候，延迟一会儿是很有用的，不然聊天栏就会很快被消息占满。试试把延迟的时间缩短，或者直接去掉看看会发生什么。

##建造魔法桥

在前一个程序里，我们写的代码可以感知玩家脚下的方块然后发送一条消息到聊天栏。这是通往一个更大程序的敲门砖，现在让我们看看把它和冒险任务3里的`setBlock()`函数结合起来，能不能做出一些更有用的东西。

只需要稍微修改一下safeFeet.py程序，就可以让它变成一个建造魔法桥的程序：不管玩家走到哪儿，都会在他脚下放置一个玻璃方块，保证他不会掉进海里或者空中。稍后我们还会重复使用这个新函数，请确保这里的函数名字是正确的。

1. 在菜单中选择File->Save As把safeFeet.py程序另存为magicBridge.py。

2. 修改`safeFeet()`函数的名字为`buildBridge()`，按照下面粗体自的方式修改`if`/`else`语句，去掉`postToChat()`，换成`setBlock()`。只要游戏角色的脚下不安全，就会在他脚下建造玻璃桥。注意`if`语句是很长的一样（而不是两行）：

    ```
    def buildBridge():
        pos = mc.player.getTilePos()
        b = mc.getBlock(pos.x, pos.y-1, pos.z)
        if b == block.AIR.id or b == block.WATER_FLOWING.id or b==block.WATER_STATIONARY.id:
            mc.setBlock(pos.x, pos.y-1, pos.z, block.GLASS.id)
    ```

3. 如第2步所示，`else`和`postToChat()`已经被去掉了，因为我们不再需要它们。

4. 如果建造魔法桥的速度太慢，玩家就会掉下去，所以我们要去掉游戏循环里的延迟。注意这里要用新的`buildBridge()`函数。

    ```
    while True:
        buildBridge()
    ```

5. 在编辑器菜单中选择File->Save As保存程序。

运行程序，然后四处走走，试着跳向空中或者走入水里。随着玩家的走动，只要他的脚下不安全，就会有神奇的魔法桥出现防止他跌落，就像图4-2中那样。现在我们的游戏角色可以在水上行走了。真是个奇迹！

> 提醒
>
> 这个程序在树莓派版中可以完美运行。在PC和苹果版上，反应就不够快了，所以玩家角色可能会往下掉，因为魔法桥出现的不够及时。你需要放慢速度，不要四处飞奔并指望魔法桥能一直保证角色的安全。你可以试试增加或者减少延迟来改进这个程序，或者使用爬行模式（在移动时按住键盘上的Shift键）来慢速移动。

what happens

在magicBridge.py程序里，我们去掉了游戏循环里的延迟。如果加上延迟会怎么样？试试在0.1秒到2秒之间不同时间的延迟，看看这对魔法桥的可靠性有什么影响。用哪个值魔法桥最安全？

------

<u>深入代码</u>

我们先暂停几分钟，因为有一些跟函数有关的技巧还没有解释清楚。从这本书一开始，我们就在使用各种函数，`mc.postToChat()`是个函数，`mc.player.getTilePos()`也是，它们都是Minecraft API的一部分。我们也用def语句自己定义了函数，例如在冒险任务3里我们自己编写了`house()`函数。但是当当我们在代码中使用这些函数时，究竟发生了什么？

我一直都说“使用”函数，但更合适的说法应该是，当我们把`house()`或者`buildBridge()`写在代码里，就“调用”了这些函数。调用函数时究竟发生了什么呢？

在Python程序运行时，计算机会记住每个时刻正在执行的语句，就像有一只无形的手指在指着正在运行的那一行代码。一般情况下，这只手指按顺序从上往下移动。当我们使用循环时，手指会跳回到循环开始的位置再重新网下移动，重复若干次。

当使用函数时，就有一些更复杂的事情在幕后进行。Python记住隐形手指当时的位置（想象一下在程序的那个位置贴个彩色标签），然后调到函数里，比如`buildBridge()`函数。当移动到`buildBridge()`函数的结尾时，就跳回到贴标签的位置，然后继续往下移动。

在程序中使用函数的原因有很多，其中最重要的两个是：

1. 可以把大型程序分割成很多小程序。
2. 可以在同一个程序中的不同地方重复使用函数里的代码。

这两点都可以让程序变得更容易理解和修改。

------

> 定义
>
> “调用”一个函数时，Python会记住程序当前执行到的位置然后临时跳到用def语句定义函数的地方开始执行。当函数执行到末尾时，Python再跳回到执行函数前的位置。

##使用Python列表作为记忆

到目前为止，我们写过的所有程序里都是使用某种形式的变量来存储一些程序运行过程中会改变的信息。每一个变量都有名字和值，如果需要程序记住更多的信息，就要更多变量。

不过这些变量的大小都是固定的，每个只能记住一件事（一个数字或者一个字符串）。以后要写的一些程序里，需要存储数量不确定的东西。我们可能不知道程序里要存储多少个元素。

幸运的是，Python跟其他现代编程语言一样，都有列表（list）的功能，它存储的元素数量在是可变的。

“列表”是编程语言中的一种可以存储任意数量元素的变量。你可以向列表中添加新的元素，计算列表的长度，访问列表中任意位置的数据，删除列表中任意位置的数据等等。列表在存储一些相似数据时很有用，比如一些需要排序的数字，一个用户组的成员，或者Minecraft游戏里的一些方块。

###列表实验

理解列表最好的办法是在Python Shelll里试验一下。

> 提示
>
> 注意这一节里的括号。这里用到了两种类型的括号，圆括号()和方括号[]。别担心，两种括号的不同含义在本节中会解释清楚。

1. 点击选中Python Shelll窗口。在最后一个>>>提示符后面点击鼠标，我们要在这里开始输入代码。别忘记要先从Python Shelll菜单里选择Shelll->Restart Shelll或者按Ctrl+C停止正在运行的程序。如果之前的程序还在运行，后面的操作是不能生效的。

2. 创建一个新的空列表，然后查看一些里面的内容：

    ```
    a = [] # an empty list
    print(a)
    ```

3. 往列表里添加一个新的项目，再次打印列表的内容：

    ```
    a.append("hello")
    print(a)
    ```

4. 往列表里添加第二个项目，再次打印列表的内容：

    ```
    a.append("minecraft")
    print(a)
    ```

5. 查看列表的长度：

    ```
    print(len(a))
    ```

6. 查看列表中某个位置的内容。这里的位置有称作“下标（index）”：

    ```
    print(a[0]) # the [0] is the index of the first item
    print(a[1]) # the [1] is the index of the second item
    ```

7. 从列表的末尾删除一个元素，打印这个元素和列表剩下的内容：

    ```
    word = a.pop()
    print(word)
    print(a)
    ```

8. 查看列表的长度和字符串`word`的长度：

    ```
    print(len(a))
    print(len(word))
    ```

9. 删除列表中剩下的一个元素，打印元素和列表的内容，以及列表的长度：

    ```
    word = a.pop()
    print(word)
    print(a)
    print(len(a))
    ```

图3-4展示了这些实验在Python Shelll里输出的信息。

> 定义
>
> “下标”是一个用来表示访问列表中某个元素的数字。我们用方括号来指定下标，比如`a[0]`表示列表的第一个元素，`a[1]`表示第二个元素。Python里的下标总是从0开始，所以0一定是第一个元素。在刚才做的实验里面，我们使用了下标来访问列表。

what happens

当我们向列表中添加元素，或者从里面删除元素时，列表的长度也会发生变化。如果尝试访问一个列表中不存在的项目会怎样？在Python Shelll里试试下面代码看会发生什么：

```
b = []
print(b[26])
```

怎么才能防止自己的程序中出现这种错误呢？（注意答案不只一种。你可以自己上网查找不同的解决方式。）

> david says
>
> 列表是一种用途广泛的变量类型，它可以用来存储任意类型的元素。它并不是唯一可以存储多个元素的变量类型。目前，我们最需要关注的是，在运行程序之前并不需要知道列表有多大。列表的长度会在我们用`append()`添加元素和用`pop()`删除元素的时候自动改变。

> 定义
>
> 从列表中弹出（pop—）一个元素时，会删除列表的最后一个元素。

###用列表建造会消失的桥

现在我们用刚刚学到的列表来写一个魔法桥建造程序，在这个程序里，魔法桥在玩家角色的双脚踏上安全的土地之后就会消失。这个程序跟magicBridge.py类似，所以你可以直接把那个程序另存为一个新文件然后再进行修改。不过这里为了解释方便，还是会给出完成的程序。如果不想输入太多代码，也可以直接从magicBridge.py里复制和粘贴过来。

1. 从IDLE菜单中选择File->New File新建一个文件，选择File->Save As保存为vanishingBridge.py。

2. 导入需要的模块：

    ```
    import mcpi.minecraft as minecraft
    import mcpi.block as block
    import time
    ```

3. 连接到Minecraft游戏：

    ```
    mc = minecraft.Minecraft.create()
    ```

4. 创建一个名为`bridge`的空列表。刚开始的时候还没有桥还没有建造，所以列表总是空的：

    ```
    bridge = []
    ```

5. 定义一个用来建造魔法桥的`buildBridge()`函数。在这个冒险任务的最后一个程序里，还要用到`buildBridge()`函数。所以请确保函数的名字是对的。函数的前面部分代码跟magicBridge.py里基本相同，所以可以直接复制过来节省输入的时间。注意缩进：

    ```
    def buildBridge():
        pos = mc.player.getTilePos()
        b = mc.getBlock(pos.x, pos.y-1, pos.z)
        if b == block.AIR.id or b == block.WATER_FLOWING.id or b == block.WATER_STATIONARY.id:
            mc.setBlock(pos.x, pos.y-1, pos.z, block.GLASS.id)
    ```

6. 当我们给桥添加了一个方块之后，需要记住这个方块的位置，这样当玩家走到安全的地方之后才能删除掉这个方块。我们用另外一个列表把坐标的三个部分组合在一起，再把这个列表加入到`bridge`列表中。这些代码的含义会在“深入代码”环节进行解释：

    ```
            coordinate = [pos.x, pos.y-1, pos.z]
            bridge.append(coordinate)
    ```

7. 要让魔法桥能在玩家离开之后自动消失，我们需要一个`else`语句来检查角色是不是还站在玻璃上。如果不是，我们的程序就要开始删除组成魔法桥的方块了。程序必须检查是不是还有魔法桥的部分方块没有删除，不然当你试图从空列表里弹出元素的时候，会产生一个错误。代码中的`elif`是“else if”的简写，“!=”的意思是“不等于”。注意缩进，`elif`需要缩进一级，因为它是`buildBridge()`函数的一部分。下面一个if需要缩进两级，因为它是elif的一部分：

    ```
        elif b != block.GLASS.id:
            if len(bridge) > 0:
    ```

8. 接下来的代码要缩进三级，因为他们是`buildBridge()`函数里、`elif`下的`if`语句的一部分！还记得之前我们把三个坐标分量组成的列表追加到列表bridge里了吗？这里我们要用下表访问坐标的列表，`coordinate[0]`表示x，`coordinate[1]`表示y，`coordinate[2]`表示z。添加`time.sleep()`可以让桥消失的慢一些，这样我们才能看清消失的过程：

    ```
                coordinate = bridge.pop()
                mc.setBlock(coordinate[0], coordinate[1], 
                coordinate[2], block.AIR.id)
                time.sleep(0.25)
    ```

9. 最后，加上游戏主循环。像之前做过的试验一样，我们可以在主循环里尝试不同的延迟时间来改进程序的可用性。记住，游戏主循环是主程序的一部分（`while True:`不需要缩进），注意检查缩进：

    ```
    while True:
        time.sleep(0.25)
        buildBridge()
    ```

从菜单中选择File->Save保存程序，选择Run->Run Module运行程序。操纵游戏角色在Minecraft世界里四处走走，让他离开岸边走到湖里，再转身回到岸上。看看发生了什么？像图4-4里那样，角色回到岸上之后桥开始消失了。

------

<u>深入代码</u>

之前在Python Shelll里进行列表试验的时候，我们向列表中添加的是字符串。在vanishingBridge.py这个程序里，有一些新的东西需要解释一下。

Python的列表里可以存放任何类型的数据，例如字符串、数字，甚至是列表。我们在之前的程序里已经用到了这个技巧，也许你还没意识到。

下面的代码创建一个包含三个元素的列表（一个方块的x、y、z坐标）：

把一些元素放在[]里，就可以创建已经包含元素的列表，而不是空列表。你也可以用下面的方法达到同样的效果：

```
coordinate = []
coordinate.append(pos.x)
coordinate.append(pos.y-1)
coordinate.append(pos.z)
```

之后当程序取出一个坐标来删除方块的时候，就从`bridge`列表中弹出一个坐标的列表：

```
coordinate = bridge.pop()
```

变量`coordinate`里的列表包含三个元素：`coordinate[0]`是方块的x坐标；`coordinate[1]`是y坐标；`coordinate[2]`是z坐标。

这种“列表的列表”看起来就像图4-5里一样。

------

> david says
>
> 实际上Python有多个不同的变量类型都可以存储多个元素，除了我们已经用过的列表之外。专业的Python程序员可能会在这个程序里使用“元组（tuple）”来存储坐标。我不想在这里一次介绍太多新概念。如果有兴趣，可以在网上搜索一下关于元组的信息，看看它和列表有什么区别，以及在Python程序中使用元组有哪些额外的好处。

##感知一个方块被击中

在这个冒险任务里我们要用到的最后一个感知的技巧，是感知玩家对方块的击打。有了这个能力，就可以自己制作一些很有趣的游戏或者程序，因为这样玩家就可以跟Minecraft世界中的每一个方块进行交互了。

新建一个程序，我们先写一个完全独立的试验性程序，之后会把它加入到这个冒险任务最终的游戏里。

1. 在IDLE编辑器菜单里选择File->New File新建一个文件。点击File->Save As保存文件并命名为blockHit.py。

2. 导入需要的模块：

    ```
    import mcpi.minecraft as minecraft
    import mcpi.block as block
    import time
    ```

3. 连接到Minecraft游戏：

    ```
    mc = minecraft.Minecraft.create()
    ```

4. 获得游戏角色的坐标，然后稍微往旁边移动一点。用这里的坐标作为创建钻石——我们的宝藏——的坐标：

    ```
    diamond_pos = mc.player.getTilePos()
    diamond_pos.x = diamond_pos.x + 1
    mc.setBlock(diamond_pos.x, diamond_pos.y, diamond_pos.z, block.DIAMOND_BLOCK.id)
    ```

5. 定义一个`checkHit()`函数。在最终的程序里我们还会用到这个函数，请确保明明正确：

    ```
    def checkHit():
    ```

6. 使用Minecraft API获得一个最近发生事件的列表。得到的是一个普通的Python列表，就像在vanishingBridge.py里用过的一样：

    ```
        events = mc.events.pollBlockHits()
    ```

7. 一次处理列表中的每个事件，使用`for`循环。关于`for`循环的这种新用法，详细的解释在后面的“深入代码”环节：

    ```
        for e in events:
            pos = e.pos
    ```

8. 检查玩家击中的方块的坐标是不是等于钻石的坐标。如果是，就在Minecraft聊天栏里发送一条消息：

    ```
            if pos.x == diamond_pos.x and pos.y == diamond_pos.y and pos.z == diamond_pos.z:
                mc.postToChat("HIT")
    ```

9. 最后，编写游戏循环。这里我们使用1秒钟延迟，来避免发送消息过快，不过你可能需要试验一下不同的延迟时间来获得最佳的效果。仔细检查缩进：

    ```
    while True:
        time.sleep(1)
        checkHit()
    ```

在菜单中选择File->Save As保存程序，然后点击Run->Run Module运行程序。

操纵游戏角色走到钻石的旁边。然后用剑对钻石的每一面都击打一下。会发生什么？就像图4-6里一样，当击中钻石的时候，Minecraft聊天栏里会出现“HIT”的消息。

> 提醒
>
> 如果你不小心用错了鼠标按键，打碎了宝藏，就永远不可能击中它了！这时候就需要重新运行程序建造一个新的“宝藏”，或者在同样的位置手动建造一个，这样才能击中它。这是因为空气（AIR）方块不会触发“击中”事件。

> 挑战
>
> 修改blockHit.py程序，在for循环中读取事件的e.face变量。方块的不同面被击中时，e.face变量的值也不同。先在代码里把e.face打印到Minecraft聊天栏，然后击打方块的每一面看它们对应的数值是多少。最后，修改程序，在击中钻石方块每一面的时候显示不同的消息。

------

<u>深入代码</u>

在方块击中检测程序里，我们使用了一个`for`循环，不过这种形式以前还没有用过。这是Python里的一个很强大的功能，我们来仔细研究一下。

我们之前用过的`for`循环是下面这样的：

```
for i in range(6):
    print(i)
```

`range(6)`实际上是一个产生数字列表[0, 1, 2, 3, 4, 5]的函数。用下面代码可以验证这一点：

```
print(range(6))
```

你会看到下面的输出：

```
[0, 1, 2, 3, 4, 5]
```

是不是看起来有点眼熟？事实上，`range()`函数做得就是生成一个数字列表。

Python里的`for`/`in`语句会在列表中的所有元素上进行循环，先把列表中的第一个元素存入循环控制变量（前面例子里的`i`），执行一次循环体，然后再把下一个元素存入循环控制变量，重复这个过程一直到列表中再没有新的元素。

这表示对任何一个列表，我们都可以用同样的方式对其中的元素进行循环。在Python Shelll里试试下面代码：

```
for name in ["David", "Gail", "Janet", "Peter"]:
    print("hello " + name)
```

我们也可以对字符串中的每个字符进行循环：

```
name = "David"
for ch in name:
    print(ch)
```

------

##编写寻宝游戏

在这个冒险任务的大部分时间里，我们都在学习新的技巧，以及编写代码片段来测试和试验Minecraft里的感知功能。现在是时候把所有这些代码组合在一起变成一个完整的游戏了。我们要编写的游戏叫做“sky hunt”，在这个寻宝游戏里，玩家需要使用导航信标来找出天空中随机出现的钻石方块，并击中它们来得分。

这个游戏有点难度：每次向前移动都会在身后留下一个黄金方块，每个黄金方块会扣掉一分。如果毫无目的的四处乱跑，你的分数就会迅速下降，甚至变成负数！我们需要使用Minecraft的导航技巧来快速找到钻石方块，并且用尽可能少的移动次数走到它们旁边。

每找到一个宝藏都会得分，并且黄金尾巴会消失不见（可能会在地上留下空洞，当心不要掉下去）。

这个程序大部分是由之前写过的实验性小程序里的可重用部分组合而成的。你可以直接从其他程序里面复制粘贴代码然后在修改，这样可以节省时间。不过我还是把完整的程序都写在这里好让你知道需要哪些部分。

职业的软件工程师通常会先写一个只有`print`语句的简单框架程序，并测试验证程序的框架，然后逐渐向里面添加和测试新功能。在这一节，我们也要像真正的软件工程师一样，一步一步的编写和测试这个程序。首先，我们写出游戏循环的框架，以及一些空函数，在之后会逐渐填充这些函数。

###编写函数和主游戏循环

1. 在菜单中选择File->New File新建一个程序文件。选择File->Save As保存程序为skyHunt.py。
2. 导入需要的模块：

    ```
    import mcpi.minecraft as minecraft
    import mcpi.block as block
    import time
    import random
    ```

3. 连接到Minecraft游戏：

    ```
    mc = minecraft.Minecraft.create()
    ```

4. 设置一个变量`score`用来记录游戏过程中的得分。还需要一个`RANGE`常量来设定游戏的难度，通过设置随机宝藏出现的位置距离玩家有多远。在测试的时候先把`RANGE`设置成一个比较小的值，程序完成之后再把它改大一些。

    ```
    score = 0
    RANGE = 5
    ```

5. 因为我们要逐步编写和测试这个程序，所以要为每个功能写一个空函数。Python的函数需要至少一条语句，我们可以用`print`语句来打印一条消息。这里只是为以后要编写的代码占个位置。

    ```
    treasure_x = None # the x-coordinate of the treasure
    def placeTreasure():
        print("placeTreasure")

    def checkHit():
        print("checkHit")

    def homingBeacon():
        print("homingBeacon")

    bridge = []

    def buildBridge():
        print("buildBridge")
    ```

6. 现在来编写主游戏循环。在正式的游戏运行时，这个循环要执行的非常快（1秒钟10次），这样才能保证黄金方块能够及时出现从而实现在空中行走，不过在测试的时候我们放慢速度到没秒一次。在游戏主循环里，我们要使用空函数。

    ```
    while True:
        time.sleep(1)

        if treasure_x == None and len(bridge) == 0:
            placeTreasure()

        checkHit()
        homingBeacon()
        buildBridge()
    ```

7. 从菜单选择File->Save As保存程序，然后选择Run->Run Module运行程序。程序不应该有任何错误，虽然到目前为止它只是没秒钟在Python Shelll里打印一条消息。我们现在已经有了整个程序的框架，现在可以一点一点的添加新代码进去了。

###在空中放置宝藏

我们要写的第一个功能，是在天空中的随机位置放置宝藏。我们使用三个全局变量来保存宝藏的坐标，它们的初始值为`None`。`None`是Python里的一个特殊值，表示变量已经在内存里了，但是没有存储任何数据。我们会在游戏主循环里用这个来判断是不是需要重新生成一个宝藏。

1. 创建全局变量来跟踪宝藏的位置，添加下面粗体字的代码：

    ```
    treasure_x = None
    treasure_y = None
    treasure_z = None
    ```

2. 用下面代码填写`placeTreasure()`函数（还要删掉之前写的`print`语句）：

    ```
    def placeTreasure():
        global treasure_x, treasure_y, treasure_z
        pos = mc.player.getTilePos()
    ```

3. 用`random`函数把宝藏放在玩家角色身边`RANGE`个方块距离范围内的某个位置，y坐标要设置为角色上方的某个位置（这样很有可能是在空中）。

    ```
        treasure_x = random.randint(pos.x, pos.x+RANGE)
        treasure_y = random.randint(pos.y+2, pos.y+RANGE)
        treasure_z = random.randint(pos.z, pos.z+RANGE)
        mc.setBlock(treasure_x, treasure_y, treasure_z, block.DIAMOND_BLOCK.id)
    ```

运行程序，看看玩家附近的天空中是不是出现了一个宝藏。

###收集宝藏

现在我们要使用blockHit.py里的代码，通过一点修改来检测玩家的剑是否击中了宝藏。

1. 删除`checkHit()`函数里的`print`语句，替换成下面的代码。变量`score`和`treasure_x`必须作为全局变量列出，因为`checkHit()`函数会改变它们的值。在函数里需要修改任何全局变量的值时，都需要把它们列出来。如果不这样做，程序是不能正常运行的。

    ```
    def checkHit():
        global score
        global treasure_x
    ```

2. 检查所有方块被击中的事件，看座标是否和宝藏的位置一致：

    ```
        events = mc.events.pollBlockHits()
        for e in events:
            pos = e.pos
            if pos.x == treasure_x and pos.y == treasure_y and pos.z == treasure_z:
                mc.postToChat("HIT!")
    ```

3. 现在我们要让程序在击中方块是往变量`score`里增加分数，然后删除宝藏。最后，还要把变量`treasure_x`设置成`None`，好让`placeTreasure()`函数可以在之后创建一个新宝藏。注意缩进，这里的代码是`if`语句的一部分：

    ```
                score = score + 10
                mc.setBlock(treasure_x, treasure_y, treasure_z, block.AIR.id)
                treasure_x = None
    ```

保存和运行程序，检查击中宝藏是它是否会消失。同时也应该发现当宝藏被击中并消失之后，一个新的宝藏会在玩家附近的随机位置出现。

###添加导航信标

导航信标会每秒钟在聊天栏里显示当前的分数和宝藏的大致距离。添加方法如下：

1. 创建一个`timer`变量。由于最终的程序里，游戏循环一秒钟会运行10次，所以每计数10次就是1秒钟。变量`timer`就是用来实现这个计数的。如果改变了游戏循环的速度，就需要同时修改`TIMEOUT`的值。确保这两行代码刚好在`homingBeacon()`函数的前面（注意这里没有缩进）。

    ```
    TIMEOUT = 10
    timer = TIMEOUT
    ```

2. 删除`homingBeacon()`函数里的`print()`语句，声明`timer`为全局变量，因为这个函数会改变它的值：

    ```
    def homingBeacon():
        global timer
    ```

3. 如果`treasure_x`变量里有值得话，天空中就会有一个宝藏。这里我们必须检查当前是否有宝藏，否则即使当前没有宝藏，聊天栏里也会有导航信标消息：

    ```
        if treasure_x != None:
    ```

4. 这个函数在游戏循环里一秒钟会被调用10次，所以我们需要每10次里只显示一次消息：

    ```
            timer = timer - 1
            if timer == 0:
                timer = TIMEOUT
    ```

5. 当`timer`变量超时（每10次调用或者每1秒钟）之后，计算一个粗略的数值来表示玩家当前距离宝藏有多远。函数`abs()`可以得到两个位置之间差值的绝对值（一个正数）。把坐标三个分量的插值的绝对值加在一起，得到一个距离宝藏远时变大，近时变小的数值。注意检查缩进，这里的代码都属于上一个if语句：

    ```
        pos = mc.player.getTilePos()
        diffx = abs(pos.x - treasure_x)
        diffy = abs(pos.y - treasure_y)
        diffz = abs(pos.z - treasure_z)
        diff = diffx + diffy + diffz
        mc.postToChat  ("score:" + str(score) + " treasure:" + str(diff))
    ```

保存并运行程序，检查方向标和分数是不是正确的显示在了聊天栏里。由于我们还在测试和开发这个程序，游戏循环的运行速度最终的版本慢10倍，所以目前这些消息每10秒钟才会显示一次。默数10秒钟看一下是不是这样。我们现在还会看到一些空函数每秒钟都会向Python Shelll窗口中打印消息，因为程序还没有最终完成。

###增加桥梁建造

接下来我们要把vanishingBridge.py里的桥梁建造函数也加进来。我们只需要做一点点修改，检查玩家是不是正站在黄金方块上，如果不是，就在脚下创建一个。

1. 像下面这样编写`buildBridge()`函数。需要修改的地方用粗体字标了出来：

    ```
    bridge = []
    def buildBridge():
        global score
        pos = mc.player.getTilePos()
        b = mc.getBlock(pos.x, pos.y-1, pos.z)
        if treasure_x == None:
            if len(bridge) > 0:
                coordinate = bridge.pop()
                mc.setBlock(coordinate[0], coordinate[1], coordinate[2], block.AIR.id)
                mc.postToChat("bridge:" + str(len(bridge)))
                time.sleep(0.25)
            elif b != block.GOLD_BLOCK.id:
                mc.setBlock(pos.x, pos.y-1, pos.z, block.GOLD_BLOCK.id)
                coordinate = [pos.x, pos.y-1, pos.z]
                bridge.append(coordinate)
                score = score - 1
    ```

2. 大功告成！我们已经完成了最终的程序。游戏程序已经就绪，我们要修改游戏循环里的`time.sleep(1)`为每次延迟0.1秒。这样游戏循环每秒钟会运行10次。`homingBeacon()`函数里的`timer`变量就会计数10次，最后每秒钟会在聊天栏里显示一条消息。

保存并运行程序。检查黄金轨迹是不是会在收集到宝藏的时候自动消失，并且每使用一个黄金方块，分数都会减一。

现在可以好好享受这个游戏了！收集宝藏并获得高分是不是挺难的？


##更多的冒险任务

在这个冒险任务里，我们学会了如何使用`getBlock()`函数来感知玩家脚下的方块，如何使用`events.pollBlockHits()`在玩家用剑击中方块的某一面时做出回应。我们在Minecraft里编写了一个神奇的、完整的游戏，可以计分的！

* 把常量`RANGE`的值改大一些，让宝藏创建得离玩家更远。这会让游戏难度更高，所以最好试着再添加一个新功能，根据玩家角色和宝藏的距离在聊天栏里显示冷（cold）、暖（warm）和热（hot）。
* 设计一个更好的计分方案，在正常游戏时能够得到相应的正的分数。反复测试得出一个获得宝藏时加分、和使用一个黄金方块时扣分的最佳值。
* 在网上搜索毕达哥拉斯定理（勾股定理），看看是不是能写一段更好的程序来估计宝藏的距离，让`homingBeacon()`函数更精确。（我们在冒险任务7里会讲到一些这方面的知识，所以你可以提前看一下那一章的内容看看自己是不是能完成这个任务。）


解锁成就：违反重力定律和水上行走专家——在一个冒险任务里实现了两大奇迹！

##下一个冒险任务

在冒险任务5里，我们会学习如何突破Minecraft虚拟游戏世界的限制，把Minecraft跟真实世界的物体连接起来突破计算机的边界。我们将使用电子元件制作自己的交互式游戏控制器，以及一个新的游戏。
