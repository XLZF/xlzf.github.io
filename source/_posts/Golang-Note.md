---
title: Golang Note
date: 2021-10-31 19:47:01
tags: [Go]
excerpt: 本文记录Golang环境配置和一点基础知识。
categories: Golang
index_img: https://gitee.com/MyHexo/blog-image/raw/master/Go.jpeg
---

## 环境部署

[Go语言入门教程，Golang入门教程（非常详细） (biancheng.net)](http://c.biancheng.net/golang/)

### `Go`编译器

[Downloads - Go](https://golang.google.cn/dl/)

安装位置：

`C:\Program Files\Go`

![image-20210502222044326](https://gitee.com/MyHexo/blog-image/raw/master/img/image-20210502222044326.png)

### 其他配置

* 创建一个任意的目录 `GolangProjects`,目录结构如下

  ```
  GolangProjects
  
  --bin
  
  --- crm.exe //主程序入口文件
  
  --pkg
  
  --- commlib.A //包文件
  
  --src
  ```

### 配置环境变量

* 添加环境变量 `GOROOT` : `C:\Program Files\Go`   *Go 编译器的安装目录*
* 添加环境变量 `GOBIN` : `E:\Code\GoLangProjects\bin` *方便直接 Go*
* 添加环境变量 `GOPATH`: `E:\Code\GoLangProjects`

### `vs code` 配置环境

#### tools 怎么来的

* github中文：https://github.com/Go-zh/tools.git

* github英文：https://github.com/golang/tools.git

  上述俩 GitHub 地址没有通天的手段基本上是整不下来的，这个时候 `码云`可以出来了

  ![image-20210519002803186](https://gitee.com/MyHexo/blog-image/raw/master/img/20210519002835.png)

  把上述的那俩git地址都挪到码云里，这个方便本地clone

  ![image-20210519003137754](https://gitee.com/MyHexo/blog-image/raw/master/img/20210519003137.png)

#### Clone 无需多言

推荐使用：SourceTree ,主要界面看着挺爽

#### 怎么摆放

在本地`go_path` 目录下新建文件夹，如下结构

``` go
-- go path
--- bin
--- pkg
--- src
---- github.com
------ Go-zh
------- tools //https://github.com/Go-zh/tools.git
---- golang.org
----- x
------ mod //https://github.com/golang/mod.git
------ sys //https://github.com/golang/sys.git
------ tools //https://github.com/golang/tools.git
------ xerrors //https://github.com/golang/xerrors.git
```

#### 安装命令

``` go
打开 go-path 目录的下的src 目录，地址栏里输入 cmd ，启动终端，输入如下命令：

1. go install golang.org/x/tools/cmd/guru
2. go install golang.org/x/tools/cmd/gorename
3. go install golang.org/x/tools/cmd/fiximports
4. go install golang.org/x/tools/cmd/gopls 
5. go install golang.org/x/tools/cmd/godex
```

注意：

我个人在第4点出错了，原因是因为英文官方的tools中按照位置找过去没有gopls；

百度过后才下载的中文社区的，由此才看到前面的目录结构。

所以到这要安装`gopls`,执行如下命令：

``` go
go install github.com/Go-zh/tools/cmd/gopls
```

#### `vscode`运行代码

* 一般姿势：

  启动vscode终端，cd 到 `go-path` 目录下的`src`,再找到指定指定`go`文件，例如：`go run app.go`

* 进阶姿势：

  启动vscode扩展，安装 code run 插件，安装按完重启编辑器，只需要右键选择code run 即可，或者快捷键 `ctrl + alt + n`

### 运行代码

`src` 目录下添加go文件

-`src`

--`crm`

---`app.go`

``` go
-- app.go

package  main

import "fmt"

func main()  {
	fmt.Printf("Hello Go~ 叫爸爸~")
}
```

--`commlib`

---`page`

```go
package commlib

func Add(i int ,j int)int {
	return i+j
}
```


运行有三种方式：

	进入项目目录执行：

* `go run app.go`
* `go build app.go` 执行完此命令后，会在同级目录下面添加同名exe`app.exe`可执行程序，然后`cmd`中执行`exe`程序即可。与第一种方式相同，只是`go run` 相当于一起把此方式中的两步骤一次执行完毕。
* `go install` 执行完命令后，会在外面那个`Bin` 文件夹生产一个`exe`可执行文件，`crm.exe`,也可以在`cmd`命令中执行，于上面第二种方式的区别在于，会在`Bin`文件夹中生成`exe`可执行文件。但是如果生成的项目不是主程序入口、只有方法的话，就会生成包文件`commlib.A`文件，类似于C# 中类，存放在bin文件夹的同级目录`pkg`文件夹中。

## `Go` 包管理

### 初识包管理

* 一个文件夹也可以称之为一个包
* 在文件夹（包）中可以创建多个文件
* 在同一个包下的每个文件中必须指定`包名称且相同`

###  包的分类

* `main`包，如果是`main`包，必须写一个 `main`函数，以此作为程序的入口，又称为主函数，编译生成后会生成一个可执行文件。
* 非`main`包

### 调用包

调用包文件内的方法：

```go
package  main

import (
	"commlib" //这里会引入包
	"fmt"
	"strconv" //引入包
)

func main()  {
    //println 自带输出换行
	fmt.Println("Hello Go~ ")
	//调用
	var num int = commlib.Add(1,6)
    //strconv.itba()方法，int 转 string
	fmt.Println(strconv.Itoa(num))

}
```

``` go
package commlib

func Add(i int ,j int)int {
	return i+j
}
```

### 包定义规范

* 定义包文件之后，包内的函数首字母必须得大写
* 大写和小写的区别：
  * 大写：类似于C# 中的 **public** 修饰符，包的外部可以进行调用
  * 小写：类似于C# 中的 **internal**修饰符，仅限于包内部使用

### 截图

![image-20210505225640086](https://gitee.com/MyHexo/blog-image/raw/master/img/image-20210505225640086.png)

## 基础知识

### 输出

在终端输出前置语言例如：请输入账户名或者密码等等...

* 内置函数
  * `print `
    * 默认打印两个`print` 是连在一起的，如果想要达到换行效果需要在内容尾部加上`\n`
  * `println`
    * 默认打印两个`println` 是自带换行效果的
* `fmt`
  * `fmt.print` 
    * 效果通内置函数 print 相仿
  * `fmt.println`
    * 同上，平时推荐使用此方法
  * `fmt.println`扩展：
    * `fmt.println("我叫","Harris","年龄","18")` 中间自带空格
    * 格式化显示：
      * `fmt.printF()` 
* 在以上两种方式的输出中推荐使用 `fmt`,原因：
  * `Golang`官方给出的回答是，在以后的规划中，不保证内置函数可以继续使用，为防止后面禁用内置函数后，代码出问题。

### 注释

* 单行注释 //
* 多行注释 /*  */
* `GoLand` 快捷键 `Ctrl+?`

### 变量

* **概述**
  * Go 是静态类型语⾔，不能在运⾏期改变变量类型
  * 使⽤关键字 var 定义变量，⾃动初始化为零值。如果提供初始化值，可省略变量类型，由 编译器⾃动推断。
  * **声明的变量必须使用** **如果声明不使用，则报错**  *编译器会将未使⽤的局部变量当做错误*

* **变量的作用域** 

  * 全局变量：没有写在函数内的变量，称之为全局变量，个人理解。

    ``` go
    var name string = "Harris"
    var name = "Harris"
    var {
        v1 = "a"
        v2 = "b"
        v3 = 18
        v4 int
    }
    name := "Harris" //这种不行滴
    ```

  * 局部变量：函数内的变量 *局部就和C#差不多了*

    ``` go
    func main(){
        var name string = "Harris"   
        name := "Harris"    
        var name = "Harris"    
        var{
            name = "Harris"
            age = 18
        }
    }
    ```

  * 多变量赋值时，先计算所有相关值，然后再从左到右依次赋值

    ``` go
    data, i := [3]int{0, 1, 2}, 0
    i, data[i] = 2, 100 // (i = 0) -> (i = 2), (data[0] = 100)
    fmt.Println(i,data)
    //结果：2 [100 1 2]
    ```

  * 特殊只写变量 "_"，⽤于忽略值占位

    ``` go
    func test() (int, string) {
     	return 1, "abc"
    }
    
    func main() {
         _, s := test()
         fmt.Println(s)
        //结果：abc
    }
    ```

  * 编译器会将未使⽤的局部变量当做错误

    ``` go
    func main() {
     	i := 0 // Error: i declared and not used。(可使⽤ "_ = i" 规避)
    }
    ```

### 常量

* 常量值必须是编译期间固定的数字、字符串、布尔值

  ``` go
  const x,y int = 1,2     //多常量初始化
  
  const z = "Hello World" //类型推断
  
  const{
      a,b = 1,2
      c bool = true
  }
  
  func main(){
      const x = "xxxx" //这个时候如果没有使用上述已经定义好的常量，不会报错
  }
  ```

* 常量可以使用在编译期间确定的函数

  ``` go
  const a,b int =1,2
  
  const c = "abc"
  
  const x = len(c)
  
  const y = unsafe.Sizeof(x) //这个方法是用来计算字节的，64为系统中，字符串和整形的字节一个都是8
  
  fmt.Println(a)
  
  fmt.Println(b)
  
  fmt.Println(c)
  
  fmt.Println(x)
  
  fmt.Println(y)
  
  //结果：
  /*
  1
  2
  abc
  3
  8*/
  ```

### 枚举

* 关键字 iota 定义常量组中从 0 开始按⾏计数的⾃增枚举值。

  ``` go
  const(
      Sunday = iota   //0
      Monday          //1,通常省略后面的表达式
      Tuesday			//2
      Wednesday		//3
      Thursday		//4
      Friday			//5
      Saturday		//6
  )
  const (
      _ = iota // iota = 0
      KB int64 = 1 << (10 * iota) // iota = 1  << 是左移运算符
      MB // 与 KB 表达式相同，但 iota = 2
      GB
      TB
  )
  
  fmt.Println(KB,MB,GB) //1024 1048576 1073741824
  ```

* 在同⼀常量组中，可以提供多个 iota，它们各⾃增⻓。

  ``` go
  const (
      A, B = iota, iota << 10 // 0, 0 << 10
      C, D // 1, 1 << 10
  )
  
  fmt.Println(A,B,C,D) //0 0 1 1024
  ```

* 如果 iota ⾃增被打断，须显式恢复

  ``` go
  const (
      A = iota // 0
      B // 1
      C = "c" // c
      D // c，与上⼀⾏相同。
      E = iota // 4，显式恢复。注意计数包含了 C、D 两⾏。
      F // 5
  )
  
  fmt.Println(A,B,C,D,E,F)//0 1 c c 4 5
  ```

* 可通过⾃定义类型来实现枚举类型限制

  ``` go
  type Color int
  
  const (
       Black Color = iota
       Red
       Blue
  )
  
  func test(c Color)Color {
      return c
  }
  
  func main() {
       c := Black
       fmt.Println(test(c))     // 0
       x := 1
       test(x) // Error: cannot use x (type int) as type Color in function argument;写的时候就会报错没法转换
  }
  ```

### 数据类型

![image-20210515232851239](https://gitee.com/MyHexo/blog-image/raw/master/img/20210515232851.png)

* 空指针值 nil，⽽⾮ C/C++ NULL。

#### 引用类型

##### 切片(Slice)

实现类似动态数组的功能

``` go
package commlib

import "fmt"

func SliceTest1()  {
	x:=make([]int,0,5)//声明一个容量为5的切片

	for i:=0;i<8;i++ {
		x= append(x,i) //向切片内追加数据，当超出容量后，自动分配更大的容量空间
	}

	fmt.Println(x)
}
```

输出结果：

![image-20210514230715956](https://gitee.com/MyHexo/blog-image/raw/master/img/image-20210514230715956.png)

##### channel 通道

![image-20210519220142304](https://gitee.com/MyHexo/blog-image/raw/master/img/20210519220142.png)

### 表达式

#### 常用

``` go
package commlib

import "fmt"

//ExpressionTest
func ExpressionTest()  {

	//if
	fmt.Println("******IF******")
    
	x:=100
	y:=6

	if x > 0{
		fmt.Println("x+")
	}else if x < 0{
		fmt.Println("x-")
	}else {
		fmt.Println("0")
	}

	// swich
	fmt.Println("******swich******")
    
	switch {
		case x > 0:
			fmt.Println("100")
		case x<0:
			fmt.Println("250")
		default:
			fmt.Println("俩250")
	}

	// for
	fmt.Println("******for++******")
    
	for i:=0;i<5;i++ {
		fmt.Println(i)
	}
    
	fmt.Println("******for--******")
    
	for i:=5;i>1;i--{
		fmt.Println(i)
	}
    
	fmt.Println("******相当于while(x<10)******")
	
    for y<10{

		y++

		fmt.Println(y)
	}
	
    fmt.Println("******相当于while(true)******")
	
    for{
		fmt.Println(x)

		x = x-50

		if x == 0 {
			break
		}
	}
	
    fmt.Println("******返回索引和值******")
	
    arr:=[]int{1,2,3,4,5}

	for i,n := range arr{
		fmt.Println(i,":",n)
	}
}
```

执行结果：

![image-20210514224453702](https://gitee.com/MyHexo/blog-image/raw/master/img/image-20210514224453702.png)

#### 标签

> `for` 标签

``` go
fmt.Println("给For 打标签，应用 break | continue 跳出或者终止循环")

f1:for i:=0;i<4;i++{ //f1 标签
    for j:=0;j<4;j++{
        if i==2{
            break f1  //此处结束的循环是f1 标签所对应的i for 循环
        }
        fmt.Println(i,j)
    }
}
```

输出结果：

![image-20210515165154495](https://gitee.com/MyHexo/blog-image/raw/master/img/20210515195143.png)

#### `goto` 跳跃

* 跳跃到指定行，然后向下执行代码

``` go
package api

import "fmt"

func GotoMethod()  {
	fmt.Println("请输入姓名：")
	var name string
	fmt.Scanln(&name)

	if name == "Harris"{
		goto SVIP
	}else if name=="Dongjie"{
		goto VIP
	}

	fmt.Println("预约...")
VIP:
	fmt.Println("等号...")
SVIP:
	fmt.Println("治疗...")
}
```

![image-20210517214916383](https://gitee.com/MyHexo/blog-image/raw/master/img/20210517214916.png)![image-20210517214945026](https://gitee.com/MyHexo/blog-image/raw/master/img/20210517214945.png)![image-20210517215011356](https://gitee.com/MyHexo/blog-image/raw/master/img/20210517215011.png)

#### 字符串格式化

``` go
package api

import "fmt"

func StringFormatMenthod()  {
	var name,address,action string

	fmt.Println("请输入姓名")

	fmt.Scanln(&name)

	fmt.Println("请输入地址")

	fmt.Scanln(&address)

	fmt.Println("请输入行为")

	fmt.Scanln(&action)

	result := fmt.Sprintf("我叫%s,我正在%s旁边%s",name,address,action)

	fmt.Println(result)
}
```

输出：

![image-20210517220631093](https://gitee.com/MyHexo/blog-image/raw/master/img/20210517220631.png)

### 运算符

#### 位运算符

进制转换：

例如：101000 -> 2*5 + 2**3 -> 32 + 8 = 40

``` go
1. 按位进行与运算(全为1，才得1)

r1 := 5 & 99
5  -> 0000101
99 -> 1100011
      0000001 -> 1

2. 按位进行或运算(只要有1，就得1)

r2 := 5 | 99
5  -> 0000101
99 -> 1100011
      1100111 -> 2*6 +2*5 +2*2 + 2*1 + 2*0 -> 64+32+4+2+1 = 103

3. 按位进行异或运算(上下不同，就得1)

r3 := 5 ^ 99
5  -> 0000101
99 -> 1100011
      1100110 -> 2*6 +2*5 +2*2 + 2*1 -> 64+32+4+2 = 102

4. 按钮进行左移运算

r4 := 5 << 2
5  -> 0000101 -> 101 //前面的0可以省略
所以：101 左移之后 后面补0 -> 10100 -> 2*4+2*2 -> 16+4 = 20

5. 按位进行右移运算

r5 := 5 >> 1
5  -> 0000101 -> 101 //前面的0可以省略
所以：101 右移之后 10 右移1位，后面咬去1位， -> 10 -> 2*1 = 2
```

![image-20210517230842863](https://gitee.com/MyHexo/blog-image/raw/master/img/20210517230842.png)

补充：

* 十进制转二进制

  | 十进制 | 二进制（逢二进一） |
  | ------ | ------------------ |
  | 0      | 0                  |
  | 1      | 1                  |
  | 2      | 10                 |
  | 3      | 11                 |
  | 4      | 100                |
  | 5      | 101                |
  | 6      | 110                |
  | 7      | 111                |
  | 8      | 1000               |
  | 9      | 1001               |
  | 10     | 1010               |

## 函数

``` go
package commlib

import (
	"errors"
	"fmt"
)

// 函数可以定义多个返回值，甚至对其命名
func FunctionTest(a, b int) (int, error) {
	if b == 0 {
		return 0, errors.New("FunctionTest")
	}
	return a / b, nil
}

func FunctionTest2() {
	a, b := 1, 0 //定义多个变量

	c, err := FunctionTest(a, b) //接受多个返回值

	fmt.Println(c, err)
}

//函数是第一类型，可以作为参数或者返回值
func FunctionTest3(x int)func()  {//返回函数类型
	return func() {//匿名函数
		fmt.Println("返回参数为函数",x)//闭包
	}
}

func FunctionTest4()  {
	x:=100

	f:=FunctionTest3(x)//返回函数

	f()//执行函数
}

//通过关键字 defer 来修饰动作标识最后执行
func FunctionTest5(a,b int)  {

	defer fmt.Println("延迟执行的动作")

	fmt.Println("正在执行的动作")

	fmt.Println(a+b)
}

func FunctionTest6()  {
	x,y := 10,0

	FunctionTest5(x,y)
}
```

执行：

``` go
fmt.Println("执行：FunctionTest2")//函数可以定义多个返回值，甚至对其命名
commlib.FunctionTest2()
fmt.Println("执行：FunctionTest4")//函数是第一类型，可以作为参数或者返回值
commlib.FunctionTest4()
fmt.Println("执行：FunctionTest6")//通过关键字 defer 来修饰动作标识最后执行
commlib.FunctionTest6()
```

执行结果：

![image-20210514224544469](https://gitee.com/MyHexo/blog-image/raw/master/img/image-20210514224544469.png)

## 常见问题

1. `go.mod file not found in current directory or any parent directory; see 'go help modules'`

   问题是go环境的问题：执行`go env -w GO111MODULE=auto`
   
2. 修改配置：运行种类和包位置 可以解决运行只局限在某个文件的问题
