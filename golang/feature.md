# Go 特性自查

**没标「略难」的，都建议搞懂**

## 一、判断题（做）
1. 函数和方法没区别，只是同一个东西的不同叫法
2. Go 需要显式实现接口所有方法才算实现该接口
3. 基础类型（数值、字符串、bool）可以作为接收者
4. （略难）指针变量一定能调用 receiver 为非指针的方法
5. （略难）非指针变量一定不能调用 receiver 为指针的方法


## 二、简答题（做）
1. 如何用 Go 实现多态
2. Go 语言的 error 类型是如何实现的
3. 重载、重写、（接口）实现都是什么意思，Go 有这些吗？能给出代码例子吗？
4. 如何用 Go 实现继承


## 三、代码实例（看）
### 3.1 用 Go 实现人民币类型
分别用结构体和 `type` 实现，更推荐结构体（但建议两种实现方式都看一下），理由如下：
1. type 不能由 proto生成
2. type 不能继承下方特性
3. type 不方便拓展

<details>
<summary>点击查看 struct 结构体实现</summary>
```go
type RMB struct {
	int
}

// 实现 Stringer 接口
func (rmb RMB) String() string {
	return fmt.Sprintf("￥%d", rmb.int)
}

func Main_3_01_b() {
	pricePencil := RMB{2}
	//pricePen := RMB{10}
	// %v 自动调用 Stringer 方法，格式化输出
	fmt.Printf("The price of Pencil is %v\n", pricePencil)
	// output: The price of Pencil is ￥2
}


```
</details>
#### 3.1.2 

<details>
<summary>点击查看 type 实现</summary>
```go
type RMB int

// 实现 Stringer 接口
func (rmb RMB) String() string {
	return fmt.Sprintf("￥%d", rmb)
}

func Main_3_01() {
	pricePencil := RMB(2)
	pricePen := RMB(10)
	// %v 自动调用 Stringer 方法，格式化输出
	fmt.Printf("The price of Pencil is %v\n", pricePencil)
	// output: The price of Pencil is ￥2

	// type ，若底层基本数据类型一致，可直接运算
	fmt.Printf("Pencil is %v cheaper than pen\n", pricePen-pricePencil)
	// output: Pencil is ￥8 cheaper than pen
}

```
</details>

## 四、代码纠错/辩析（做）

### 4.1 纠错：关于指针与切片
以下代码的两个 `change` 函数造成的改动，都能成功生效吗？  
如果不能，分别是因为什么原因？怎么写才是对的？ 
<details>
<summary>点击查看代码详情</summary>
``` go
type personInfos struct {
	Filename string
	Persons []Person
}

type Person struct {
	Name string	//标准名
	Alias []string	//别名，可以是邮箱、QQ号等
	Submit bool // 是否已提交
}


func (p personInfos)ChangeSomeThingV1() {
	for _, person := range p.Persons {
		if person.Name == "随便写个判断意思一下" {
			person.Submit = true
		}
	}
}

func (p *personInfos)ChangeSomeThingV2() {
	for _, person := range p.Persons {
		if person.Name == "随便写个判断意思一下" {
			person.Submit = true
		}
	}
}

```

</details>

### 4.2 结构体嵌套字段的跨名冲突问题

<details>
<summary>点击查看代码</summary>
```go
//Address 地址结构体
type Address struct {
    Province   string
    City       string
    CreateTime string
}

//Email 邮箱结构体
type Email struct {
    Account    string
    CreateTime string
}

//User 用户结构体
type User struct {
    Name   string
    Gender string
    Address
    Email
}

func main() {
    var user3 User
    user3.Name = "pprof"
    user3.Gender = "女"
    // user3.CreateTime = "2019" //ambiguous selector user3.CreateTime
    user3.Address.CreateTime = "2000" //指定Address结构体中的CreateTime
    user3.Email.CreateTime = "2000"   //指定Email结构体中的CreateTime
}
```

</details>