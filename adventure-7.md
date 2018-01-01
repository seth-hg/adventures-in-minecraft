#冒险任务7：建造平面和立体结构

Minecraft编程的有趣之处在于，我们不但可以在二维的屏幕上看到自己创造的东西，还可以在一个虚拟的三维时间里把它们建造出来。我们可以在它们周围走动，进到里面去，或者让它们变得更大——如果愿意的话，甚至可以炸掉它们！使用原本用于在二维屏幕上显示三维物体的技术，我们可以在Minecraft里对三维物体进行编程，让普通的物体变得非同寻常。

在这个冒险任务里，我们将学习如何使用minecraftstuff模块来建造二维线条和形状，然后通过一些数学的组合来编程一个巨大的时钟，这个钟达到你可以在它运转的时候站在它的指针上（见图7-1）。只要学会了创建二维形状，很快就能学会如何将它们组合起来建造巨大的三维结构。

##minecraftstuff模块

minecraftstuff是Minecraft API的一个扩展模块，专门为这本书编写，包含了绘制形状和控制三维物体需要用到的所有代码。它包含了一组称作MinecraftDrawing的函数集，可以用来绘制直线、原型、球体以及其他形状。这些复杂的代码都在一个单独的模块里，因此我们编写代码的时候会更简单，也更容易理解。模块是一种把程序划分成多个小块的方法。当程序变得庞大起来，就会变得难于阅读和理解，查找问题也需要花费更多时间。

Minecraft大冒险新手包里已经包含了mincraftstuff模块，导入Python程序的方法跟Minecraft和block这些模块一样。

##画直线、圆形和球体

把很多小的、简单的东西组合起来，就能按照你的想法来创建各种大型和复杂的东西。在这个冒险任务里，我们要使用很多的直线、圆形、球体和其他形状来生成一个巨大的Minecraft结构。在现阶段，我们要编写一个新程序，导入需要的模块，设置好Minecraft和minecraftrawing模块。稍后，我们会用模块里的函数来绘制直线、圆形和球体。

启动Minecraft、IDLE，如果使用Mac或者PC，还要同时启动Bukkit服务器。到现在我们已经练习过很多次了，不过如果需要提示的话，可以参考冒险任务1。

1. 首先启动IDLE，新建一个文件，保存在MyAdventures文件夹，命名为LinesCirclesAndSepheres.py。
2. 输入下面几行代码，导入minecraft、block、minecraftstuff和time四个模块:

    ```
    import mcpi.minecraft as minecraft
    import mcpi.block as block
    import mcpi.minecraftstuff as minecraftstuff
    import time
    ```

3. 建立和Minecraft的连接:

    ```
    mc = minecraft.Minecraft.create()
    ```

4. 输入下面代码用minecraftstuff模块来创建一个MinecraftDrawing对象：

    ```
    mcdrawing = minecraftstuff.MinecraftDrawing(mc)
    ```

------

<u>深入代码</u>

MinecraftDrawing是minecraftstuff模块中的一个类。类是面向对象编程方法里的概念，用来把相似的函数和数据组织在一起——在这个例子里，就是Minecraft绘图函数和相关数据——以便于理解和使用。使用类加上一个名字（例如mcdrawing），就可以创建一个对象。这叫做实例化。对象类似于变量，不同的是它不只保存值（或者说数据），还包含函数。

面向对象编程是一个复杂的话题，有成白上千的专著来讨论它的定义和用法。不过，[这里](en.wikibooks.org/wiki/A_Beginner's_Python_Tutorial/Classes)有一篇很有用的文章，介绍了怎么在Python中使用类。

你可以用IDLE打开MyAdventures/mcpi目录下的minecraftstuff.py文件来查看minecraftstuff的代码。

------



###画线

MinecraftDrawing对象有一个`drawLine()`函数，调用的时候传入两个坐标（x, y, z）和一个方块类型作为参数，就会在两个位置中间生成一条用方块组成的直线。就像在冒险任务3里学到的`setBlocks()`函数。

> 定义
>
> 有时一个函数需要某些信息才能运行，例如`setBlock()`函数需要x、y、z和方块类型，那么这些数值就被称作**参数**。当程序使用这些函数的时候，就是**调用**函数和**传递**参数。

下面的函数创建了一条如图7-2所示的方块组成的直线：

```
drawLine(x1, y1, z1, x2, y2, z2, blockType, blockData)
```


现在修改我们的程序，用`drawLine()`函数来画三条直线——一条竖直向上的，一条水平的和一条斜对角的。把下面几行代码加入到LinesCirclesAndSpheres.py程序的末尾：

1. 首先，我们要用下面的代码获得角色当前的位置：

    ```
    pos = mc.player.getTilePos()
    ```

2. 从角色当前位置向上画一条20个方块长的竖直线：

    ```
    mcdrawing.drawLine(pos.x, pos.y, pos.z,
                       pos.x, pos.y + 20, pos.z,
                       block.WOOL.id, 1)
    ```

3. 再画一条20个方块长度的水平线：

    ```
    mcdrawing.drawLine(pos.x, pos.y, pos.z,
                       pos.x + 20, pos.y, pos.z,
                       block.WOOL.id, 2)
    ```

4. 接着画一条20个方块长的斜对角线：

    ```
    mcdrawing.drawLine(pos.x, pos.y, pos.z,
                       pos.x + 20, pos.y + 20, pos.z, 
                       block.WOOL.id, 3)
    ```

5. 最后，我们还要添加一个时间延迟，便于我们看清楚整个过程：

    ```
    time.sleep(5)
    ```

6. 保存文件并运行程序。我们已经创建了三条直线，每一条都是用不同颜色的羊毛方块组成的：一个从角色所在位置竖直向上延伸，一个水平延伸，最后一个在前二者中间斜线延伸。

------

<u>深入代码</u>

`drawLine()`函数使用布雷森汉姆直线算法来画直线。看一下[这里](https://en.wikipedia.org/wiki/Bresenham's_line_algorithm)来了解更多关于这个算法的知识。

------



###画圆形

我们不止能画直线——也可以用MinecraftDrawing的`drawCircle()`函数来画圆形，只需要传递圆心的坐标、半径和方块类型作为参数。我们可以用下面的函数来画圆形：

```
drawCircle(x, y, z, radius, blockType, blockData)
```


要画出图7-3那样的圆形，把下面的代码加到LinesCirclesAndSpheres.py程序的末尾：

1. 首先，输入下面代码获取角色的位置：

    ```
    pos = mc.player.getTilePos()
    ```

2. 然后输入下面代码画一个圆环，从角色上方20个方块处开始，半径为20个方块：

    ```
    mcdrawing.drawCircle(pos.x, pos.y + 20, pos.z, 20,
                         block.WOOL.id, 4)
    ```

3. 最后，添加一个时间延迟，以便看清整个过程，同时在这段时间可以移动游戏角色：

    ```
    time.sleep(5)
    ```

4. 保存并运行程序。

之前的直线会首先被重新画一遍，然后有5秒钟的时间可以移动角色，来避免圆环跟直线重叠。

------

<u>深入代码</u>

`drawCircle()`函数使用中点算法画圆环。这种算法是基于布雷森汉姆直线算法修改而来的。你可以在[这里](https://en.wikipedia.org/wiki/Midpoint_circle_algorithm)了解更多关于这个算法的知识。

------



###画球体

`drawSphere()`函数类似于`drawCircle()`函数，需要中心点、半径和方块类型作为参数。我们可以用下面函数来创建球体：

```
drawSphere(x, y, z, radius, blockType, blockData)
```


要绘制图7-4那样的球体，把下面代码加入LinesCirclesAndSpheres.py程序：

1. 获取角色当前的位置：

    ```
    pos = mc.player.getTilePos()
    ```

2. 从角色上方20个方块处开始，画一个半径为15个方块长度的球体，输入下面代码：

    ```
    mcdrawing.drawSphere(pos.x, pos.y + 20, pos.z, 15, block.WOOL.id, 5)
    ```

3. 保存并运行程序。

之前的直线和圆环会被重新绘制，之后在绘制球体之前有5秒钟的时间来移动角色的位置。

你可以从本书的配套网站上下载绘制直线、圆环和球体的完整代码。

> martin says
>
> 球体很适合用来在Minecraft里制造爆炸。通过创建一个空气（AIR）方块组成的球体，我们就可以删除球体中心附近的所有方块，在世界里制造一个“空洞”。

> 挑战
>
> 我们已经见识到创建基本形状是很简单的，现在可以用代码来创造一些自己的3D艺术作品，使用直线、圆环和球体这些基本形状。

##创造一个Minecraft闹钟

在学会了如何绘制直线和圆环之后，我们就可以来学习如何编程制造一个图7-1那样的闹钟了。钟面就是一个大的圆环，每个指针都是一条直线。最困难的部分是，算出每个指针的位置，以及如何移动指针。

在冒险任务的这一部分，我们要使用三角学把真怎的角度变换成钟面上的坐标，从而计算指针指向的位置（见图7-5）。我们先用某种方块绘制出指针，再用空气（AIR）方块重新绘制让它们消失，最后在新位置重新绘制指针，这样就能让它们看起来好像在移动一样。

定义

**三角学**是数学的一个分支，专门处理三角形角度和边长的关系。查看[这个页面](https://en.wikipedia.org/wiki/Trigonometry)学习更多关于它的知识。

按照下面步骤来创造自己的时钟：

1. 打开IDLE新建一个程序文件。保存在MyAdventures文件夹里，命名为MinecraftClock.py。
2. 输入下面代码导入minecraft、block、minecraftstuff、time、datetime和math几个模块：

    ```
    import mcpi.minecraft as minecraft
    import mcpi.block as block
    import mcpi.minecraftstuff as minecraftstuff
    import time
    import datetime
    import math
    ```

3. 定义一个`findPointOnCircle()`函数。输入圆心的位置和指针的角度，这个函数返回图7-5所示的指针位置。

    ```
    def findPointOnCircle(cx, cy, radius, angle):
        x = cx + math.sin(math.radians(angle)) * radius
        y = cy + math.cos(math.radians(angle)) * radius
        x = int(round(x, 0))
        y = int(round(y, 0))
        return(x,y)
    ```

4. 连接到Minecraft并生成MinecraftDrawing对象：

    ```
    mc = minecraft.Minecraft.create()
    mcdrawing = minecraftstuff.MinecraftDrawing(mc)
    ```

5. 输入下面代码，获取游戏角色当前的位置：

    ```
    pos = mc.player.getTilePos()
    ```

6. 现在创建变量保存中心点（位置在角色上方25个方块处），钟面的半径和指针的长度：

    ```
    clockMiddle = pos
    clockMiddle.y = clockMiddle.y + 25
    CLOCK_RADIUS = 20
    HOUR_HAND_LENGTH = 10
    MIN_HAND_LENGTH = 18
    SEC_HAND_LENGTH = 20
    ```

7. 接下来，输入下面代码用`drawCircles()`函数绘制钟面：

    ```
    mcdrawing.drawCircle(clockMiddle.x, clockMiddle.y,  
                         clockMiddle.z,
                         CLOCK_RADIUS, block.DIAMOND_BLOCK.id)
    ```

8. 开始一个无限循环，之后的所有代码都是这个循环的一部分。

    ```
    while True:
    ```

9. 下一步要用`datetime.datetime.now()`函数获取电脑当前的时间。然后要把时间分割成小时、分钟和秒。由于我们的时钟只有12个小时而不是24小时，如果当前时间是下午，需要从小时里减去12（例如，如果当前时间是14:00，在钟上显示为2:00）。输入下面代码：

    ```
        timeNow = datetime.datetime.now()
        hours = timeNow.hour
        if hours >= 12:
            hours = timeNow.hour - 12
        minutes = timeNow.minute
        seconds = timeNow.second
    ```

10. 接着画出时针。角度是360度除以12小时，再乘以当前的小时数。用`findPointOnCircle()`函数算出时针顶端的x和y座标，再用`drawLine()`函数画出时针：

  ```
      hourHandAngle = (360 / 12) * hours
      hourHandX, hourHandY = findPointOnCircle( 
          clockMiddle.x, clockMiddle.y,  
          HOUR_HAND_LENGTH, hourHandAngle)
      mcdrawing.drawLine( 
          clockMiddle.x, clockMiddle.y, clockMiddle.z,
          hourHandX, hourHandY, clockMiddle.z,
          block.DIRT.id)
  ```

11. 下一步画出分针，要比时针后移一个方块的距离（z-1）：

    ```
        minHandAngle = (360 / 60) * minutes
        minHandX, minHandY = findPointOnCircle( 
            clockMiddle.x, clockMiddle.y,
            MIN_HAND_LENGTH, minHandAngle)
        mcdrawing.drawLine( 
            clockMiddle.x, clockMiddle.y, clockMiddle.z-1,
            minHandX, minHandY, clockMiddle.z-1,
            block.WOOD_PLANKS.id)
    ```

12. 然后再画秒针，要前移一个方块的距离（z+1）:

    ```
        secHandAngle = (360 / 60) * seconds
        secHandX, secHandY = findPointOnCircle( 
            clockMiddle.x, clockMiddle.y,
            SEC_HAND_LENGTH, secHandAngle)
        mcdrawing.drawLine( 
            clockMiddle.x, clockMiddle.y, clockMiddle.z+1,
            secHandX, secHandY, clockMiddle.z+1,
            block.STONE.id)
    ```

13. 等待1秒钟：

    ```
        time.sleep(1)
    ```

14. 现在我们需要重画指针来清除当前时间，只不过这次需要用AIR类型的方块。输入下面代码：

    ```
        mcdrawing.drawLine( 
            clockMiddle.x, clockMiddle.y, clockMiddle.z,
            hourHandX, hourHandY, clockMiddle.z,
            block.AIR.id)
        mcdrawing.drawLine( 
            clockMiddle.x, clockMiddle.y, clockMiddle.z-1,
            minHandX, minHandY, clockMiddle.z-1,
            block.AIR.id)
        mcdrawing.drawLine( 
            clockMiddle.x, clockMiddle.y, clockMiddle.z+1,
            secHandX, secHandY, clockMiddle.z+1,
            block.AIR.id)
    ```

15. 保存文件并运行程序，看看你工作的成果。时钟会出现在角色的上方，可以向上看或者走到侧面来看时间。注意要从钟的正面看，不然时间就是倒着走的了。

------

<u>深入代码</u>

函数`findPointOnCircle()`根据输入的圆心座标、半径和角度计算圆环上的一个点的座标（x, y）（见图7-5）。

1. 函数有`cx`、`cy`、`radius`和`angle`四个参数：

    ```
    def findPointOnCircle(cx, cy, radius,  
                          angle):
    ```

2. 圆环上的点(x, y)是用math模块里的`sin()`和`cos()`函数来计算的，乘以半径`radius`：

    ```
        x = cx + math.sin(math.radians(angle))  
                 * radius
        y = cy + math.cos(math.radians(angle))  
                 * radius
    ```
    ​	
    `math.sin()`和`math.cos()`两个函数调用时需要传递**弧度**作为参数。弧度是另一种表示角度的方式（不同于常用的0~360度），`math.radians()`函数就是用来把角度转换成弧度的。

3. 计算出来的x和y是小数，但是我们需要的是整数，`round()`和`int()`函数用来把小数取整到最近的整数：

    ```
        x = int(round(x, 0))
        y = int(round(y, 0))
    ```

4. 然后x和y的值被返回给调用程序：

    ```
        return(x,y)
    ```

    这个函数同时返回x和y两个值，所以程序在调用它的时候必须提供两个变量。

绘制钟表指针的过程用了三个步骤：

1. 用360度除以12或者60（取决于是时针、分针还是秒针）然后再乘以当前的小时、分钟或秒数来计算出指针的角度：

    ```
        hourHandAngle = (360 / 12) * hours
    ```

2. 用`findPointOnCircle()`函数计算出指针顶点的坐标(x, y)：

    ```
        hourHandX, hourHandY = findPointOnCircle( 
            clockMiddle.x, clockMiddle.y,
            HOUR_HAND_LENGTH, hourHandAngle)
    ```


    由于函数返回了两个数值，因此调用的时候需要提供`hourHandX`和`hourHandY`两个变量。

3. 画一条从圆环的圆心到指针定点的直线：

    ```
        mcdrawing.drawLine( 
            clockMiddle.x, clockMiddle.y, clockMiddle.z,
            hourHandX, hourHandY, clockMiddle.z,
            block.DIRT.id)
    ```

------



> 挑战
>
> 在真是的钟表里，时针也会随着一小时内分钟数的变化而移动。例如，如果时间是11:30，时针就会在11点和12点的正中间。我们刚才编写的Minecraft时钟的代码并不是这样的——时钟会一直保持在11点位置不动一直到时间从11:59:59变成12:00:00。
>
> 试试看你能不能改变时针角度的计算方法，自己编写一个像真正的时钟那样工作的Minecraft时钟。

##画多边形

多边形是由相互连接的边组成的二维形状。多边形可以有三条（三角形）和以上数量的条。如图7-6所示，我们可以在Minecraft世界里创造很多有趣的多边形。

虽然多边形是二维的，但是它们在制作三维物体的时候很有用，因为通过把很多多边形组合在一起，几乎可以制作出任何三维物体。一起被用来穿件三维物体的多边形又称作“面”。我们可以用这种方式创造很多令人惊讶的结构。看图7-7，里面是用很多多边形创建的曼哈顿岛轮廓线（具体的做法请看[这里](www.stufaboutcode.com/2013/04/minecraft-pi-edition-manhattan-stroll.html)）。

> 定义
>
> **面**是指某个大物体上平坦的一部分，例如，立方体的一个侧面或者鼓的上面。

我们可以用`MinecraftDrawing`里的`drawFace()`函数来创建多边形（或者一些面）。这个函数需要输入一个点座标(x, y, z)的列表，把这些点按顺序连接起来，就能创建一个完整的多边形。后面一个参数传`True`或者`False`可以决定多边形是否要被填充，最后一个参数是构成这些面的方块类型（参见图7-8）:

```
drawFace(points, filled, blockType, blockData)
```

新建一个程序来试验`drawFace()`函数，创建图7-8里的那种三角形：

1. 首先打开IDLE新建一个文件。将文件保存在MyAdventures文件夹，命名为Polygon.py。
2. 输入下面代码，导入minecraft、block和minecraftstuff三个模块：

    ```
    import mcpi.minecraft as minecraft
    import mcpi.block as block
    import mcpi.minecraftstuff as minecraftstuff
    ```

3. 连接到Minecraft，创建`MinecraftDrawing`对象：

    ```
    mc = minecraft.Minecraft.create()
    mcdrawing = minecraftstuff.MinecraftDrawing(mc)
    ```

4. 获得角色的位置：

    ```
    pos = mc.player.getTilePos()
    ```

5. 我们需要一个列表来保存多边形的定点。首先输入下面代码：

    ```
    points = []
    ```

6. 追加三个座标到列表里，这三个点连接起来就是一个三角形：

    ```
    points.append(minecraft.Vec3(pos.x, pos.y, pos.z))
    points.append(minecraft.Vec3(pos.x + 20, pos.y, pos.z))
    points.append(minecraft.Vec3(pos.x + 10, pos.y + 20,  
                                 pos.z))
    ```

7. 使用`MinecraftDrawing.drawFace()`函数来创建三角形：

    ```
    mcdrawing.drawFace(points, True, block.WOOL.id, 6)
    ```

8. 保存文件并运行程序，创建三角形。

------

<u>深入代码</u>

多边形的定点用`minecraft.Vec3(x, y, z)`来创建，`minecraft.Vec3()`是Minecraft API里用来保存三维座标(x, y, z)的方式。Vec3是三维向量（3D Vector）的缩写。

这些定点用`append()`函数添加到列表中。`append()`函数把一个新的数据项添加到列表的末尾。

------



> 挑战
>
> 想想用`drawFace()`函数还能画出什么样的形状？画一个像五角大楼那样的五边形怎么样？

##金字塔

找一张金字塔的图片好好看一下。你发现了什么？它的侧面是什么形状？这些侧面之间有什么共同点？一共有多少个面？

你大概已经知道，金字塔的各个面（除了底面）都是三角形。埃及金字塔有四个面（如果算上底面就是5个），不过它们可以有三个以上任意多个面。你有没有发现任意金字塔的底面都可以放进一个圆里？如果不明白我的意思可以看一下图7-9。

我们现在要写一个程序，用`drawFace()`和`findPointOnCircle()`两个函数来建造任意尺寸、高度和面数的金字塔：

1. 首先，打开IDLE新建一个文件。保存到MyAdventures文件夹并命名为MinecraftPyramids.py。
2. 输入以下几行代码导入minecraft、block、minecraftstuff和math模块：

    ```
    import mcpi.minecraft as minecraft
    import mcpi.block as block
    import mcpi.minecraftstuff as minecraftstuff
    import math
    ```

3. 定义`findPointOnCircle()`函数，用来计算组成金字塔的每个三角形的位置：

    ```
    def findPointOnCircle(cx, cy, radius, angle):
        x = cx + math.sin(math.radians(angle)) * radius
        y = cy + math.cos(math.radians(angle)) * radius
        x = int(round(x, 0))
        y = int(round(y, 0))
        return(x,y)
    ```

4. 连接到Minecraft并创建MinecraftDrawing对象：

    ```
    mc = minecraft.Minecraft.create()
    mcdrawing = minecraftstuff.MinecraftDrawing(mc)
    ```

5. 声明金字塔所需要的变量。金字塔的中心是角色所在的位置。下面这些变量的值决定了金字塔的尺寸（半径）、高度和面数。输入：

    ```
    pyramidMiddle = mc.player.getTilePos()
    PYRAMID_RADIUS = 20
    PYRAMID_HEIGHT = 10
    PYRAMID_SIDES = 4
    ```

6. 循环处理金字塔的每个面。之后的所有代码都要在for循环之下缩进一级：

    ```
    for side in range(0, PYRAMID_SIDES):
    ```

    > 注意
    >
    > 金字塔越大，程序运行需要的时间也越长，在Minecraft游戏中显示金字塔需要的时间也越长。如果金字塔太高，也有可能超出游戏世界的最大高度。所以请慢慢尝试，逐渐增加这些变量的值。我们可以创建巨型金字塔，但是如果太大的话可能需要一点时间才能显示出来，要耐心等待。

7. 对金字塔的每个侧面，计算三角形的边的角度，然后使用`findPointOnCircle()`函数计算x、y、z坐标。角度是用360度除以侧面的总数再乘以当前正在绘制的面的编号来计算的，如图7-10所示。输入下面代码：

    ```
        point1Angle = int(round((360 / PYRAMID_SIDES) * side,0))
        point1X, point1Z = findPointOnCircle(pyramidMiddle.x, pyramidMiddle.z, PYRAMID_RADIUS, point1Angle)
        point2Angle = int(round((360 / PYRAMID_SIDES) * (side + 1),0))
        point2X, point2Z = findPointOnCircle(pyramidMiddle.x, pyramidMiddle.z, PYRAMID_RADIUS, point2Angle)
    ```

8. 生成三角形的定点，并用`drawFace()`函数生成金字塔的一个侧面：

    ```
        trianglePoints = []
        trianglePoints.append( 
            minecraft.Vec3(point1X, pyramidMiddle.y, point1Z))
        trianglePoints.append( 
            minecraft.Vec3(point2X, pyramidMiddle.y, point2Z))
        trianglePoints.append( 
            minecraft.Vec3(pyramidMiddle.x,
                           pyramidMiddle.y + PYRAMID_HEIGHT,
                           pyramidMiddle.z))
        mcdrawing.drawFace(trianglePoints, True,  
                           block.SANDSTONE.id)
    ```

    > martin says
    >
    > 砂岩（block.SANDSTONE.id）是建造金字塔时很有用的一个方块类型，因为它看起来很像沙子，但是又有一个很重要的特点：它不受重力影响，底下没有其他方块支撑也不会掉落下来。如果你要用沙子来建造金字塔，你的游戏角色就会被沙子掩埋，需要花很长时间才能挖出来。

9. 保存文件并运行程序。你会看到一个金字塔在角色的上方出现——并且把他困在里面！

这个程序可以创建任意尺寸、有任意数量侧面的金字塔。试着改变金字塔相关的变量再重新运行程序。图7-11里有一些令人印象深刻的例子。

你也可以从本书的配套网站上下载完整的代码。

> 挑战
>
> 我们创建的金字塔没有底面。试试看你能不能生成一个恰好能填充金字塔底部的多边形。这对于一个有四个侧面的金字塔是很容易的，不过如果你的代码正确，应该也可以同样应用于有5、6、7个侧面的金字塔。

##关于二维和三维形状的更多冒险

使用`drawFace()`函数可以生成各种类型的多面体，也就是由多个平面组成的立体图形（就像我们刚才创建的金字塔），为什么不试着创造更多东西呢？

在下面这些网页可以找到很多关于多面体的例子和好点子：

* [Maths is Fun](www.mathsisfun.com/geometry/polyhedron.html)
* [Kids Math Games](www.kidsmathgamesonline.com/facts/geometry/3dpolyhedronshapes.html)


##下一个冒险

在下一个冒险任务里，我们要学习如何让Minecraft物体拥有自己的思想，跟方块交朋友，抵抗外星人的入侵。