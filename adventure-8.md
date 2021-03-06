#冒险8：有思想的方块

计算机自己是不会思考的。它们不会有任何想法，只会做程序员让它们做的事。不过我们可以让计算机看起来像是有思想，并能自己作出决定。通过编程可以让计算机“理解”当前的情况，并制定规则让它能“决定”应该做什么，这会给我们带来很多乐趣。

在这个冒险任务里，我们要编程创造一个会跟着我们走的方块朋友，前提是不要走太远。我们还要创造一个飞碟，它会追赶你指导飞到你头顶并把你传送上去。

我们会学习如何让一个方块按着自己决定的最佳路径进行移动，以及如何使用Python的random模块让计算机看起来像是会思考一样。我们还要用minecraftstuff模块（已经包含在新手包里）的MinecraftShape函数来生成各种图形。

##我的方块朋友

Minecraft世界非常孤独。但是我们的游戏角色却不一定要孤独——我们可以创造一个跟着他四处移动、跟他对话的方块做他的朋友（如图8-1）。

要给一个方块伴侣进行编程，我们必须像她那样思考！她在游戏角色身边的时候会很高兴，所以会跟着角色四处移动并停留在角色身边。只要你的游戏角色在她身边，她就会很高兴；如果角色走开了，她会跟在后面。如果角色走得太远，她就会感觉很失落并且不再跟随，直到角色回来并给他一个拥抱（站在她旁边就可以了）。

方块伴侣程序有两个不同的部分：

* **方块决定要做什么的规则**：程序的这一部分决定了方块伴侣是要朝游戏角色（目标）移动，还是停留在原地。
* **移动方块到目标位置的代码**：方块伴侣到达目标位置以后，会再次使用上面的规则来计算接下来要做什么。

当方块伴侣向目标移动时，她应该在地面上移动，而不是漂浮在空中或者埋在地下！我们用`mc.getHeight(x, z)`函数来实现这一点。这个函数需要一个水平坐标(x, z)作为输入，返回天空下面第一个不是空气（AIR）的方块的垂直坐标（y）。

从新建一个程序开始：

1. 打开IDLE，点击File->New File新建一个文件，保存到MyAdventures文件夹并命名为BlockFriend.py。

2. 导入minecraft、block、minecraftstuff、math和time模块：

    ```
    import mcpi.minecraft as minecraft
    import mcpi.block as block
    import mcpi.minecraftstuff as minecraftstuff
    import math
    import time
    ```

3. 我们要做的第一件事是编写一个计算两点之间距离的函数。这个函数用来计算方块伴侣和游戏角色之间的距离：

    ```
    def distanceBetweenPoints(point1, point2):
        xd = point2.x - point1.x
        yd = point2.y - point1.y
        zd = point2.z - point1.z
        return math.sqrt((xd*xd) + (yd*yd) + (zd*zd))
    ```

4. 现在我们需要确定方块伴侣跟角色距离超过多少时停止跟随。声明一个常量用来用来作为判断“距离太远”的标准。我选择了15，也就是说当方块伴侣和游戏角色距离达到15个方块，方块伴侣就不再跟随游戏角色了：

    ```
    TOO_FAR_AWAY = 15
    ```

5. 创建Minecraft和MinecraftDrawing对象：

    ```
    mc = minecraft.Minecraft.create()
    mcdrawing = minecraftstuff.MinecraftDrawing(mc)
    ```

6. 声明一个变量保存方块伴侣的情绪。在这个冒险任务里，方块伴侣的情绪可以是开心（happy）或者伤心（sad）。先把变量设为“happy”：

    ```
    blockMood = "happy"
    ```

7. 生成方块伴侣。为了让它跟角色有一点距离，获取角色的座标，在x座标上加5，用`getHeight()`函数获得y座标，保证方块伴侣在底面上：

    ```
    friend = mc.player.getTilePos()
    friend.x = friend.x + 5
    friend.y = mc.getHeight(friend.x, friend.z)
    mc.setBlock(friend.x, friend.y, friend.z,  
                block.DIAMOND_BLOCK.id)
    mc.postToChat("<block> Hello friend")
    ```

8. 生成方块伴侣的目标，方块伴侣将要向这个目标移动。首先，把目标设置为方块伴侣当前的位置，这样方块伴侣在程序刚开始的时候就不会移动：

    ```
    target = friend.clone()
    ```

9. 开始一个无限循环，让程序能一直运行。（注意这之后的所有代码都要缩进。）

    ```
    while True:
    ```

10. 获得角色座标，并用`distanceBetweenPoints()`函数计算角色和方块伴侣之间的距离：

  ```
      pos = mc.player.getTilePos()
      distance = distanceBetweenPoints(pos, friend)
  ```

11. 编写方块伴侣用来决定要做什么的规则。如果它正处在开心（happy）状态，就比较方块伴侣和角色之间的距离和`TO_FAR_AWAY`常量，如果大于常量，把方块伴侣的情绪改为伤心（sad），见图8-2：

    ```
        if blockMood == "happy":
            if distance < TOO_FAR_AWAY:
                target = pos.clone()
            elif distance >= TOO_FAR_AWAY:
                blockMood = "sad"
                mc.postToChat("<block> Come back. You are too far away. I need a hug!")
    ```

12. 如果距离小于`TO_FAR_AWAY`常量，并且方块伴侣的情绪是伤心，这种情况下要一直等到距离减小到一个方块（也就是拥抱）然后才把情绪变为开心：

    ```
        elif blockMood == "sad":
            if distance <= 1:
                blockMood = "happy"
                mc.postToChat("<block> Awww thanks. Let’s go.")
    ```

13. 接下来这段代码让方块伴侣向目标移动：

    ```
        if friend != target:
    ```

14. 用MinecraftDrawing里的`getLine()`函数找出方块伴侣和目标位置之间所有的方块：

    ```
            blocksBetween = mcdrawing.getLine( 
                friend.x, friend.y, friend.z,
                target.x, target.y, target.z)
    ```

    `getLine()`函数类似于`drawLine()`（参考冒险任务7）。它需要两个点的坐标作为参数，不过它不会画线，而是返回一个坐标点的列表，里面的坐标组成一条连接两个点之间的直线。

15. 紧接着前面的代码，我们要让代码循环处理方块伴侣和目标位置之间的所有方块，让方块伴侣沿着这写方块逐步移动到角色身边：

    ```
        for blockBetween in blocksBetween[:-1]:
            mc.setBlock(friend.x, friend.y, friend.z, block.AIR.id)
            friend = blockBetween.clone()
            friend.y = mc.getHeight(friend.x, friend.z)
            mc.setBlock(friend.x, friend.y, friend.z,  
                        block.DIAMOND_BLOCK.id)
            time.sleep(0.25)
        target = friend.clone()
    ```

    `for`循环结束之后，方块伴侣就到达了自己的目标位置，把新的目标位置设置为方块伴侣的当前位置。这样她就不会再移动了。

    程序移动方块伴侣的方法是，先把她从前一个位置删除掉（通过把这个位置的方块设为空气AIR），然后把方块伴侣的位置更新为直线上的下一个位置，最后在新位置重新创建出方块伴侣。

    注意`blockBetween`和`blocksBetween`之间的区别，前者存储了方块伴侣当前所在的位置，后者是一个方块伴侣和目标位置之间的方块列表。

> martin says
>
> 方块伴侣移动的速度取决于程序在每次移动她之前延迟的时间——`time.sleep(0.25)`。如果这个延迟太短，方块伴侣就会移动很快，游戏角色永远也不可能摆脱她。如果延迟太大，移动速度就会慢到让你发狂。

16. 在循环的末尾加入一个小延迟，防止程序一直循环并请求角色坐标，给Minecraft造成太大压力：

    ```
    time.sleep(0.25)
    ```

17. 运行程序。

方块伴侣会出现在游戏角色的身边，并且一直跟随游戏角色，除非二者之间距离太大。一旦出现这种情况，方块伴侣就会停下来，必须要让游戏角色回去站在她的身边，才会重新开始跟随。

你可以在本书的配套网站上下载完整的源码。

------

<u>深入代码</u>

我们编写了一个`distanceBetweenPoints(point1, point2)`函数来计算方块和游戏角色之间的距离。这个函数用勾股定理（毕达哥拉斯定理）来计算亮点之间的准确距离。在冒险任务4的“增加导航信标”挑战里，你可能已经了解过这个定理了。

这个函数是这样计算距离的：

1. 算出点1（point1）和点2（point2）的x、y、z座标的差值：

    ```
    xd = point2.x - point1.x
    yd = point2.y - point1.y
    zd = point2.z - point1.z
    ```

2. 计算这些差值的平方：

    ```
    xd*xd
    ```

3. 把平方的结果加起来：

    ```
    (xd*xd) + (yd*yd) + (zd*zd)
    ```

4. 用Python的`math.sqrt()`函数计算平方和的平方根，返回平方根作为两个点之间的距离：

查看[这个网页](http://www.mathsisfun.com/algebra/distance-2-points.html)学习勾股定理，以及如何使用它计算亮点之间的距离。

方块伴侣移动的方式是，先找出和目标位置之间的所有方块，然后循环遍历这些方块：

[:-1]告诉Python要遍历`blocksBetween`列表里的所有方块。用这种方式，方块伴侣向游戏角色移动时会站在角色的旁边，而不是角色的上面。

------

> 定义
>
> 一个数字的**平方根**是跟自己相乘可以得到这个数字的值。例如，9的平方根是3，因为3×3=9。

> 挑战
>
> 1. 方块伴侣总是以固定的速度移动：大约每0.25秒移动一个方块的距离，或者说是4个方块每秒。不过，真实的伴侣应该目标越远的时候移动速度越快。试着修改程序，当方块伴侣距离角色越远的时候，移动越快。
> 2. 复习冒险任务4里的“添加回家信标”，并且重新完成这个挑战任务，不过这次要用`distanceBetweenPoints()`函数实现一个更好的距离估计算法。

##用随机数让方块伴侣更有趣

刚刚编写的方块伴侣程序的问题在于它是可以预测的；它总是做同样的事情。只要运行过几次程序之后，我们就能知道它什么时候会干什么，很快就会变得无聊。要让方块伴侣有自己的“思想”，我们需要给它一些不可预测性。

> 定义
>
> **可预测**是指在事情发生之前就可以预知。如果我们想要表现的更加真实，可预测就不是什么好事。真实的事情并不总是可预测的。

我们可以使用随机数来模拟不可预测性——换句话说，就是让某些东西表现得不可预测。我们要让程序按照一定的**概率**来选择要做的事情；例如，我们可以让它每100次做某件事一次。通过添加更多不同的概率规则，我们可以让程序变得更难预测。在修改方块伴侣程序使用随机数之前，我们先来研究一下生成随机数和检查概率的代码。你可能还记得我们在冒险任务3里就已经用过随机数了。

> 定义
>
> **概率**表示某件事情发生的可能性，例如，抛硬币的时候正面朝上的概率是50%（或者1/2）。

Python的`random`模块里有`random.randint(startNumber, endNumber)`函数用来生成一个`startNumber`和`endNumber`之间的随机数。

下面代码每次运行的时候会打印一个1和10之间的随机数。要想看到执行结果，需要新建一个程序：

```
import random
randomNo = random.randint(1,10)
print randomNo
```

再添加一个`if`语句判断随机数是不是10，就是一个每10次有一次为真的概率检查：

```
import random
if random.randint(1,10) == 10:
    print "This happens about 1 time in 10"
else
    print "This happens about 9 times out of 10"
```

如果运行这个程序100次，你大概会看到“Tis happens about 1 time in 10”这句话被打印10次（见图8-3），也有可能是9次或者11次，甚至是一次也没有。这是不可预测的。

如果把随机数和概率检查用在方块伴侣程序里，就可以让它变得更难预测。甚至可以让方块伴侣变得对玩家“不友好”！

在方块伴侣程序中添加一些规则，当方块伴侣处在“伤心”状态时，她有1/100的概率会等得不耐烦，即使玩家回到她身边给她一个拥抱，也不会再跟着玩家走。

1. 打开IDLE，并打开MyAdventures文件夹里的BlockFriend.py程序。
2. 点击File->Save As把文件保存为BlockFriendRandom.py。
3. 在程序顶部的import语句后添加random模块（代码里粗体的一行）：

    ```
    import mcpi.minecraft as minecraft
    import mcpi.block as block
    import mcpi.minecraftstuff as minecraftstuff
    import math
    import time
    import random
    ```

4. 添加一个概率为1/100的随机数测试，当她的情绪为“伤心”时，把方块伴侣的情绪改为“受够了（hadenough）”：

    ```
        elif blockMood == "sad":
            if distance <= 1:
                blockMood = "happy"
                mc.postToChat("<block> Awww thanks. Let’s go.")
            if random.randint(1,100) == 100:
                blockMood = "hadenough"
                mc.postToChat("  <block> That's it. I have had enough.")
    ```

5. 运行程序。

当玩家距离方块太远，如果等待足够长的时间（让1/100的概率成真），方块伴侣就会觉得自己受够了，并和玩家绝交！一旦发生这种情况，方块伴侣就会说：“That's it. I have had enough（好了，我受够了）.”，并且会永远停在原地不动。

> 挑战
>
> 修改程序，当方块伴侣的情绪为“受够了”的时候，如果玩家给她一个拥抱，有1/50的机率会原谅玩家。

##更大的形体

在方块伴侣程序中，我们让一个方块跟随玩家移动。如果想要很多方块一起移动呢？或者说怎么让一个很多方块组成的形状，例如汽车或者外形飞船，移动呢？

这里会复杂很多，因为我们要控制很多方块。每次要移动这个物体的时候，都需要把所有方块设置为空气（AIR），并且在新位置重新生成。如果方块数量太多，这个形体移动起来就会像爬行一样慢。

minecraftstuff模块里有一个MinecraftShape函数，用来生成和移动形状。它管理组成某个形状的所有方块，当需要移动的时候，只移动发生改变了的方块，而不是所有方块。

要使用MinecraftShape，我们需要创建一个组成形体的方块列表，告诉它这个形体的样子。形体里的每个方块都有一个坐标(x, y, z)和一个方块类型。

图8-4里是一个7个方块组成的简单的形体，包括了方块的坐标。在这个例子里，“坐标”跟minecraft里的坐标不太一样。木马中心的坐标是(0,0,0)，其他方块的坐标都是相对于这个中心点的，所以右边的方块坐标是(1,0,0)。

现在，我们要编写一个程序用MinecraftShape来生成图8-4里的木马，并让它移动起来：

1. 打开IDLE，点击File->New File新建一个程序。保存文件到MyAdventures文件夹里，命名为WoodenHorse.py。
2. 导入minecraft、block、minecraftstuff和time等模块：

    ```
    import mcpi.minecraft as minecraft
    import mcpi.block as block
    import mcpi.minecraftstuff as minecraftstuff
    import time
    ```

3. 创建一个Minecraft对象：

    ```
    mc = minecraft.Minecraft.create()
    ```

4. 创建一个组成木马的方块列表，用`ShapeBlock`定义每个方块的相对坐标和方块类型：

    ```
    horseBlocks = [ 
           minecraftstuff.ShapeBlock(0,0,0,block.WOOD_PLANKS.id),
           minecraftstuff.ShapeBlock(-1,0,0,block.WOOD_PLANKS.id),
           minecraftstuff.ShapeBlock(1,0,0,block.WOOD_PLANKS.id),
           minecraftstuff.ShapeBlock(-1,-1,0,block.WOOD_PLANKS.id),
           minecraftstuff.ShapeBlock(1,-1,0,block.WOOD_PLANKS.id),
           minecraftstuff.ShapeBlock(1,1,0,block.WOOD_PLANKS.id),
           minecraftstuff.ShapeBlock(2,1,0,block.WOOD_PLANKS.id)]
    ```

    这些方块的相对坐标跟图8-4一样。

5. 我们还要告诉MinecraftShape要在Minecraft世界里的什么位置生成这个木马。活动玩家当前的坐标，并在z和y坐标上加1，这样它就不会在玩家的正上方：

    ```
    horsePos = mc.player.getTilePos()
    horsePos.z = horsePos.z + 1
    horsePos.y = horsePos.y + 1
    ```

6. 用MinecraftShape生成木马，把Minecraft对象、木马的坐标和组成木马的`ShapeBlock`列表作为参数传递跟函数：

    ```
    horseShape = minecraftstuff.MinecraftShape(mc, horsePos, horseBlocks)
    ```

7. 运行程序。应该会有一匹木马出现在玩家的身边。
8. 修改WoodenHorse.py程序，在最后添加下面代码让木马移动：

    ```
    for count in range(1,10):
        time.sleep(1)
        horseShape.moveBy(1,0,0)
        horseShape.clear()
    ```

    `moveBy(x, y, z)`函数告诉木马在三个方向上分别移动x、y、z个方块的距离。在这个例子里，`horseShape`在x方向上移动一个方块的距离。`clear()`函数删除木马，把所有的方块设为空气（AIR）。

9. 运行程序，然后看木马奔跑吧！

> 提示
>
> 除了`moveBy(x, y, z)`和`clear()`，我们还可以用`move(x, y, z)`函数让形体移动到Minecraft里的任何位置。如果形体已经被删除，可以用`draw()`重新生成。

配套网站上可以下载这个程序的完整代码。

形体很有用，我们马上就要用它生成一个外形飞船！稍后还要再次使用形体生成一些障碍物。

##外星入侵

外星人就要入侵Minecraft了！一艘飞船从天而降来到玩家头顶，玩家生命收到威胁——这些外星人豪不友善，不完成任务绝不罢休。

在接下来这个程序里，我们要用MinecraftShape和在方块伴侣程序里学到的编程技巧来创造一个外形飞船（图8-5），它会悬浮在底面上方，追逐玩家，试图飞到玩家头顶。如果让它成功，它就会把玩家传送到飞船上。

像木马程序一样，我们给出飞船中每个方块的相对位置和方块类型，用MinecraftShape来生成外星飞船。图8-6从侧面和上面给出了每个方块的相对座标。

> martin says
>
> 通过外形飞船的例子我们可以学到如何使用MinecraftShape生成三维形体。这些形体可大可小，可简单可复杂。我们在Minecraft程序中使用形体的时候就有很多选择。

像方块伴侣程序一样，外星入侵程序的代码也分成两部分。第一部分是决定外星飞船如何行动的规则；第二部分是让外星飞船向玩家移动的代码。

当外星飞船在追逐玩家的时候，它会在聊天栏发送消息来嘲讽玩家（比如“你不可能一直跑下去”）。消息的内容是从一个嘲讽语句列表里随机选择的（图8-7）。

决定外星飞船如何行动的规则基于下面三种模式之一：

* **下降**：当程序开始时，这是飞船的初始状态，它会从控制直接向玩家的位置降落。
* **攻击**：一旦飞船落地，它就开始攻击，一直追这玩家直到飞到玩家的正上方，然后就会把玩家传送到飞船里。
* **任务完成**：当玩家被传送到飞船里，并且外星人已经准备好要把玩家送回地球的时候，就进入了这种状态。这时程序就会结束，并且玩家会被传送回去。

一旦外星飞船抓住了玩家，它就会建造一个禁闭室把玩家关押在里面（图8-8）。外星人会给玩家发送一条消息，之后会把玩家传送会原来的位置并清除禁闭室。

按照下面步骤来编写外星入侵程序：

1. 打开IDLE，点击new->New File新建文件，保存在MyAdventures文件夹里，命名为AlienInvasion.py。
2. 导入minecraft、block、minecraftstuff和time模块：

  ```
  import mcpi.minecraft as minecraft
  import mcpi.block as block
  import mcpi.minecraftstuff as minecraftstuff
  import time
  ```

3. 定义distanceBetweenPoints()函数：

  ```
  def distanceBetweenPoints(point1, point2):
        xd = point2.x - point1.x
        yd = point2.y - point1.y
        zd = point2.z - point1.z
        return math.sqrt((xd*xd) + (yd*yd) + (zd*zd))
  ```

4. 定义程序中使用的常量。`HOVER_HEIGHT`是外星飞船悬浮的高度；`ALIEN_TAUNTS`是外星飞船在追逐玩家时发送的嘲讽消息列表：

    ```
    HOVER_HEIGHT = 15
    ALIEN_TAUNTS = ["<aliens>You cant run forever",
                    "<aliens>Resistance is useless",
                    "<aliens>We only want to be friends"]
    ```

    你可以自己修改外星人的嘲讽语句——看看你的创造力如何。也可以增加新的语句。

5. 创建Minecraft和MinecraftDrawing对象：

    ```
    mc = minecraft.Minecraft.create()
    mcdrawing = minecraftstuff.MinecraftDrawing(mc)
    ```

6. 设置外星飞船的起始位置和状态，分别是玩家正上方50个方块处和“landing(下降)”：

    ```
    alienPos = mc.player.getTilePos()
    alienPos.y = alienPos.y + 50
    mode = "landing"
    ```

7. 用MinecraftShape生成外星飞船（见图8-6）：

    ```
    alienBlocks = [ 
        minecraftstuff.ShapeBlock(-1,0,0,block.WOOL.id, 5),
        minecraftstuff.ShapeBlock(0,0,-1,block.WOOL.id, 5),
        minecraftstuff.ShapeBlock(1,0,0,block.WOOL.id, 5),
        minecraftstuff.ShapeBlock(0,0,1,block.WOOL.id, 5),
        minecraftstuff.ShapeBlock(  0,-1,0, 
        block.GLOWSTONE_BLOCK.id),
        minecraftstuff.ShapeBlock(  0,1,0, 
        block.GLOWSTONE_BLOCK.id)]
    alienShape = minecraftstuff.MinecraftShape(mc, alienPos, 
        alienBlocks)
    ```

8. 写一个while循环，这个循环会一直执行下去直到飞船状态变为“mission accomplished(任务完成)”，否则就会退出循环：

    ```
    while mode != "mission accomplished":
    ```

9. 每次循环首先获得玩家的位置：

    ```
        playerPos = mc.player.getTilePos()
    ```

10. 下一段代码用来决定飞船接下来要做什么——如果状态是“landing(下降中)”，设置飞船的目标为玩家的上方（飞船会向这个目标位置移动），并把状态改成“attack(攻击)”：

   ```
       if mode == "landing":
           mc.postToChat("  <aliens> We dont come in peace - please panic")
           alienTarget = playerPos.clone()
           alienTarget.y = alienTarget.y + HOVER_HEIGHT
           mode = "attack"
   ```

11. 如果状态是“attack”，检查外星飞船是不是在玩家上方。如果是，就把玩家传送到飞船里，并把状态改为“missionaccomplished”。否则，如果玩家已经逃跑，则把飞船的目标设为玩家现在的位置，并发送一条嘲讽消息：

    ```
        elif mode == "attack":
            #check to see if the alien ship is above the player
            if alienPos.x == playerPos.x and alienPos.z == playerPos.z:
                mc.postToChat("<aliens>We have you now!")
                #create a room
                mc.setBlocks(0,50,0,6,56,6,block.BEDROCK.id)
                mc.setBlocks(1,51,1,5,55,5,block.AIR.id)
                mc.setBlock(3,55,3,block.GLOWSTONE_BLOCK.id)
                #beam up player
                mc.player.setTilePos(3,51,5)
                time.sleep(10)
                mc.postToChat("  <aliens>Not very interesting at all - send it back")
                time.sleep(2)
                #send the player back to the original position
                mc.player.setTilePos(  playerPos.x, playerPos.y, playerPos.z)
                #clear the room
                mc.setBlocks(0,50,0,6,56,6,block.AIR.id)
                mode = "missionaccomplished"
        else:
            #the player got away
            mc.postToChat(ALIEN_TAUNTS[random.randint(0,len(ALIEN_TAUNTS)-1)])
            alienTarget = playerPos.clone()
            alienTarget.y = alienTarget.y + HOVER_HEIGHT
    ```

    玩家被传送到外星飞船里之后，就会在出生点上方50个方块处建造一个房间，并把玩家的位置改到房间里。（就像Doctor Who的Tardis，内部空间要比外形更大！）并且会向聊天栏发送消息，然后会把玩家的位置改会刚开始的地方，并清楚房间。

    > martin says
    >
    > 玩家被传送到的房间在出生位置上方，部分原因是为了方便，另外也是因为这里距离其他东西都比较远。你可以在任何位置建造这个房间，不过，玩家离开这个房间后，它就会被清除，一切就会恢复之前的状态。

12. 在程序最后输入下面代码，如果外星飞船的位置不等于目标位置（在上面规则里设置），让外星飞船向目标移动，这段代码要在`while`循环下缩进一级：

    ```
        if alienPos != alienTarget:
            blocksBetween = mcdrawing.getLine( 
              alienPos.x, alienPos.y, alienPos.z,
              alienTarget.x,alienTarget.y,alienTarget.z)
            for blockBetween in blocksBetween:
                alienShape.move(  blockBetween.x, blockBetween.y, blockBetween.z)
                time.sleep(0.25)
            alienPos = alienTarget.clone()
    ```

13. 到这里程序会返回`while`循环的开始。当飞船状态变为“mission accomplished”时，`while`循环结束。程序的最后一行让外星飞船消失：

    ```
    alienShape.clear()
    ```

14. 运行程序，当心从天而降的外星飞船。

在本书的配套网站上可以下载到外星入侵程序的完整源代码。

------

<u>深入代码</u>

每次外星飞船追逐玩家时，都会从`ALIEN_TAUNTS`常量中随机挑选一个嘲讽语句发送到聊天栏：

```
    mc.postToChat(ALIEN_TAUNTS[ 
        random.randint(0,len(ALIEN_TAUNTS)-1)])
```

`ALIEN_TAUNTS`是一个字符串列表；`random.randint()`函数用来从列表中挑选消息，在0到列表`ALIEN_TAUNTS`长度减一的范围内随机选择一个数字。

列表的长度要减去1是因为，`len()`函数返回的是列表中数据对象的实际数量（例如3），但是引用列表中的数据对象时下标要从0开始（例如0、1、2）。

------

> 挑战
>
> 程序里的外星飞船非常简陋。试着制造一个让人惊叹和喜欢的外星飞船。修改飞船，让它在降落之后进入“lurk(潜伏)”模式，跟玩家保持一定的距离，但是不会攻击。然后在一定的概率下，会在没有警告的情况下进入攻击模式。

##关于模拟的更多冒险任务

在这个冒险任务里，我们用算法和规则模拟了伙伴和外星入侵——为什么不再进一步，模拟一些其他的东西，例如一群鸟（或者方块），大海里的波浪，或者用方块组成一个像康威生命游戏那样的完整的细胞系统。

查看[这个网页](https://zh.wikipedia.org/wiki/%E5%BA%B7%E5%A8%81%E7%94%9F%E5%91%BD%E6%B8%B8%E6%88%8F)了解更多关于康威生命游戏的知识。

下一个冒险任务

在下个冒险任务里，我们要用之前冒险任务里学到的所有技巧来编写一个游戏，在这个游戏里玩家要跟时钟赛跑并收集钻石。不过要小心——路上会有障碍物，它们会千方百计阻止玩家。