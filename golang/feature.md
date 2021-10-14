# Go 特性自查
## 一、判断题
1. 函数和方法没区别，只是同一个东西的不同叫法


## 二、简答题
1. 如何用 Go 实现多态
2. Go 语言的 error 类型是如何实现的

## 三、代码实例

### 关于指针与切片
以下代码的两个 `change` 函数造成的改动，都能成功生效吗？  
如果不能，分别是因为什么原因？怎么写才是对的？  
```
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