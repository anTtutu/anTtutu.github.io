---
layout: post
title: Golang学习笔记 - 关键字和基础数据类型介绍
date: 2020-02-11
tags: golang 学习 
description: "Golang学习笔记"
---

#### 记录自己的Golang学习笔记

# 一、Golang关键字和介绍
## 25个关键字:
```
break     default      func     interface    select
case      defer        go       map          struct
chan      else         goto     package      switch
const     fallthrough  if       range        type
continue  for          import   return       var
```
#### 针对自己不太熟悉的关键字说明下，虽然以后用着会熟悉，但是提前了解不是坏事
### break:
跳出循环
```
func main() {
    //跳出循环
    for i := 0; i < 10; i++ {
        fmt.Println(i)
        if i == 6 {
            break
        }
    }
}

---------------------------------
0
1
2
3
4
5
6

Process finished with exit code 0
```

### switch case default:
switch分支流程控制语句
```
func main() {
    //写法1：
    switch i := 6; i {
    case 1, 3, 5, 7, 9:
        fmt.Println("奇数5")
    case 2, 4, 6, 8, 10:
        fmt.Println("偶数")
    default:
        fmt.Println("other", i)
    }

    //写法2：
    finger := 2
    switch finger {
    case 1:
        fmt.Println("大拇指")
    case 2:
        fmt.Println("食指")
    case 3:
        fmt.Println("中指")
    case 4:
        fmt.Println("无名指")
    case 5:
        fmt.Println("小指")
    default:
        fmt.Println("无效的数字")
    }

    //写法3：
    age := 30
    switch {
    case age < 24:
        fmt.Println("学习阶段")
    case age >= 24 && age < 60:
        fmt.Println("工作阶段")
    case age > 60:
        fmt.Println("退休阶段")
    default:
        fmt.Println("活着真好")
    }
}

---------------------------------
偶数
食指
工作阶段

Process finished with exit code 0
```

### func:
函数
```
//1、带返回值的函数
func add(a, b int) int {
    return a + b
}

//2、多个返回值的函数
func vals() (int, int) {
    return 3, 7
}

//3、没返回值的函数
func query () {
    fmt.Println("query success.")
}
```

### interface:
接口
```
type Dog interface {
    Category()
}

type Ha struct {
    Name string
}

func (h Ha) Category() {
    fmt.Println("狗子")
}

func main() {
    h := Ha{"二哈"}
    h.Category()
    test(h)
}

func test(a Dog) {
    fmt.Println("成功调用啦")
}

---------------------------------
狗子
成功调用啦

Process finished with exit code 0
```

### select:
A "select" statement chooses which of a set of possible send or receive operations will proceed. It looks similar to a "switch" statement but with the cases all referring to communication operations.  
一个select语句用来选择哪个case中的发送或接收操作可以被立即执行。它类似于switch语句，但是它的case涉及到channel有关的I/O操作。
```
//select基本用法
select {
case <- chan1:
// 如果chan1成功读到数据，则进行该case处理语句
case chan2 <- 1:
// 如果成功向chan2写入数据，则进行该case处理语句
default:
// 如果上面都没有成功，则进入default处理流程
}
```

### defer:  
A "defer" statement invokes a function whose execution is deferred to the moment the surrounding function returns, either because the surrounding function executed a return statement, reached the end of its function body, or because the corresponding goroutine is panicking.  
一个defer语句在函数返回、函数结束或者对应的goroutine发生panic的时候defer就会执行。  
在golang中，我们使用defer语句来进行一些错误处理和收尾工作，它的作用类似java里面finally关键字的作用
```
func CopyFile(dstName, srcName string) (written int64, err error) {
    src, err := os.Open(srcName)
    if err != nil {
        return
    }
    defer src.Close()

    dst, err := os.Create(dstName)
    if err != nil {
        return
    }
    defer dst.Close()

    // other codes
    return io.Copy(dst, src)
}
```
列举2个简单demo:  
demo1: 理解延迟
```
func main() {
    // demo1
    defer fmt.Println("hello")
    fmt.Println("world")
}

--------------------------------
world
hello

Process finished with exit code 0
```
demo2: 理解defer、return、返回值的顺序  
1、多个defer的执行顺序为“后进先出”；  
2、defer、return、返回值三者的执行逻辑应该是：return最先执行，return负责将结果写入返回值中；接着defer开始执行一些收尾工作；最后函数携带当前返回值退出。
```
func main() {
    // demo2
    fmt.Println("c return:", *(c())) // 打印结果为 c return: 2
}

func c() *int {
    var i int
    defer func() {
        i++
        fmt.Println("c defer2:", i) // 打印结果为 c defer: 2
    }()
    defer func() {
        i++
        fmt.Println("c defer1:", i) // 打印结果为 c defer: 1
    }()
    return &i
}

---------------------------------
c defer1: 1
c defer2: 2
c return: 2

Process finished with exit code 0
```

### go:
轻松开启高并发，一直都是golang语言引以为豪的功能点。golang通过goroutine实现高并发，而开启goroutine的钥匙正是go关键字。开启并发执行的语法格式是：
```
func main() {
    go testName()
    fmt.Println("main method")
    time.Sleep(time.Second)
}

func testName () {
    fmt.Println("test method")
}

---------------------------------
main method
test method

Process finished with exit code 0
```

### map:
map是映射关系容器为map，其内部使用散列表（hash）实现，一种无序的基于key-value的数据结构，Go语言中的map是引用类型，必须初始化才能使用。
```
func main() {
    //1、声明map
    scoreMap := make(map[string]int, 8)
    scoreMap["张三"] = 90
    scoreMap["小明"] = 100
    fmt.Println(scoreMap)
    fmt.Println(scoreMap["小明"])
    fmt.Printf("type of a:%T\n", scoreMap)

    //2、遍历map
    for k, v := range scoreMap {
        fmt.Printf("key[%s] - value[%d]\n", k, v)
    }

    //3、遍历map只要key或者value
    for k := range scoreMap {
        fmt.Printf("key[%s]\n", k)
    }

    for _, v := range scoreMap {
        fmt.Printf("value[%d]\n", v)
    }

    //4、判断某个key是否存在  特殊写法：value, ok := map[key]
    // 如果key存在ok为true,v为对应的值；不存在ok为false,v为值类型的零值
    v, ok := scoreMap["张三"]
    if ok {
        fmt.Println(v)
    } else {
        fmt.Println("查无此人")
    }

    //5、删除某个key
    //将小明:100从map中删除
    delete(scoreMap, "小明")
    for k,v := range scoreMap{
        fmt.Printf("key[%s] - value[%d]\n", k, v)
    }
}

---------------------------------
map[小明:100 张三:90]
100
type of a:map[string]int

key[张三] - value[90]
key[小明] - value[100]

key[张三]
key[小明]
value[90]
value[100]

90

key[张三] - value[90]

Process finished with exit code 0
```