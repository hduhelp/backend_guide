# 面向对象思想入门
## 一、形象解释什么是面向对象
### 1.1 小例子
先来个例子，简单了解一下面向过程和面向对象的区别。

> 有一天你想吃鱼香肉丝了，怎么办呢？你有两个选择：  
1、自己买材料，肉，鱼香肉丝调料，蒜苔，胡萝卜等等然后切菜切肉，开炒，盛到盘子里。  
2、去饭店，张开嘴：老板！来一份鱼香肉丝！  
> 看出来区别了吗？1是面向过程，2是面向对象。

*文章来源：[什么是面向对象（OOP） - 简书](https://www.jianshu.com/p/7a5b0043b035)*

*本文也就这段话是复制的了，本来想做个资源帖的收集，写着写着发现，还是自己写的东西有趣，干脆就不参考了。
至于那些非常专业的文章，大家在看了我的文章后，建立了对面向对象思想的理解，也就看得懂其他文章了，自行查阅就好。*

狭义地理解，面向过程就是，**以过程为中心**，就像做数学题那样，定义了解题的一个个步骤。  
面向对象就是，抽象出一个个的模型，已经预定义了这个类型有哪些属性，能做些什么，直接调用就好。 
### 1.2 面向对象再解释

上面我们通过一个例子感性得理解了面向对象的整体，下面我们继续举例，带大家理解面向对象的细节。  

直接来个实际的例子，例如，我们要实现一个查询学生电话号码的接口。  

**面向过程的实现方法：**  
写个 `getStudentInfo` 函数，形参是学生的唯一标识符（如学号）。  
先不考虑数据库，那么我们需要维护一个学生信息的数组，然后一个个比对。  

如果我们又要实现，查询学生寝室号、辅导员，又要再写两个函数吗？而且这些函数的形参，全是学生ID。  

有没有更好的解决方案呢？

这时候你可能会质疑，我学过 C 啊，你讲的这玩意，压根不用定义函数啊？  

我直接进行一个结构体的定义，一个 `Student` 结构体，然后把学号、姓名、电话、辅导员信息都塞里面，
想查询信息的时候，只要用 `student.name` 就能获取到姓名了。  

其实，这便是一种非常简单的面向对象的思想。前面讲过，面向对象，在我的理解中，最浅层的，
就是把一些属性和能做什么聚在一起，形成一个类。而 C语言 的结构体，就是做到了属性的聚合。  

继续给需求，如果主人又来任务了，这时候给了个，「上课签到记录」的接口，怎么办？
结构体成员变量实现这个功能吧？

一个声音浮现在脑海：用函数啊！直接定义一个签到函数！

可是函数，它就，不优雅了啊，它是面向过程！

那么，我们既然已经把属性塞到结构体里面了，采用 `变量.属性名` 的方式获取。  
我们可不可以，把函数也塞到结构体里面呢？采用 `变量.函数名` 的方式获取？

大聪明鬼，这就是 Go 语言面向对象的简单实现了！！！

把属性和行为塞（封装）到结构体里面之后，和之前的面向过程有什么不一样？

之前的函数调用，学生属于形参，是传进去的。 `getInfo(user)` ，
而现在，学生是调用方，是主体， `user.GetInfo()`。

换句话说，之前的函数调用， 操作的是外部的数据信息。而现在，变量（属性）操作（方法）
成为了类型的一部分，是固有的。一旦这个类型建立了，那它对应的变量什么的，也都直接附带上了。
同时，利用对象调用方法，更像是一种「操作」，什么叫操作？就是，我这个对象，里面有些数据，
通过调用某个方法，修改了对象内的一些信息，「操作」了一下。就这么先这么狭义理解。


有个小细节需要注意，当函数和对象结合的时候，它就不叫函数了，叫「方法」。任何语言都是如此。  

不同语言面向对象的实现方法都不一样，大部分面向对象的语言，都是用 `class` 关键字实现的，
Go 语言的实现比较奇葩，是用 `Struct` 和 `receiver` 来结合实现的，它不是纯粹的面向对象语言。  

所以在初学面向对象思想的时候，不建议一开始就拿 Go 语言学习面向对象，容易走火入魔。
比较建议看看 `C++` 或 `Java` 的面向对象的实现。不过话说回来，虽然 Go 的面向对象实现有点显得歪门邪道，
但是，当它和 Go 的接口结合起来的时候，一切就都会变得非常有趣。
我更愿意称 Go 语言的面向对象是灵活的面向对象。

## 面向对象概念解释
首先是 `类` 的概念。类，你可以理解为，类型的意思。
或者说，他们是同一类的，他们有相似的行为（函数），相似的特征（属性），那他们就是一类。
比如，人类，动物类，学生类。  

然后是 `对象` 的概念，这个对象不是那种，冬天了可以抱抱的对象。是 `类` 的 `实例`。  
类，概括出了同一种类型的事物，对象，就是这一类事物中，具体的某个个体。  
比如，`人类` 是个 `类`，张三就是个对象，或者说 `实例`。利用类建立对象的过程，就叫 `实例化`。

然后是 `继承`。我不说啥意思，就举例子，你自己体会。哺乳动物类继承动物类，人类继承哺乳动物类，
男人类继承人类，爸爸类继承男人类。

A类 `继承` B类，表明，A类拥有B类的所有属性（特征）和方法（行为、能做的事）。并且自己可以加些新的属性和方法。
比如，女人类拥有人类的一切属性，又能做任何人能做的事，她又可以有些特殊的属性或方法。
而程序员类不是继承了男人类或女人类的。被继承的类叫 `父类`，另外一个叫 `子类`。

再解释一下什么叫 `封装`。先贴个专业解释：
> 通过对象隐藏程序的具体实现细节，将数据与操作包装在一起，
> 对象与对象之间通过消息传递机制实现互相通信（方法调用），
> 具体的表现就是通过提供访问接口实现消息的传入传出。

麻了吧？

其实前面讲的那些，把属性和方法放在一起，就是封装的一部分。也就是被引用的内容中提到的
「将数据和操作包装在一起」。数据就是属性，操作就是方法、行为。

什么叫隐藏实现细节呢？简单说两点：

第一，C语言的结构体，任何类型都是可以直接访问的，
我想让外部调用者没法直接访问某些数据怎么办？用类可以实现。  
第二，别人写好的东西，我才不管你具体怎么实现的，我只要知道怎么用就行。
甚至，我只要知道哪个方法能查成绩就行，我就这么写了，
如果你想修改查成绩的内部实现，和我无关，因为接口已经定死了。

其实这个和函数封装差不多。

最后解释一下，`多态`。

还是举例子吧，前面讲到，我想查询学生的基本信息，写了个方法。

现在，我又想查询老师的基本信息了，再写个方法？

那这样的话，要针对不同的类型写不同的方法，但他们实际执行的操作差不多啊？
而且，我如果想用一个函数就能查询老师或学生的信息，要写一堆 `if` 吗？

能优雅一点吗？

面向对象，类似的属性和操作，都能封装。他们都叫「查询某个人的基本信息」。

其实，我们可以定义一个「学校师生」父类，「学生类」和「教职工」类分别继承它。
「学校师生」类可以定义一个「查询信息」方法，然后另外两个类 `重写` 这个方法。
即，父类有个 `getInfo()` 的方法，两个子类也各有一个 `getInfo()` 的同名方法。

这么做有什么好处？怎么还不讲多态？？？

多态有很多种，这里讲一种。比如，你可以定义一个基类的变量（指针），它是可以引用（指向）
所有父类的。换句话说，你可以定义一个「学校师生」类的变量，
然后查询信息的时候，直接调用 `父类.getInfo()`
无论你是通过「学生类」还是「教职工」类的对象调用的这个方法，
编译器都能找到对应的方法，即，它知道该调用哪个查询信息的方法，即，
如果是学生调用这个方法，就查询学生信息，如果是老师就查询老师信息。非常的阳间。

未完待续。。。
