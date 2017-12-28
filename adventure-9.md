#终极大冒险：Crafty Crossing

终于来到最后的大冒险了！我们要用一路学到的所有技巧来编写一个Minecraft里的迷你游戏。你会发现Minecraft的无限潜力，以及如何用获取和设置方块、获得和改变玩家坐标等简单命令来完成一些令人瞩目的事情。

我们还会学到一项新的技巧，用线程让程序同时做多件事。

这个项目还有很多扩展的可能，完成这个项目并不表示我们的冒险已经结束。相反，我们才刚刚具备了开发更复杂，更有创意和挑战性的游戏的能力。

##游戏里的游戏

我们这次要开发的游戏叫做Crafty Crossing。游戏的目标是收集钻石并在时间结束之前到达竞技场的另一边。在这个过程中会有很多令人讨厌的障碍物来减缓玩家的速度。

每收集到一颗钻石都会获得积分，最后再乘以到达另一边时时钟上剩余的秒数的到最终分数。

为了在最短的时间里穿过竞技场，玩家需要跳到河流上方的移动平台上，穿过上下移动的墙，并且躲避地面上随机出现的洞（见图9-1）。

开发一个游戏是一个重大任务。我们需要开发、测试，并最终把很多组件组合在一起才能完成最终的游戏。为了让这个项目更容易学习，我把项目指引分成了4个部分，这样我们就可以分段开发和测试程序：

* 第一部分：编写游戏主题框架，建造游戏发生的竞技场。
* 第二部分：编写阻挡和延缓玩家的障碍物。
* 第三部分：给程序加入游戏循环，创建不同的难度等级、计分以及必不可少的“Game Over(游戏结束)”。
* 第四部分：用冒险任务5里学到的技巧，重新使用按钮和7段显示器来添加一个开始按钮、一个钻石倒数计数器，以及一个提醒玩家时间流逝的提示器。

完成了这个游戏之后，你还可以按照自己的想法进行改造，加入自己的创意做出属于你自己的游戏。

> 技巧提示
>
> 这个冒险任务的每一部分都列出了一些建议的挑战任务。在完成整个冒险任务之前，你最好不要尝试这些跳着，或者修改你的代码，否则你可能会被各种复杂的情况搞得一团乱麻。等完成整个冒险任务之后再来看这些挑战。

##第一部分：建造建造竞技场

Crafty Crossing竞技场是游戏里所有活动发生的地方。玩家会从竞技场的一边开始，然后冲向另外一边——不过会有散布在竞技场里的障碍物来拖慢他的速度。

没有障碍物的竞技场是一个长方形的空间，地板是草地（`GRASS`方块），四周是玻璃（`GLASS`方块）（见图9-2）。我们用常量来定义竞技场的宽度、高度和长度（即x、y和z）。修改这些常量就可以改变竞技场的大小，障碍物会自动改变大小来适应竞技场。地板至少要有三个方块的厚度，因为障碍河会有两个方块深。

还记得冒险任务3里用过的`setBlocks()`函数吗？我们现在要用它来建造竞技场。

启动Minecraft、IDLE，如果用PC或者Mac，还要启动Bukkit服务器。到这里你应该已经很熟练了，如果需要提示的话可以参考冒险任务1。

先创建一个新程序。第一步是编写游戏的初始结构：

1. 打开IDLE，新建一个文件，保存在MyAdventures文件夹并命名为CraftyCrossing.py。
2. 导入要用的模块。这次我们要用到一个新模块thread，在第二部分编写障碍物的时候会详细介绍：

    ```
    import mcpi.minecraft as minecraft
    import mcpi.block as block
    import mcpi.minecraftstuff as minecraftstuff
    import time
    import random
    import thread
    ```

3. 定义三个常量，分别表示竞技场的宽度、高度和长度：

    ```
    ARENAX = 10
    ARENAZ = 20
    ARENAY = 3
    ```

4. 加入程序需要用到的函数。我们在后面会逐步填写这些函数，直到完成整个游戏：

    ````
    def createArena(pos):
        pass

    def theWall(arenaPos, wallZPos):
        pass

    def theRiver(arenaPos, riverZPos):
        pass

    def theHoles(arenaPos, holesZPos):
        pass

    def createDiamonds(arenaPos, number):
        pass
    ````

    `pass`语句不做任何事情，不过可以用来作为占位符暂时替代以后将要编写的代码。


5. 连接到Minecraft游戏：

    ```
    mc = minecraft.Minecraft.create()
    ```

6. 定义一个布尔型变量，当游戏结束的时候会被设为`True`，不过在刚开始的时候是`False`：

    ```
    gameOver = False
    ```

在后面，我们会逐步添加主程序的代码并完成上面定义的这些函数。

> martin says
>
> 这个程序现在就可以运行，但是什么也不会发生！不过还是建议运行一下现在的程序，如果Python Shell里没有显示任何错误，我们就能确定现在写的部分都是正确的。

下一步是编写`createArea()`函数，用来在Minecraft里建造竞技场：

1. 先在刚才编写的代码里找到`createArea()`函数，也就是下面几行：

    ```
    def createArena(pos):
        pass
    ```

    ​删除掉`pass`语句。


2. 在`def createArena(pos):`一行后面缩进一级，连接到Minecraft：

    ```
        mc = minecraft.Minecraft.create()
    ```

3. 现在我们可以用传给函数的`pos`参数和常量`ARENAX`、`ARENAY`、`ARENAZ`来计算竞技场的边界坐标，然后用`setBlocks()`函数建造竞技场（图9-3显示了如何计算竞技场坐标）：

    ```
        mc.setBlocks(pos.x - 1 , pos.y, pos.z - 1,
        pos.x + ARENAX + 1, pos.y - 3,
        pos.z + ARENAZ + 1,
        block.GRASS.id)
    ```

4. 接下来建造玻璃墙，先创建一个玻璃立方体，然后再在里面创建一个空气立方体来清空竞技场的内部：

    ```
        mc.setBlocks(pos.x - 1, pos.y + 1, pos.z - 1, pos.x + ARENAX + 1, pos.y + ARENAY, pos.z + ARENAZ + 1, block.GLASS.id)
        mc.setBlocks(pos.x, pos.y + 1, pos.z, pos.x + ARENAX, pos.y + ARENAY, pos.z + ARENAZ, block.AIR.id)
    ```

5. 函数`createArea()`到这里就完成了，不过还需要在主程序里调用它。把下面几行代码加到程序的末尾，用来获取玩家坐标并用它作参数调用`createArea()`函数：

    ```
        arenaPos = mc.player.getTilePos()
        createArena(arenaPos)
    ```

6. 现在该运行程序了！我们应该能看到竞技场出现在玩家身边，像图9-3那样。

##第二部分——生成障碍物

富有挑战性是好游戏的一个重要方面——简单的游戏很快就会变得无聊。在接下来的部分，我们要生成一些障碍物来阻碍玩家，给他的游戏过程增加一些难度。

###墙

我们要创建的第一个障碍是一堵转墙，它贯穿整个竞技场并且会上下移动。当它落下时，会挡住去路，玩家必须等它重新升起才能通过（如图9-4）。

用墙来阻挡玩家是一种简单而有效的方式，不过如果玩家能把握好时间的话，他就可以直接从下面穿过，进入下一个挑战。

我们要用minecraftstuff模块里的`MinecraftShape`来创建砖墙，在冒险任务8外星入侵里已经介绍过了。

按照下面步骤修改`theWall()`函数：

1. 在之前编写的代码中找到下面几行：

    ```
    def theWall(arenaPos, wallZPos):
        pass
    ```

    删除`pass`语句。

2. 在第一行的下面缩进一级，连接到Minecraft：

    ```
        mc = minecraft.Minecraft.create()
    ```

3. `theWall()`函数需要两个参数：`areaPos`（竞技场的位置）和`wallZPos`（墙在竞技场Z方向上的位置）。用这两个参数可以计算出墙的位置（见图9-5）：

  ```
        wallPos = minecraft.Vec3(arenaPos.x, arenaPos.y + 1, arenaPos.z + wallZPos)
  ```

4. 我们现在要用两个`for`循环来创建组成墙的方块——一个沿着竞技场x方向生成方块，另一个向上方（y方向）生成方块——并把它们加入列表`wallBlocks`：

    ```
        wallBlocks = []
        for x in range(0, ARENAX + 1):
            for y in range(1, ARENAY):
                wallBlocks.append(minecraftstuff.ShapeBlock(x, y, 0, block.BRICK_BLOCK.id))
    ```

5. 用第3步和第4步中的`wallPos`和`wallBlocks`作为参数生成墙的形体：

    ```
        wallShape = minecraftstuff.MinecraftShape(mc, wallPos, wallBlocks)
    ```

6. 墙已经造好了——不过它还是静止的！要让它上下移动起来，我们要用MinecraftShap里的`moveby()`函数，并在中间加入一小段延迟：

    ```
        while not gameOver:
            wallShape.moveBy(0,1,0)
            time.sleep(1)
            wallShape.moveBy(0,-1,0)
            time.sleep(1)
    ```

    `while`循环里的代码会一直运行直到变量`gameOver`被设为`True`，也就是玩家完成游戏或者输掉游戏的时候。

7. `theWall()`函数到这里就完成了。剩下要做的就是在主程序里调用它。在程序末尾添加下面几行：

    ```
    WALLZ = 10
    theWall(arenaPos, WALLZ)
    ```

    常量`wallZ`保存了墙的z坐标，也就是墙在竞技场里的位置。

8. 运行程序。现在应该能看到一堵砖墙在竞技场的中间上下移动。

> 提醒
>
> 我们还没有写把`gameOver`设为`True`的代码，所以程序一运行起来就不会停止！我们需要在Python Shell里点击Shell->Restart Shell来停止程序。

------

<u>深入代码</u>

生成墙里的方块时，我们用了两个`for`循环——其中一个在另一个里面。这是一种很有用的编程技巧，称作嵌套。我们也可以说这两个循环是嵌套的。第一个for循环对竞技场宽度方向（x）上的每个方块循环一次。第二个循环也是类似的，不过是对竞技场高度方向（y）上每个方块进行循环。

变量`x`和`y`用来为墙里的每个方块创建一个`ShapeBlock`对象：`minecraftstuff.ShapeBlock(x,y,0,block.BRICK_BLOCK.id)`。

我们使用for循环和常量`ARENAX`、`ARENAY`，这样即使竞技场的大小改变了，我们建造的墙仍然会贯穿整个竞技场。

------



###运行多个障碍物

现在我们有麻烦了！我们编写的程序卡住了。它会一直循环，让砖墙一直上下移动，但是不会再做任何其他事情。这是因为我们编写的程序是顺序的，也就是说程序里的每条指令依次执行，下一跳指令必须等到上一条完成才会开始执行。我们的程序卡住了，因为它永远执行不到`while`循环后面的指令。

> 定义
>
> **顺序**是指按照一定的序列或顺序，通常是一件事一件事的进行。

如果程序卡在这里，只会让墙上下移动，我们怎么才能生成更多障碍物，并运行游戏其他部分呢？

有一个办法，需要用到多线程！到目前为止，我们编写过的所有程序都是单线程的。换句话说，它们都是顺序的，指令一条接一条的执行。

想象一下，我们的程序就像一条放平的项链，指令就像项链上的珍珠。程序从项链的一端运行到另一端，碰到一颗珍珠就执行一条指令。如果有循环，就会让程序回到之前的某个珍珠；如果有if语句，程序就可能错过某个珍珠——但是它一次只能执行一条指令。

使用多线程时，我们告诉程序要创建一个新的线程（或者“项链”），这个线程有它自己的指令（“珍珠”）。它会跟原来的程序同时运行。我们的程序现在就可以同时做两件事，而不是一件（见图9-6）。

要让游戏里的所有障碍物同时运行，我们需要给每个障碍物创建一个线程，也就是说让这些障碍物工作的程序在同时运行。

> martin says
>
> 多线程在计算机编程里面非常的有用，不过它属于高级功能，会变得很复杂。如果想了解更多Python多线程编程的知识，可以看这个[网页](www.tutorialspoint.com/python/python_multithreading.htm)。

我们回到刚才的砖墙。要让它在自己的线程里运行，我们需要改变调用theWall()函数的那一行代码，使用`thread.start_new_thread()`函数：

1. 删除程序最后一行，调用`theWall()`函数的代码，也就是下面这一行：

    ```
    theWall(arenaPos, WALLZ)
    ```

2. 把下面一行加入程序末尾，它还是调用`theWall()`函数，不过这次会启动一个新的线程来运行：

    ```
    thread.start_new_thread(theWall, (arenaPos, WALLZ))
    ```

3. 运行程序。

程序运行的结果跟之前一样，也是一堵砖墙上下移动。不同的是，这次控制墙的程序运行在自己的线程里。现在我们可以继续编写程序的剩余部分了。

> 提醒
>
> 如果要停止IDLE中运行的一个多线程程序，需要在Python Shell里点击Shell->Restart Shell。如果你在Python Shell里按Ctrl + C，只能停止主程序，不能停止其他线程，所以要点击Shell->Restart Shell来停止主程序和其他所有线程。

------

<u>深入代码</u>

我们用语句`thread.start_new_thread(theWall, (arena, WALLZ))`让`theWall()`函数在独立线程中运行。第一个参数是函数的名字——`theWall`——第二个参数保存了函数需要的参数——`arena`，`WALLZ`。

传递给`theWall()`函数的参数需要写在括号里面：` (arena, WALLZ)`，因为`start_new_thread`需要以元组（tuple）的形式传递它们。

任何Python函数都可以用这种方式来调用，比如下面这个在屏幕上打印消息的函数：

```
def printMessage(message):
    print message
```

我们也可以在独立线程里运行它：

```
thread.start_new_thread(printMessage, ("Hello Minecraft World",))
```

"Hello Minecraft World"后面的逗号告诉Python这是个元组，不过里面只有一个成员。

------

> 定义
>
> **元组**类似于在之前的冒险任务里用过的列表——最主要的区别是元组一旦被创建就不能再修改，而列表则可以增删元素进行修改。在编程术语里，不能修改的东西被称作“不可变（immutable）”，而可以修改的则叫做“可变（mutable）”。如果想了解更多关于Python元组的知识，请访问[这个网页](www.tutorialspoint.com/python/python_tuples.htm)。

###建造河流

我们的下一个任务是挖一条跟竞技场一样宽的河。河两岸之间的距离要大到让玩家无法跳过。幸好河上还有一座浮桥。不幸的是，浮桥会在河上前后移动，所以玩家必须小心的计算上桥和下桥的时间（见图9-7）。

如果玩家掉进了河里，就会回到起始点。我们会在第三部分编写把玩家送到起始点的代码，这一节的代码只是关于建造河流和移动浮桥的。

首先我们要清除掉竞技场地板上的部分方块，并在里面填上水方块，从而创造出河流。我们用`MinecraftShape`创建浮桥，并用跟砖墙一样的方法让它运动起来。

按照下面步骤修改`theRiver()`函数来生成河流：

1. 从之前编写的代码中找到`theRiver()`函数，也就是下面几行：

    ```
    def theRiver(arenaPos, riverZPos):
        pass
    ```

    删除`pass`语句。

2. 在第一行之后缩进一级，连接到Minecraft：

    ```
        mc = minecraft.Minecraft.create()
    ```

3. 定义两个常量，分别表示河面的宽度和浮桥的宽度：

    ```
        RIVERWIDTH = 4
        BRIDGEWIDTH = 2
    ```

4. 用传递给函数的参数`arenaPos`和`riverZPos`（河流在竞技场Z方向上的位置）来生成河流：

    ```
        mc.setBlocks(arenaPos.x, 
                     arenaPos.y - 2, 
                     arenaPos.z + riverZPos,
                     arenaPos.x + ARENAX, 
                     arenaPos.y, 
                     arenaPos.z + riverZPos + RIVERWIDTH - 1,
                     block.AIR.id)

        mc.setBlocks(arenaPos.x, 
                     arenaPos.y - 2, 
                     arenaPos.z + riverZPos,
                     arenaPos.x + ARENAX, 
                     arenaPos.y - 2, 
                     arenaPos.z + riverZPos + RIVERWIDTH - 1,
                     block.WATER.id)
    ```

    我们用`setBlocks()`在竞技场地板上创建一个空气（`AIR`）区域，然后在这个区域的底部铺上一层水（`WATER`）。

5. 生成浮桥位置的坐标。我们要把它放在河中间，这样玩家就不能直接走上去，必须跳上去：

    ```
        bridgePos = minecraft.Vec3(arenaPos.x, arenaPos.y, 
                                   arenaPos.z + riverZPos + 1)
    ```

6. 生成浮桥的形体。方法跟墙类似，用两层嵌套循环，一个是浮桥宽度（x）方向上的，另一个是河面宽度（z）方向上的：

    ```
        bridgeBlocks = []
        for x in range(0, BRIDGEWIDTH):
        for z in range(0, RIVERWIDTH - 2):
            bridgeBlocks.append(minecraftstuff.ShapeBlock(x,
                                                          0,
                                                          z,
                                      block.WOOD_PLANKS.id))
    ```

    生成浮桥的时候，要从`RIVERWIDTH`里减去2，让浮桥和两边河岸之间各有一个方块的距离，这样玩家必须跳跃才能到桥上或者从桥上下来（见图9-8）。

7. 用步骤5和6里产生的`bridgePos`和`bridgeBlocks`做参数来生成浮桥：

    ```
        bridgeShape = minecraftstuff.MinecraftShape( 
          mc, bridgePos, bridgeBlocks)
    ```

8. 浮桥在河面上每次移动一个方块的距离，我们要计算浮桥从竞技场的一边移动到另外一边的步数，也就是竞技场的宽度减去浮桥的宽度：

    ```
        steps = ARENAX - BRIDGEWIDTH
    ```

9. 用两个for循环让浮桥移动起来（一个向左移动，另一个向右移动），用`MinecraftShape.moveBy()`函数让浮桥每次向当前方向移动一个方块的距离，每一步之间要有一个小的延迟：

    ```
        while not gameOver:
            for left in range(0, steps):
                bridgeShape.moveBy(1,0,0)
                time.sleep(1)
            for right in range(0, steps):
                bridgeShape.moveBy(-1,0,0)
                time.sleep(1)
    ```

10. `theRiver()`函数到这里就写完了。我们还需要在主函数里用一个新线程来调用它。把下面几行代码添加到程序的末尾：

  ```
  RIVERZ = 4
  thread.start_new_thread(theRiver, (arenaPos, RIVERZ))
  ```

11. 下面就该运行程序了！我们现在应该能看到竞技场的中间有一堵砖墙，靠近起点的一侧有一条河，一座浮桥在河面上左右移动。

> martin says
>
> 进入竞技场，试着穿过浮桥。如果控制得好，你会觉得这很简单——不过等你需要同时收集钻石，并且要越快越好的时候，就会有压力了。那时候跳跃失误的机会就大多了。

###陷阱

我们给Crafty Crossing创造的最后一种障碍物是陷阱。它们在竞技场的地板上随机出现，过一会儿就关闭，然后不同的地方重新出现。

我们要用`randint()`函数来获得这些陷阱的随机位置。在陷阱打开之前，地板上会短暂的出现黑色羊毛方块（`BLACKWOOL`），给玩家一些警告，让玩家有机会绕过去。我们通过把地板上的方块变成空气（`AIR`）来创造陷阱。它们会保持几秒钟，然后重新变回草地方块，之后新的陷阱在其他地方出现。

跟河一样，如果玩家掉进陷阱里，就会回到竞技场的起点，不过我们要到第三部分才会加入这段代码。

现在我们要修改`theHoles()`函数来创造陷阱：

1. 在之前编写的代码里找到`theHoles()`函数，也就是下面几行：

    ```
    def theHoles(arenaPos, holesZPos):
        pass
    ```

    删掉`pass`语句。

2. 在第一行之后缩进一级，加入下面代码连接到Minecraft：

    ```
        mc = minecraft.Minecraft.create()
    ```

3. 定义两个常量，分别表示陷阱的数量（`HOLES`）和宽度（`HOLESWIDTH`），见图9-10：

    ```
    HOLES = 15
    HOLESWIDTH = 3
    ```

4. 编写`while`循环，它会一直执行到游戏结束：

    ```
        while not gameOver:
    ```

    `theHoles()`函数里剩下的代码都要在`while`循环下缩进一级。

5. 用`random.randint()`函数为陷阱生成随机的x和z坐标（y坐标就是竞技场的y坐标），把它们放到一个列表里：

    ```
            holes = []
            for count in range(0,HOLES):
                x = random.randint(arenaPos.x, 
                                   arenaPos.x + ARENAX)
                z = random.randint(arenaPos.z + holesZPos, 
                                   arenaPos.z + holesZPos + HOLESWIDTH)
                holes.append(minecraft.Vec3(x, arenaPos.y, z))
    ```

6. 循环遍历陷阱列表里的所有位置，把所有的方块都变成黑色羊毛（`BLACK.WOOL`）：

    ```
            for hole in holes:
                mc.setBlock(hole.x, hole.y, hole.z,  
                            block.WOOL.id, 15)
            time.sleep(0.25)
    ```

    把陷阱位置的地板变成黑色，警告玩家这里马上会有一个陷阱，让他们赶紧离开。

7. 用`setBlocks()`把陷阱坐标下方的方块设为空气，打开陷阱：

    ```
            for hole in holes:
                mc.setBlocks(hole.x, hole.y, hole.z,
                             hole.x, hole.y - 2, hole.z,
                             block.AIR.id)
            time.sleep(2)
    ```

    陷阱打开之后，要加入一个延迟，让陷阱保持2秒钟。

8. 用同样循环再把陷阱关闭，不过这次要把方块设置成草地：

    ```
            for hole in holes:
                mc.setBlocks(hole.x, hole.y, hole.z,
                             hole.x, hole.y - 2, hole.z,
                             block.GRASS.id)
            time.sleep(2)
    ```

    程序执行到这里就会返回`while`循环的开始，重新生成一组陷阱。

9. `theHoles()`函数到这里就写完了。在程序末尾添加下面几行：

    ```
    HOLESZ = 15
    thread.start_new_thread(theHoles, (arenaPos, HOLESZ))
    ```

    常量`HOLESZ`是陷阱在竞技场里z方向上的坐标。

10. 运行程序。跟以前一样，我们会看到一个带有砖墙和河流的竞技场。不同的是，在靠近竞技场远端的一边还会有一些陷阱不停的在随机位置出现和消失。

进入竞技场试一下，看看你能不能在里面来回穿梭而不掉下去或者被卡住。

> martin says
>
> 你可以调整程序里的常量让它变得个性化。比如增加竞技场的长度或者宽度，把机关放在不同位置，或者让机关移动更快、更难通过来增加难度。

------

<u>深入代码</u>

生成机关的函数类似于迷你程序，因为使用了多线程，所以它们都独立运行。因此想要在竞技场里加入多个机关是很简单的。或者你想要两堵砖墙？只要再调用theWall()函数一次，并给它一个不同的z坐标，就可以生成另外一面砖墙：

```
WALL2Z = 13
thread.start_new_thread(theWall, (arenaPos, WALL2Z))
```

------



> 挑战
>
> 我们可以做的还不止于此。用同样的方法，你能创造一种新的机关吗？比如可以随机的变出一个笼子来困住玩家，或者增加一些平台，让玩家必须跳过它们才能到达竞技场的远端。

> 提示
>
> 如果觉得这些机关太难或者太简单，可以调整常量的值来改变难度。比如，我们可以增加或者减少延迟时间让机关移动的更慢或者更快。要加速浮桥，把处理移动的for循环里的time.sleep(1)改成time.sleep(0.5)。

##第三部分——游戏规则

下一部分是给游戏添加游戏规则。我们的目标是把竞技场从一堆机关的组合变成一个让玩家反复去玩，并且进入新的关卡。

要做到这一点，游戏需要变得更刺激更有挑战，并且要有奖励和目标。

给玩家的挑战是收集随机出现在竞技场里的所有钻石，同时还要在一定的时间限制内穿越所有机关。收集到钻石和通过关卡都会有分数奖励。玩家通过关卡的速度越快，获得的分数就越多。目标是完成所有关卡并得到尽可能多的分数。

我们的游戏会有三个关卡，每一个关卡都比前一关更难：有更多钻石，并且时限更短。

###开始游戏

这一件我们要建立其游戏的框架，定义表示每个关卡的钻石数量和可用时间的常量。

程序有两个主要循环：

* 游戏循环：这个循环一直运行到游戏结束（`while not gameOver`）。每个关卡都是在这里建立并开始。关卡结束后还要计算分数。
* 关卡循环：这个循环运行到关卡结束——也就说，直到通过关卡或者游戏结束（`while not gameOver and not levelComplete`）。这个循环里还会把掉进河里或者陷阱的玩家送回起点，清除掉玩家击中的钻石，检查时间是否已经用完。

> 提醒
>
> 这一节的内容里会提到把代码在游戏循环或者关卡循环下缩进一级。把这些代码放在正确的位置非常重要，否则程序就不能运行。

首先，按照下面步骤在程序末尾添加必要的代码来确定游戏的结构，生成两个主循环：

1. 定义三个常量分别表示关卡的数量，每个关卡的钻石数量和玩家过关时间限制（以秒为单位）：

    ```
    LEVELS = 3 
    DIAMONDS = [3,5,9]
    TIMEOUTS = [30,25,20]
    ```

    常量`DIAMONDS`和`TIMEOUTS`是列表。它们都有三个成员，分别对应三个关卡。举个例子，在第一个关卡，玩家需要收集3个钻石并在30秒以内到达竞技场的另一边。

2. 定义两个变量来保存玩家的得分和当前的关卡：

    ```
    level = 0
    points = 0
    ```

3. 开始游戏循环。在上面加一行注释用作提醒，以为后面的操作步骤里会用到“在游戏循环下缩进一级”这种说法：

    ```
    #game loop
    while not gameOver:
    ```

    变量`gameOver`的值在程序开始时被设为`False`。当玩家时间用完或者完成游戏之后，会被改成`True`。这个变量在机关函数里也用到了，当它变成`True`的时候，所有机关都会停止。

4. 在游戏循环下缩进一级，改变玩家的坐标，让他出现在竞技场的起点位置准备游戏：

    ```
        mc.player.setPos(arenaPos.x + 1, arenaPos.y + 1,  
                         arenaPos.z + 1)
    ```

5. 获得当前时间并保存在一个变量里，以便计时：

    ```
        start = time.time()
    ```

6. 把关卡通过的标志设为`False`并开始关卡循环。更游戏循环一样，在这里加入加一条注释作为提示，后面的步骤中会用到“在关卡循环下缩进一级”的说法：

    ```
        levelComplete = False
        #level loop
        while not gameOver and not levelComplete:
    ```

7. 在关卡循环下缩进一级，加入一个小的延迟。因为在玩游戏的时候程序会一直运行，没有这个延迟，程序就可能会耗尽计算机的所有计算能力：

    ```
            time.sleep(0.1)
    ```

8. 运行程序。我们能看到的唯一变化是玩家被自动放在了竞技场的起点，尽管如此，这还是能帮助我们检验程序是否都能正常运行，确保没有错误发生。

> 挑战
>
> 玩家被放在竞技场的起点，从靠右手边的角落开始游戏。如果从中间开始会不会更好？修改代码，在开始关卡的饿时候把玩家放在竞技场的中间而不是角落。

###收集钻石

游戏的主要目标是收集钻石。我们现在要编写代码让钻石出现在竞技场里的随机位置（见图9-11）。玩家要通过“击中”（也就是拿着剑的时候右键点击）钻石来进行收集。

钻石被击中之后就会消失，玩家收集完所有钻石之后，就可以继续前进到竞技场的另一端来完成关卡。

按照下面步骤修改`createDiamonds()`函数并在游戏循环里调用：

1. 在之前写的代码里找到`createDiamonds()`函数，也就是下面几行：

    ```
    def createDiamonds(arenaPos, number):
        pass
    ```
    删掉`pass`语句。

2. 在第一行下面缩进一级，连接到Minecraft：

    ```
        mc = minecraft.Minecraft.create()
    ```

3. 把竞技场里随机x和z坐标处的方块设成钻石方块（`DIAMOND_BLOCK`），生成一定数量的钻石：

    ```
        for diamond in range(0, number):
            x = random.randint(arenaPos.x, arenaPos.x + ARENAX)
            z = random.randint(arenaPos.z, arenaPos.z + ARENAZ)
            mc.setBlock(x, arenaPos.y + 1, z,  
                        block.DIAMOND_BLOCK.id)
    ```

4. `createDiamonds()`函数需要在游戏循环开始的时候被调用，每次开始一个新的关卡，就要生成一些新的钻石。在游戏循环`while`语句的下面，缩进一级，添加下面代码：

    ```
    #game loop
    while not gameOver:
        # 添加下面两行
        createDiamonds(arenaPos, DIAMONDS[level])
        diamondsLeft = DIAMONDS[level]
    ```

    这段代码同时会定义一个变量`diamondLeft`，用来记录玩家还剩下多少个钻石需要收集。

5. 运行程序。因为玩家一开始会进入第一关，所以会看到竞技场里有三个随机生成的钻石。 

在生成钻石之后，我们就可以编写代码，用`pollBlockHits()`函数（在冒险任务4里已经学过）来监控玩家是否“击中”钻石，如果玩家击中一个钻石方块，就把它变成空气。

在关卡循环的下面添加如下代码：

1. 在关卡循环下缩进一级，调用`pollBlockHits()`函数来获得方块被击中的事件列表：

    ```
        #level loop
        while not gameOver and not levelComplete:
            hits = mc.events.pollBlockHits()    # 添加这一行
    ```

2. 遍历事件列表，获得被击中的方块的类型：

    ```
            for hit in hits:
                blockHitType = mc.getBlock(hit.pos.x, hit.pos.y,  
                                           hit.pos.z)
    ```

3. 检查被击中的方块是不是`DIAMOND_BLOCK`。如果是，就把它变成AIR，并从`diamondLeft`变量里减去1：

    ```
                if blockHitType == block.DIAMOND_BLOCK.id:
                    mc.setBlock(hit.pos.x,hit.pos.y, hit.pos.z,  
                                block.AIR.id)
                    diamondsLeft = diamondsLeft - 1
    ```

    变量`diamondLeft`会在玩家到达竞技场的远端时被用来检查是否所有钻石都被收集到。

4. 运行程序。现在当玩家集中钻石方块时，它就会变成空气。

> 技巧提示
>
> 记住，要击中一个方块，玩家必须拿着剑，并且用鼠标右键点击。

> 挑战
>
> 你能让击中钻石变得困难一点吗？比如把钻石升高或者降低，让玩家只有在空中的时候才能击中。

###时间截止

如果玩家有无限多的时间，游戏就会变得很简单（也很无聊）。加入时间限制可以让游戏变得更有挑战性。我们给玩家设定的目标就是在规定的时间里收集到所有钻石并到达竞技场的远端。

时间到了，游戏就结束了。我们的下一个任务是在关卡循环里加入检查时间的代码，如果时间已经结束，就把`gameOver`标志设为`True`：

1. 在关卡循环下缩进一级，计算这一关剩下的秒数：

    ```
        #level loop
        while not gameOver and not levelComplete:
            secondsLeft = TIMEOUTS[level] - (time.time() - start)
    ```

2. 如果剩下时间小于0，把`gameOver`设置为`True`并向聊天频道发送一条消息：

    ```
            if secondsLeft < 0:
                gameOver = True
                mc.postToChat("Out of time...")
    ```

3. 运行程序。30秒之后（因为这是第一关），程序就会结束，消息“Out of time...”会出现（见图9-12）。

------

<u>深入代码</u>

关卡剩余的秒数用下面代码进行计算：

```
        secondsLeft = TIMEOUTS[level] - (time.time() - start)
```

变量`secondsLeft`使用常量`TIMEOUTS`的本关卡最大时间`TIMEOUTS[level]`，减去玩家已经用掉的时间。后者是当前时间减去关卡开始时间：

`(time.time() - start)`

------



###跟踪玩家

当玩家收集到所有钻石并到达竞技场的另一边，就通过了这一关卡。如果他掉进了河里或者陷阱里，会退回到竞技场的起点。要做到这些，我们的程序需要监控玩家的位置。

在检查过玩家的位置之后，程序或者设置`levelComplete`标志位`True`（如果玩家已经收集到所有钻石的话），或者把玩家的位置设置到竞技场起点。

我们的下一个任务是在关卡循环中加入代码来跟踪玩家的位置，以及把玩家放回竞技场起点或者完成关卡：

1. 在关卡循环下缩进一级，获取玩家位置：

    ```
        #level loop
        while not gameOver and not levelComplete:
            pos = mc.player.getTilePos()    # 添加这一行
    ```

2. 检查玩家的高度（y坐标），是否小于竞技场的高度。如果是，那么他一定是掉进了河里或者陷阱里，所以要把他放回竞技场起点：

    ```
            if pos.y < arenaPos.y:
                mc.player.setPos(arenaPos.x + 1, arenaPos.y + 1,  
                                 arenaPos.z + 1)
    ```

3. 检查玩家是否到达竞技场的另外一边，并且已经收集到了所有钻石。这是通过检查玩家的z坐标是否跟竞技场另一边相同来实现的，如果是，就把`levelComplete`标志设为`True`：

    ```
            if pos.z == arenaPos.z + ARENAZ and diamondsLeft == 0:
                levelComplete = True
    ```

    当`levelComplete`标志变成`True`，关卡循环结束，就会重新生成钻石然后再次开始游戏。

4. 运行程序。这次当玩家收集到所有钻石并跑到竞技场另一边时，游戏会重新开始。如果玩家掉进河里或者陷阱里，他就会回到竞技场的起点。

> martin says
>
> 到目前为止，游戏只能玩第一关——最初的也是最简单的一关。建议你先练习一下，因为接下来难度会越来越大。

###完成关卡和计算分数

玩家通过关卡之后，程序需要计算他的得分，并进入下一个关卡。如果玩家通过关卡，就会得分。每收集到一颗钻石得一分，然后再乘以剩余的秒数。

我们要添加下面代码来完成这些。新代码要在游戏循环下缩进一级，不过必须在关卡循环结束之后：

1. 在游戏循环下、关卡循环之后，缩进一级，检查玩家是否通过关卡：

    ```
    #game loop
    while not gameOver:
        [code]
        #level loop
        while not gameOver and not levelComplete:
            [code]
            if levelComplete:    # 添加这一行
    ```

2. 如果是，就计算本关的得分并加到`points`变量里，然后把结果发送的聊天栏：

    ```
                points = points + (DIAMONDS[level] * int(secondsLeft))
                mc.postToChat("Level Complete - Points = " + str(points))
    ```

3. 把level变量加1进入下一关：

    ```
                level = level + 1
    ```

4. 如果已经是最后一关，就把gameOver标志设为True，并发送一条祝贺的消息到聊天栏：

    ```
                if level == LEVELS:
                    gameOver = True
                    mc.postToChat("Congratulations - All levels complete")
    ```

    常量`LEVELS`保存了游戏的总关数。

5. 运行程序。

我们的游戏已经快要完成！当玩家收集到所有钻石并在时间结束之前奋力冲到竞技场的终点，游戏就会开始下一个关卡。之后难度会加大一些，有更多钻石要收集并且时间更短。如果玩家通过所有关卡，游戏就结束了，祝贺！

> 挑战
>
> 试着添加更多关卡。你可以给游戏的前几关设置更长的时间和更少的钻石，让游戏初期变得更轻松，然后逐渐加大难度。

###添加游戏结束消息

在完成这个游戏之前，我们要做的最后一件事是在程序的末尾添加一条消息，告诉玩家游戏已经结束，并给出他的最终得分（图9-13）。只要在程序最下方加入下面代码就可以了：

```
mc.postToChat("Game Over - Points = " + str(points))
```



> 挑战
>
> 把玩家的得分保存到电脑上的一个文件里，并建立一个排行榜——游戏结束后，显示玩家在排行榜上的位置。

你可以在配套网站上下载Crafty Crossing的完整源代码。

我们的编程到这里还没有结束呢！这个程序是可以扩展的，尝试调整各种参数，加入一些新的机关，创造一个更有意思的竞技场。只要你想，就可以做到！

##第四部分：添加按钮和显示器

Crafty Crossing程序有一些问题。不管玩家有没有准备好，游戏都会自动开始。而且也没有地方显示有用的信息，比如剩余的钻石数量和时间是否结束。

在冒险任务的最后一部分，我们要重新使用冒险任务5里用来制造引爆器的那些硬件，给游戏添加一个开始按钮。我们还要是使用7段显示器来展示剩余钻石的数量。在关卡结束只剩5秒钟的时候，显示器上的小数点会被点亮。

###硬件准备

我们需要跟冒险任务5一样的电子元件，按照该冒险里最后的任务，制造引爆器那样连接。我们需要一个安装了7段显示器和按钮的面包板，连接到树莓派或者Arduino（图9-15）。如果需要，你可以复习一下冒险任务5，看看该如何连接这些硬件。

###设置硬件

要使用按钮和7段显示器，首先要导入正确的模块，设置GPIO并定义表示引脚的常量。

修改Crafty Crossing程序，增加设置硬件，并等待按钮按下之后再开始游戏：

1. 在现有的`import`语句之下添加线面一行导入7段显示模块：

    ```
    import anyio.seg7 as display
    ```

2. 导入GPIO模块，并为要使用的引脚定义相应的常量：

    **在树莓派上：**
    ```
    import RPi.GPIO as GPIO
    BUTTON = 4
    LED_PINS = [10,22,25,8,7,9,11,15]
    ```

    **在Arduino上：**
    ```
    import anyio.GPIO as GPIO
    BUTTON = 4
    LED_PINS = [7,6,14,16,10,8,9,15]
    ```

3. 设置硬件：

    ```
    GPIO.setmode(GPIO.BCM)
    GPIO.setup(BUTTON, GPIO.IN)
    ON = False # common-anode, set to True for common cathode
    display.setup(GPIO, LED_PINS, ON)
    ```

    如果使用普通的阴极7段显示器（在冒险任务5里已经讲过），请使用`ON = True`替换`ON = False`。

4. 在游戏循环之前，添加下面代码来等待按钮按下：

    ```
    mc.postToChat("Press the button to start")
    while GPIO.input(BUTTON):
        time.sleep(0.1)
    # 添加上面三行
    # game loop
    while not gameOver:
    ```

    现在，程序会创建竞技场和机关，然后等待玩家按下按钮。只有按钮按下之后，程序才会退出循环，把玩家放入竞技场并开始游戏。

5. 让程序正确结束并清理GPIO，这一点很重要。在程序末尾添加下面代码：

    ```
    GPIO.cleanup()
    ```

6. 运行程序，并测试开始按钮。

> 挑战
>
> 试试看你能不能修改程序，在程序结束后可以按下按钮重新开始游戏，而不需要重新运行程序？

###钻石倒数

7段显示器用来显示还有多少个钻石需要收集。显示器的内容用`display.write()`函数来更新。

要实现钻石倒数，我们需要修改Crafty Crossing程序，每次在竞技场里生成钻石的时候或者每次玩家集中钻石的时候，都要更新显示器的内容。

1. 在游戏循环中调用过`createDiamonds()`函数在竞技场里放入钻石之后，加入下面代码更新显示：

    ```
    createDiamonds(arenaPos, DIAMONDS[level])
    diamondsLeft = DIAMONDS[level]
    display.write(str(diamondsLeft))    # 添加这一行
    ```

    函数`display.write()`需要传递一个字符串类型的参数，所以剩余钻石数量要用`str()`函数转换成字符串，然后才能作为这个函数的参数。

2. 当玩家集中钻石，剩余钻石数减1之后，更新显示：

    ```
    hits = mc.events.pollBlockHits()
    for hit in hits:
        blockHitType = mc.getBlock(hit.pos.x, hit.pos.y,  
                                   hit.pos.z)
        if blockHitType == block.DIAMOND_BLOCK.id:
            mc.setBlock(hit.pos.x,hit.pos.y, hit.pos.z,  
                        block.AIR.id)
            diamondsLeft = diamondsLeft - 1 
            display.write(str(diamondsLeft))  # 添加这一行
    ```


3. 在程序的末尾，我们要确保清除显示的内容，在清理GPIO之前，添加下面代码清空显示：

    ```
    display.clear()  # 添加这一行
    GPIO.cleanup()
    ```

4. 运行程序。钻石的数量就会被显示出来，当玩家集中一个钻石方块时会减1，直到减为0。

> martin says
>
> 7段显示器最大只能显示数字9。如果我们修改程序让某一关卡有多于9个钻石，就需要给程序添加一个`if`语句，让程序只在剩余钻石数量小于等于9时才开始更新显示。

###剩余时间指示器

我们的最后一个任务是修改程序，更新显示器让玩家知道时间快要到了。我们通过点亮显示器的小数点来实现这一点。小数点应该在剩余时间小于5秒时被点亮，让玩家有最后的机会去尝试完成关卡。

按照下面步骤修改Crafty Crossing程序在剩余时间小于5秒是点亮小数点：

1. 在关卡循环中计算出剩余秒数之后，检查剩余时间是不是小于5秒。如果是，点亮小数点，否则就关闭：

    ```
        secondsLeft = TIMEOUTS[level] - (time.time() - start)
        # 添加下面几行
        if secondsLeft < 5:
            display.setdp(True)
        else:
            display.setdp(False)
    ```

2. 运行程序。当剩余时间小于5秒时，小数点就会亮起来。

> 挑战
>
> 开始新关卡时，在显示钻石数量之前，让关卡序号在显示器上一闪而过。

##未来的冒险

Minecraft是一块充满创意和冒险的画布。加上通过代码控制游戏的可能，唯一能限制你的只有想象力。接下来你想做点儿什么？

这里有一些想法和资料，可能会给你一些启发：

1. 编写游戏是一个提升编程能力的好方法。到[这个网站](http://www.classicgamesarcade.com/)看看，也许有不错的点子。
2. Minecraft是一个多人游戏。不如利用这一点来编写一个可以让很多人一起玩耍的游戏？
3. 与电路进行交互可以让Minecraft跟现实世界关联起来。在[Adventures in Arduino](eu.wiley.com/WileyCDA/WileyTitle/productCd-1118948475.html)这本书里学习更多技巧。
4. 互联网上有很多开放的数据源。试试把Minecraft和某些网站整合，比如twitter或者met office的天气预报。