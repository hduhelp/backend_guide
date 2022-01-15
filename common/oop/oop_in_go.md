# 谈谈Go与面向对象

本文面向已经会 Go 的基础语法并基本掌握一门面向对象语言的读者。

先抛个问题：Go 是不是面向对象语言？

官方的回答是：「Yes and no」。

Go 语言可以做到绝大多数面向对象语言的特性，但它不是一门「标准」的面向对象语言，它没有「type hierarchy」。

一开始，我觉得它以自己的奇怪甚至近乎「妖魔」的方式与面向对象打了个擦边球；
后来，我反而觉得完美面向对象就应该是这样灵活的，现在的所谓的「标准面向对象」，反而是一种不完美的实现。

下面来谈谈 Go 如何实现面向对象。因为 Java 是比较「规整」的面向对象实现，所以下文会多次与 Java 进行对比。

## 封装

封装指隐藏对象的属性和实现细节，仅对外提供公共访问方式。

这个应该不用多讲，Go 使用大小写控制可访问性（包外），大写代表导出（public），小写代表私有（private）。

方法暴露，使用 `receiver` ，类型、变量、方法的可见性规则都是用大小写。

```go
type Person struct {
    Name string // 大写，导出，包外可见，public
    age  int    // 小写，私有，仅包内可见，private
}

// Speak 暴露公有方法
func (p *Person) Speak() {
    fmt.Println("Hello, my name is", p.Name)
}

// SetAge GetAge 方法略
```

为类增加方法，Go 与 Java 最大的不同是，Java 的方法是在类内的，而 Go 在类外，有点像 struct 上贴了一个个狗皮膏药的感觉。  
起初可能会不习惯，但这让方法的添加变得更灵活，receiver 的设计更使得所有类都能拥有方法。

Go 的 receiver 的设计更符合底层的面向对象的思想：为某一类事物，附加一些行为。

Go 的任何非内置的「定义类型」，都能拥有方法；而 Java，只有类才能拥有方法，或者说，想在一个东西上施加操作，必须定义一个类，显得臃肿。

## 继承

我对继承的理解是，子类拥有父类的所有属性和方法。

Go 的继承使用类似组合的方式实现，与标准面向对象实现最大的区别在于，基类变量不能引用子类变量。

```go
type Man struct {
	Person              // 内嵌，继承
	otherField string   // 组合
}

man := Man{"bird"}
_ = man.Person.Name
_ = man.Name    // 省略 Person 匿名字段
man.Speak()
```

如上，在 Man 中加入一个只有类型没有名字的 Person 属性，就是内嵌。Go 的一大特性就是，内嵌字段可以被省略。详见代码。

所以，Man 可以省略 Person，**直接访问 Person 的所有属性并调用其方法**。所以看起来，就像继承一样。

总结一下就是，结构体内嵌匿名变量就是继承，无名就是组合。

和 Java 的区别在于， `var p person = Man{}` 是不可行的。父类变量无法引用子类对象（但这并不意味着 Go 没有多态）。
所以 Go 的继承是不满足「里氏替换」原则的。

我觉得这个特性挺好，逼迫开发者少使用继承，多面向抽象编程。

如何实现对方法的重写（Override），即子类覆盖父类的同签名方法，以实现不同表现？

稍微岔开一下，Go 没有「重载」，即同函数名，函数签名却不同。所以这个问题其实是，Go 如何覆盖父类同名方法？

这时候就体现 Go 的设计哲学了， less is more。你都不需要知道什么是重写：

没有那么多的术语和要记的东西，也不需要新的关键字，一切都是自然而然：
子类想用父类的，直接继承了不用管；子类想拥有不同的表现形式（行为），那就自己定义一个。

怎么自己定义一个方法呢，很简单，定义一个以子类作为 receiver 的方法。正如前面的继承，没有新增任何关键字。

简单理一下逻辑，如果调用方法的时候，子类本身拥有该方法，那就直接调用；如果没有，就看看父类有没有，一层层找上去。

如何实现多继承？想继承谁，内嵌什么类型就行。

至此，Go 非常优雅且简单地实现了继承。

这里还有个面向对象的原则，「使用组合而不是继承」，通俗继承的坏处很多，比如父类改了子类就会被动跟着改动。
所以 Go 使用了组合的方式来实现了继承，是不是天生规避了一些继承的缺点？
以及继承会暴露父类的实现细节，这个问题 Go 也存在。

## 多态

Go 的多态是用 interface 实现的， interface 是 Go 语言的灵魂之一，为静态的 Go 语言增添了动态性。

我理解的接口，是一种「约定」，接口的方法，约定了一系列操作，某样东西能完成这个操作，它就实现了这个接口。

下面这段代码演示了许多面向对象的内容，其中，末尾的 `AllSpeak` 函数是多态的展示。

```go
type Speaker interface {
	Speak()
}
type Person struct {
	Name string
}
func (p Person) Speak() {
	fmt.Println("Hello, my name is", p.Name)
}
type Dog struct {
	Name string
}

type SingleDog struct {
	Dog
}

func (d Dog) Speak() {
	fmt.Println("Wang Wang, my name is", d.Name)
}
func TestInf(t *testing.T) {

	var s Speaker
	p := Person{"Tom"}
	s = p       // 接口变量能指向实现了该接口的对象
	s.Speak()

	person := Person{"Tom"}
	dog := Dog{"Jerry"}
	singleDog := SingleDog{Dog{Name: "SingleDog"}}
	speakers := []Speaker{person, dog, singleDog}
	AllSpeak(speakers)
}

// AllSpeak 多态演示
func AllSpeak(s []Speaker) {
	for _, v := range s {
		v.Speak()
	}
```

Go 语言的接口是非常灵活的，不需要显示声明，Java 需要 `implements`，而 Go 是 DuckType。

什么是 DuckType？鸭子会嘎嘎叫，所以会嘎嘎叫的就能当成鸭子。

翻译成编程语言，接口是一个约定，遵循了这个约定，就实现了接口。这个遵循，只要某个类型的方法签名是某接口定义的方法签名的超集就行。

所以，一定记住，在编码层面，方法先于接口，即先有了方法，才有了是否实现该接口的判断。而不是 Java 的先明确实现什么接口，挖好坑，再去填。完全相反。

如果实现多态？接口类型的变量，可以引用实现了该接口的任何类型的变量。

在 `AllSpeak` 函数中，形参是 `Speaker` 接口的切片，实参却是 Person、Dog、SingleDog 类型。
为什么能调用成功呢？因为 `Person` 和 `Dog` 都拥有 `Speak()` 方法，满足了 `Speaker` 接口定义的所有函数的签名。

那为什么 SingleDog 类型也能传参成功，是不是可以理解为，SingleDog 继承了 Dog 的接口？似乎变得复杂起来了。

Go 没有那么多术语。紧抓两点：①编码时，先看方法，再看接口 ②某类型拥有的方法是某接口方法的超集，就实现了该接口

所以，是 SingleDog 先「继承」了Dog的 `Speak()` 方法，而后该方法刚好又满足了 `Speaker()` 的约定，自然就实现了该接口。

许多语言的接口与继承，对于开发者而言其实是迷惑的，可能经常分不清到底该用哪个，而 Go 不会。
许多面向对象语言的继承，是被滥用的，而 Go，压根没开这个门，你只能用接口。
它以一种非常灵活的方式，实现了多态，并且接口的设计，对「依赖反转（面向抽象编程）」天然友好。

interface 真的是个好东西，当你不知道怎么设计更优雅的时候，它总能给你带来惊喜。


