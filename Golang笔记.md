### Golang

#### 1 语言结构

```go
package main

import "fmt"

func main() {
   /* 这是我的第一个简单的程序 */
   fmt.Println("Hello, World!")
}
```

* package main定义了包名，必须在源文件中非注释的第一行指明这个文件属于哪个包。package main表示一个可独立执行的程序，每个Go应用程序都包含一个名为main的包
* 当标识符以一个大写字母开头，使用这种形式的标识符的对象可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）。标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的（像面向对象语言中的 protected ）
* `{`不能单独放在一行
* 执行Go程序
  * `$ go run hello.go`
  * 生成二进制文件，`$ go build hello.go`，运行，`$ ./hello`

#### 2 基础语法

* 行分隔符
  * 一行代表一个语句结束，无需以分号 ; 结尾
  * 若将多个语句写在同一行，必须使用 ; 区分
* 注释
  * 单行注释：以 // 开头
  * 多行注释：以 /* 开头，*/ 结尾
* 标识符
  * 一个或是多个字母(A\~Z和a\~z)、数字(0~9)、下划线_组成的序列
  * 第一个字符必须是字母或下划线而不能是数字
* 字符串连接
  * 字符串可以通过+实现：`fmt.Println("Google" + "Runoob")`

#### 3 数据类型

| 类型       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| 布尔型     | 只可以是常量true或false，`var b bool = true`                 |
| 数字类型   | 整型int和浮点型float32、float64，支持复数，其中位的运算采用补码 |
| 字符串类型 | 一串固定长度的字符连接起来的字符序列                         |
| 派生类型   | 指针、数组、结构体、Channel、函数、切片、接口(interface)、Map |

* 基于架构的类型

| 类型   | 描述             |
| ------ | ---------------- |
| uint8  | 无符号 8 位整型  |
| uint16 | 无符号 16 位整型 |
| uint32 | 无符号 32 位整型 |
| uint64 | 无符号 64 位整型 |
| int8   | 有符号 8 位整型  |
| int16  | 有符号 16 位整型 |
| int32  | 有符号 32 位整型 |
| int64  | 有符号 64 位整型 |

* 浮点型

| 类型       | 描述            |
| ---------- | --------------- |
| float32    | 32 位浮点型数   |
| float64    | 64 位浮点型数   |
| complex64  | 32 位实数和虚数 |
| complex128 | 64 位实数和虚数 |

* 其它数字类型

| 类型    | 描述                         |
| ------- | ---------------------------- |
| byte    | 类似 uint8                   |
| rune    | 类似 int32                   |
| uint    | 32 或 64 位                  |
| int     | 与 uint 一样大小             |
| uintptr | 无符号整型，用于存放一个指针 |

#### 4 变量

* 第一种，指定变量类型，如果没有初始化，则变量默认为零值：

  * 无初始化：`var identifier type`

  * 有初始化：`var identifier type = value`

  * 零值就是变量没有做初始化时系统默认设置的值

    * 数值类型（包括complex64/128）为 0

    * 布尔类型为 false

    * 字符串为 ""（空字符串）

    * 以下几种类型为 nil

      ```go
      var a *int
      var a []int
      var a map[string] int
      var a chan int
      var a func(string) int
      var a error // error 是接口
      ```

* 第二种，根据值自行判定变量类型：

  * `var identifier = value`

* 第三种，省略 var：

  * `v_name := value`

  * 只能在函数体中出现，不可用于全局变量的声明与赋值

  * 如果`:=`左侧没有声明新的变量，则产生编译错误

    ```go
    var intVal int 
    intVal :=1 // 产生编译错误
    intVal,intVal1 := 1,2 // 不会产生编译错误，因为有声明新的变量
    ```

* 多变量声明

  ```go
  var vname1, vname2, vname3 type
  vname1, vname2, vname3 = v1, v2, v3  //第一种
  
  var vname1, vname2, vname3 = v1, v2, v3 //第二种
  
  vname1, vname2, vname3 := v1, v2, v3 //第三种
  
  // 这种因式分解关键字的写法一般用于声明全局变量
  var (
      vname1 v_type1
      vname2 v_type2
  )
  ```

* 在相同的代码块中，不可以再次对于相同名称的变量使用初始化声明

* 声明了一个局部变量却没有在相同的代码块中使用它，会得到编译错误

* 全局变量是允许声明但不使用的

* 多变量可以在同一行进行赋值(并行赋值)

  ```go
  var a, b int
  var c string
  a, b, c = 5, 7, "abc"
  
  a, b, c := 5, 7, "abc"
  ```

* 想要交换两个变量的值，可以简单地使用`a, b = b, a`，两个变量的类型必须相同
* 空白标识符 _ 可被用于抛弃值，如值 5 在`_, b = 5, 7`中被抛弃。_ 是一个只写变量，不能得到它的值。这样做是因为Go语言中必须使用所有被声明的变量，但有时并不需要使用从一个函数得到的所有返回值
* 并行赋值也被用于当一个函数返回多个返回值

#### 5 常量

* 常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型

* 定义格式：`const identifier [type] = value`

* 可以省略type

* 多个常量声明可以简写为：`const c_name1, c_name2 = value1, value2`

* 常量可用作枚举：

  ```go
  const (
      Unknown = 0
      Female = 1
      Male = 2
  )
  ```

* 常量可以用len(), cap(), unsafe.Sizeof()函数计算表达式的值。常量表达式中，函数必须是内置函数，否则无法编译：

  ```go
  import "unsafe"
  const (
      a = "abc"
      b = len(a)
      c = unsafe.Sizeof(a)
  )
  ```

* iota

  * 特殊常量，一个可以被编译器修改的常量

  * iota在const关键字出现时将被重置为0，const中每新增一行常量声明将使iota计数一次(可理解为const语句块中的行索引)

  * iota可以被用作枚举值：

    ```go
    const (
        a = iota     //a=0
        b = iota     //b=1
        c = iota     //c=2
    )
    //可简写为
    const (
        a = iota
        b
        c
    )
    ```

  * 用法

    ```go
    func main() {
        const (
                a = iota   //0
                b          //1
                c          //2
                d = "ha"   //独立值，iota += 1
                e          //"ha"   iota += 1
                f = 100    //iota +=1
                g          //100  iota +=1
                h = iota   //7,恢复计数
                i          //8
        )
        fmt.Println(a,b,c,d,e,f,g,h,i)   //0 1 2 ha ha 100 100 7 8
    }
    ```

#### 6 运算符

* 算术运算符：+、-、*、/、%、++、--

* 关系运算符：==、!=、>、<、>=、<=

* 逻辑运算符：&&、||、!

* 位运算符：&、|、^、<<、>>

* 赋值运算符：=、+=、-=、*=、/=、%=、<<=、>>=、&=、^=、|=

* 其它运算符：

  | 运算符 | 描述             | 实例               |
  | ------ | ---------------- | ------------------ |
  | &      | 返回变量存储地址 | &a：变量的实际地址 |
  | *      | 指针变量         | *a：一个指针变量   |

#### 7 条件语句

* if语句

  ```go
  if 布尔表达式 {
     /* 在布尔表达式为 true 时执行 */
  }
  ```

* if...else语句

  ```go
  if 布尔表达式 {
     /* 在布尔表达式为 true 时执行 */
  } else {
    /* 在布尔表达式为 false 时执行 */
  }
  ```

* if嵌套语句

  ```go
  if 布尔表达式 1 {
     /* 在布尔表达式 1 为 true 时执行 */
     if 布尔表达式 2 {
        /* 在布尔表达式 2 为 true 时执行 */
     }
  }
  ```

* switch语句

  * 默认情况下case最后自带break语句，匹配成功后不会执行其它case语句

  ```go
  switch var1 {
      case val1:
          ...
      case val2:
          ...
      default:
          ...
  }
  ```

  * 变量var1可以是任何类型，而val1和val2是同类型的任意值或表达式
  * 可同时测试多个可能符合条件的值，使用逗号分割它们，例如：`case val1, val2, val3`
  * switch语句可被用于type-switch来判断某个interface变量中实际存储的变量类

  ```go
  switch x.(type){
      case type1:
         statement(s)
      case type2:
         statement(s)
      default: /* 可选 */
         statement(s)
  }
  ```

  * 使用fallthrough会强制执行后面的case语句，fallthrough不会判断下一条case的表达式结果是否为true

* select语句

  * 每个case必须是一个通信操作，要么是发送要么是接收
  * select随机执行一个可运行的case，如果没有case可运行，它将阻塞，直到有case可运行
  * 一个默认的子句应该总是可运行的

  ```go
  select {
      case communication clause  :
         statement(s)    
      case communication clause  :
         statement(s)
      default : /* 可选 */
         statement(s)
  }
  ```

  * 每个case都必须是一个通信

  * 所有channel表达式都会被求值

  * 所有被发送的表达式都会被求值

  * 如果任意某个通信可以进行，它就执行，其他被忽略

  * 如果有多个case都可以运行，select会随机公平地选出一个执行，其他不会执行，否则：

    1. 如果有default子句，则执行该语句
    2. 如果没有default子句，select将阻塞，直到某个通信可以运行。Go不会重新对channel或值进行求值

  * 实例

    ```go
    package main
    
    import "fmt"
    
    func main() {
       var c1, c2, c3 chan int
       var i1, i2 int
       select {
          case i1 = <-c1:
             fmt.Printf("received ", i1, " from c1\n")
          case c2 <- i2:
             fmt.Printf("sent ", i2, " to c2\n")
          case i3, ok := (<-c3):  // same as: i3, ok := <-c3
             if ok {
                fmt.Printf("received ", i3, " from c3\n")
             } else {
                fmt.Printf("c3 is closed\n")
             }
          default:
             fmt.Printf("no communication\n")
       }    
    }
    ```

#### 8 循环语句

* for循环

  * 形式1：`for init; condition; post { }`

    * init：一般为赋值表达式，给控制变量赋初值

    * condition：关系表达式或逻辑表达式，循环控制条件

    * post：一般为赋值表达式，给控制变量增量或减量

      ```go
      for i := 0; i <= 10; i++ {
          sum += i
      }
      ```

  * 形式2：`for condition { }`，类似于C中的while语句

  * 形式3：`for { }`，无限循环

  * for循环的range格式可以对slice、map、数组、字符串等进行迭代循环：

    ```go
    for key, value := range oldMap {
        newMap[key] = value
    }
    ```

* 循环嵌套

  ```go
  for [condition |  ( init; condition; increment ) | Range]
  {
     for [condition |  ( init; condition; increment ) | Range]
     {
        statement(s);
     }
     statement(s);
  }
  ```

* break语句

  * 用于循环语句中跳出循环，并开始执行循环之后的语句

  * break在switch中起到在执行一条case后跳出语句的作用

  * 在多重循环中，可以用标号label标出想break的循环

    ```go
    func main() {
        // 不使用标记
        for i := 1; i <= 3; i++ {
            fmt.Printf("i: %d\n", i)
                    for i2 := 11; i2 <= 13; i2++ {
                            fmt.Printf("i2: %d\n", i2)
                            break
                    }
            }
        // 使用标记
        re:
            for i := 1; i <= 3; i++ {
                fmt.Printf("i: %d\n", i)
                for i2 := 11; i2 <= 13; i2++ {
                    fmt.Printf("i2: %d\n", i2)
                    break re
                }
            }
    }
    ```

* continue语句

  * 在多重循环中，可以用标号label标出想continue的循环

* goto语句

  *  goto语句可以无条件地转移到过程中指定的行
  * 在结构化程序设计中不主张使用goto语句

  ```go
  goto label
  ..
  ..
  label: statement
  ```

#### 9 函数

* 函数定义

  ```go
  func function_name( [parameter list] ) [return_types] {
     函数体
  }
  ```

  * 函数可以返回多个值

  * 实例

    ```go
    func swap(x, y string) (string, string) {
       return y, x
    }
    func main() {
       a, b := swap("Google", "Runoob")
       fmt.Println(a, b)      //Runoob Google
    }
    ```

* 值传递

  * 调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，不会影响到实际参

* 引用传递

  * 调用函数时将实际参数的地址传递到函数中，那么在函数中对参数所进行的修改，将影响到实际参数

  * 实例

    ```Go
    func swap(x *int, y *int) {
       var temp int
       temp = *x    /* 保持 x 地址上的值 */
       *x = *y      /* 将 y 值赋给 x */
       *y = temp    /* 将 temp 值赋给 y */
    }
    
    调用：swap(&a, &b)
    ```

* 函数作为实参

  * 实例

    ```go
    func main(){
       /* 声明函数变量 */
       getSquareRoot := func(x float64) float64 {
          return math.Sqrt(x)
       }
    
       /* 使用函数 */
       fmt.Println(getSquareRoot(9))
    }
    ```

* 函数闭包

  * 支持匿名函数，可作为闭包。匿名函数是一个"内联"语句或表达式

  * 优越性在于可以直接使用函数内的变量，不必声明

  * 实例，创建函数getSequence() ，返回另外一个函数。该函数的目的是在闭包中递增 i 变量：

    ```go
    package main
    import "fmt"
    
    func getSequence() func() int {
       i:=0
       return func() int {
          i+=1
         return i  
       }
    }
    
    func main(){
       /* nextNumber 为一个函数，函数 i 为 0 */
       nextNumber := getSequence()  
    
       /* 调用 nextNumber 函数，i 变量自增 1 并返回 */
       fmt.Println(nextNumber())      //1
       fmt.Println(nextNumber())      //2
       fmt.Println(nextNumber())      //3
       
       /* 创建新的函数 nextNumber1，并查看结果 */
       nextNumber1 := getSequence()  
       fmt.Println(nextNumber1())     //1
       fmt.Println(nextNumber1())     //2
    }
    ```

* 函数方法

  * 方法就是一个包含了接受者的函数，接受者可以是命名类型或者结构体类型的一个值或者是一个指针
  * 所有给定类型的方法属于该类型的方法集

  ```go
  func (variable_name variable_data_type) function_name() [return_type]{
     /* 函数体*/
  }
  ```

  * 实例

    ```go
    type Circle struct {
      radius float64
    }
    
    func main() {
      var c1 Circle
      c1.radius = 10.00
      fmt.Println("圆的面积 = ", c1.getArea())
    }
    
    //该 method 属于 Circle 类型对象中的方法
    func (c Circle) getArea() float64 {
      return 3.14 * c.radius * c.radius
    }
    ```

* 递归函数

  * 需要设置退出条件

  ```go
  func recursion() {
     recursion() /* 函数调用自身 */
  }
  ```

  * 实例，计算阶乘：

    ```go
    func Factorial(n uint64)(result uint64) {
        if (n > 0) {
            result = n * Factorial(n-1)
            return result
        }
        return 1
    }
    
    func main() {  
        var i int = 15
        fmt.Printf("%d 的阶乘是 %d\n", i, Factorial(uint64(i)))
    }
    ```

#### 10 变量作用域

* 局部变量

  * 在函数体内声明的变量
  * 作用域只在函数体内，参数和返回值变量也是局部变量

* 全局变量

  * 在函数体外声明的变量
  * 可以在整个包或外部包（被导出后）中使用
  * 全局变量与局部变量名称可以相同，但是函数内的局部变量会被优先考虑

* 形式参数

  * 作为函数的局部变量使用

* 不同类型的局部和全局变量默认值

  | 数据类型 | 初始化默认值 |
  | -------- | ------------ |
  | int      | 0            |
  | float32  | 0            |
  | pointer  | nil          |

#### 11 数组

* 具有相同唯一类型的一组已编号且长度固定的数据项序列

* 数组声明：`var variable_name [SIZE] variable_type`

* 初始化：

  * 初始化数组中`{}`中的元素个数不能大于`[]`中的数字

  ```go
  var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
  
  var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}  //编译器自行推断数组长度
  
  balance := [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
  
  balance := [5]float32{1:2.0,3:7.0}  //如果设置了数组的长度，可通过指定下标来初始化元素
  ```

* 多维数组

  * `var variable_name [SIZE1][SIZE2]...[SIZEN] variable_type`

  * 实例

    ```go
    func main() {
        values := [][]int{}   //创建数组
    
        //使用 appped() 函数向空的二维数组添加两行一维数组
        row1 := []int{1, 2, 3}
        row2 := []int{4, 5, 6}
        values = append(values, row1)
        values = append(values, row2)
    }
    ```

  * 多维数组可通过大括号来初始值

    ```go
    a := [3][4]int{  
     {0, 1, 2, 3} ,   /*  第一行索引为 0 */
     {4, 5, 6, 7} ,   /*  第二行索引为 1 */
     {8, 9, 10, 11},   /* 第三行索引为 2 */
    }
    ```

  * 可创建各个维度元素数量不一致的多维数组

    ```go
    func main() {
        // 创建空的二维数组
        animals := [][]string{}
    
        // 创建三个一维数组，各数组长度不同
        row1 := []string{"fish", "shark", "eel"}
        row2 := []string{"bird"}
        row3 := []string{"lizard", "salamander"}
    
        // 使用 append() 函数将一维数组添加到二维数组中
        animals = append(animals, row1)
        animals = append(animals, row2)
        animals = append(animals, row3)
    
        // 循环输出
        for i := range animals {
            fmt.Printf("Row: %v\n", i)
            fmt.Println(animals[i])
        }
    }
    ```

* 向函数传递数组
  * 第一种，形参设定数组大小：`void myFunction(param [10]int){}`
  * 第二种，形参未设定数组大小：`void myFunction(param []int){}`

#### 12 指针

* 取地址符是&，放到一个变量前使用就会返回相应变量的内存地址

* 一个指针变量指向了一个值的内存地址，指针声明格式：`var var_name *var-type`

* 在指针类型前面加上*号来获取指针所指向的内容

* 当一个指针被定义后没有分配到任何变量时，它的值为nil，即为空指针

* 空指针判断：

  ```go
  if(ptr != nil)     /* ptr 不是空指针 */
  if(ptr == nil)    /* ptr 是空指针 */
  ```

* 指针数组：`var ptr [MAX]*int`
* 指向指针的指针变量：`var ptr **int`
* 指针作为函数参数：引用传递

#### 13 结构体

* 一系列具有相同类型或不同类型的数据构成的数据集合

* 结构体的格式：

  ```go
  type struct_variable_type struct {
     member definition
     member definition
     ...
     member definition
  }
  ```

* 一旦定义了结构体类型，就能用于变量的声明，语法格式：

  ```go
  variable_name := structure_variable_type {value1, value2...valuen}
  或
  variable_name := structure_variable_type { key1: value1, key2: value2..., keyn: valuen}
  // 忽略的字段为 0 或 空
  ```

* 如果要访问结构体成员，需要使用点号`.`操作符，格式为：`结构体.成员名`

  * 实例

    ```go
    type Books struct {
       title string
       subject string
    }
    func main() {
       var Book1 Books        /* 声明 Book1 为 Books 类型 */
    
       Book1.title = "Go 语言"
       Book1.subject = "计算机"
    
       fmt.Printf( "Book 1 title : %s\n", Book1.title)
       fmt.Printf( "Book 1 subject : %s\n", Book1.subject)
    ```

* 可将结构体类型作为参数传递给函数
* 可定义指向结构体的指针
  
  * 使用结构体指针访问结构体成员，使用 "." 操作符：`struct_pointer.title`

#### 14 切片Slice

* 切片(动态数组)是对数组的抽象

* 与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大

* 切片声明

  ```go
  var identifier []type  //声明一个未指定大小的数组来定义切片，切片不需要说明长度
  
  var slice1 []type = make([]type, len)  //使用make()函数来创建切片
  
  slice1 := make([]type, len)  //简写，len是数组的长度并且也是切片的初始长度
  
  slice1 := make([]type, len, capacity)  //可以指定容量，capacity为可选参数
  ```

* 切片初始化

  ```go
  s :=[] int {1,2,3}  //直接初始化切片，[]表示是切片类型，初始化值依次是1、2、3，其cap=len=3
  
  s := arr[:]  //初始化切片s，是数组arr的引用
  
  s := arr[startIndex:endIndex]  //将arr中startIndex到endIndex-1的元素创建为一个新的切片
  
  s := arr[startIndex:]  //默认endIndex时将表示一直到arr的最后一个元素
  
  s := arr[:endIndex]  //默认startIndex时将表示从arr的第一个元素开始
  
  s1 := s[startIndex:endIndex]  //通过切片s初始化切片s1
  ```

* len(s)：获取长度

* cap(s)：测量切片最长可以达到多少

* 一个切片在未初始化之前默认为nil，长度为0

* append()和copy()函数

  * 创建一个新的更大的切片并把原分片的内容都拷贝过来，从而增加切片的容量

  * 实例

    ```go
    func main() {
       var numbers []int
       printSlice(numbers)  //len=0 cap=0 slice=[]
    
       /* 允许追加空切片 */
       numbers = append(numbers, 0)
       printSlice(numbers)  //len=1 cap=1 slice=[0]
    
       /* 向切片添加一个元素 */
       numbers = append(numbers, 1)
       printSlice(numbers)  //len=2 cap=2 slice=[0 1]
    
       /* 同时添加多个元素 */
       numbers = append(numbers, 2,3,4)
       printSlice(numbers)  //len=5 cap=6 slice=[0 1 2 3 4]
    
       /* 创建切片 numbers1 是之前切片的两倍容量*/
       numbers1 := make([]int, len(numbers), (cap(numbers))*2)
    
       /* 拷贝 numbers 的内容到 numbers1 */
       copy(numbers1,numbers)
       printSlice(numbers1)  //len=5 cap=12 slice=[0 1 2 3 4]
    }
    
    func printSlice(x []int){
       fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
    }
    ```

#### 15 范围Range

* range用于for循环中迭代数组、切片、通道、集合的元素

* 在数组和切片中它返回元素的索引和索引对应的值，在集合中返回key-value对

* 实例

  ```go
  func main() {
      //使用range求一个slice的和
      nums := []int{2, 3, 4}
      sum := 0
      for _, num := range nums {
          sum += num
      }
      fmt.Println("sum:", sum)
  
      //range可以用在map的键值对上
      kvs := map[string]string{"a": "apple", "b": "banana"}
      for k, v := range kvs {
          fmt.Printf("%s -> %s\n", k, v)
      }
  
      //range可以用来枚举Unicode字符串
      for i, c := range "go" {
          fmt.Println(i, c)
      }
  }
  ```

#### 16 集合Map

* 无序的键值对的集合，可以像迭代数组和切片那样迭代它

* 通过key来快速检索数据，key类似于索引，指向数据的值

* Map使用hash表来实现，因此无序，即无法决定它的返回顺序

* Map定义：

  ```go
  /* 声明变量，默认 map 是 nil */
  var map_variable map[key_data_type]value_data_type
  
  /* 使用 make 函数 */
  map_variable := make(map[key_data_type]value_data_type)
  ```

* 如果不初始化map，就会创建一个nil map，nil map不能用来存放键值对

* 实例

  ```go
  func main() {
      var countryCapitalMap map[string]string /* 创建集合 */
      countryCapitalMap = make(map[string]string)
  
      /* map插入key - value对，各个国家对应的首都 */
      countryCapitalMap [ "France" ] = "巴黎"
      countryCapitalMap [ "Italy" ] = "罗马"
  
      /* 使用键输出地图值 */
      for country := range countryCapitalMap {
          fmt.Println(country, "首都是", countryCapitalMap [country])
      }
  
      /* 查看元素在集合中是否存在 */
      capital, ok := countryCapitalMap [ "American" ] /* 若确定是真实的则存在，否则不存在 */
      if (ok) {
          fmt.Println("American 的首都是", capital)
      } else {
          fmt.Println("American 的首都不存在")
      }
  }
  ```

* delete函数

  * 用于删除集合的元素，参数为map和其对应的key

  * 实例

    ```go
    func main() {
    		/* 创建map */
    		countryCapitalMap := map[string]string{"France": "Paris", "Italy": "Rome"}
    
    		/*删除元素*/ 
    		delete(countryCapitalMap, "France")
    }
    ```

#### 17 类型转换

* 将一种数据类型的变量转换为另外一种类型的变量：`type_name(expression)`

#### 18 接口

* 把所有的具有共性的方法定义在一起，任何其他类型只要实现了这些方法就是实现了这个接口：

  ```go
  /* 定义接口 */
  type interface_name interface {
     method_name1 [return_type]
     method_name2 [return_type]
     method_name3 [return_type]
     ...
     method_namen [return_type]
  }
  
  /* 定义结构体 */
  type struct_name struct {
     /* variables */
  }
  
  /* 实现接口方法 */
  func (struct_name_variable struct_name) method_name1() [return_type] {
     /* 方法实现 */
  }
  ...
  func (struct_name_variable struct_name) method_namen() [return_type] {
     /* 方法实现*/
  }
  ```

* 实例

  ```go
  type Phone interface {
      call()
  }
  
  type NokiaPhone struct {
  }
  
  func (nokiaPhone NokiaPhone) call() {
      fmt.Println("I am Nokia, I can call you!")
  }
  
  type IPhone struct {
  }
  
  func (iPhone IPhone) call() {
      fmt.Println("I am iPhone, I can call you!")
  }
  
  func main() {
      var phone Phone
  
      phone = new(NokiaPhone)
      phone.call()  //I am Nokia, I can call you!
  
      phone = new(IPhone)
      phone.call()  //I am iPhone, I can call you!
  }
  ```

#### 19 错误处理

* Golang通过内置的错误接口提供了非常简单的错误处理机制

* error类型是一个接口类型，定义：

  ```go
  type error interface {
      Error() string
  }
  ```

* 函数通常在最后的返回值中使用errors.New返回一个错误信息：

  ```go
  func Sqrt(f float64) (float64, error) {
      if f < 0 {
          return 0, errors.New("math: square root of negative number")
      }
      // 实现
  }
  ```

* 在调用Sqrt的时候传递一个负数，得到了non-nil的error对象，将此对象与nil比较，结果为true，所以fmt.Println(fmt包在处理error时会调用Error方法)被调用，以输出错误：

  ```go
  result, err:= Sqrt(-1)
  
  if err != nil {
     fmt.Println(err)
  }
  ```

* 实例

  ```go
  // 定义一个 DivideError 结构
  type DivideError struct {
      dividee int
      divider int
  }
  
  // 实现error接口
  func (de *DivideError) Error() string {
      strFormat := `
      Cannot proceed, the divider is zero.
      dividee: %d
      divider: 0
  `
      return fmt.Sprintf(strFormat, de.dividee)
  }
  
  // 定义int类型除法运算的函数
  func Divide(varDividee int, varDivider int) (result int, errorMsg string) {
      if varDivider == 0 {
              dData := DivideError{
                      dividee: varDividee,
                      divider: varDivider,
              }
              errorMsg = dData.Error()
              return
      } else {
              return varDividee / varDivider, ""
      }
  }
  
  func main() {
      // 正常情况
      if result, errorMsg := Divide(100, 10); errorMsg == "" {
              fmt.Println("100/10 = ", result)
      }
      // 当除数为零的时候会返回错误信息
      if _, errorMsg := Divide(100, 0); errorMsg != "" {
              fmt.Println("errorMsg is: ", errorMsg)
      }
  }
  ```

#### 20 并发

* Golang支持并发，只需要通过关键字go来开启goroutine

* goroutine是轻量级线程，goroutine的调度由Golang运行时进行管理

* goroutine语法格式：`go 函数名( 参数列表 )`

* 使用go语句开启一个新的运行期线程， 即goroutine，以一个不同的、新创建的goroutine来执行一个函数。 同一个程序中的所有goroutine共享同一个地址空间

* 实例

  ```go
  func say(s string) {
          for i := 0; i < 5; i++ {
                  time.Sleep(100 * time.Millisecond)
                  fmt.Println(s)
          }
  }
  
  func main() {
          go say("world")
          say("hello")
  }
  //输出的hello和world没有固定的先后顺序，因为有两个goroutine在执行
  ```

* 通道channel

  * 用来传递数据的一个数据结构

  * 通道可用于两个goroutine之间通过传递一个指定类型的值来同步运行和通讯。操作符 `<-` 用于指定通道的方向，发送或接收。如果未指定方向，则为双向通道

  * 通道声明：

    ```go
    ch := make(chan int)
    
    ch <- v    //把 v 发送到通道 ch
    v := <-ch  //从 ch 接收数据，并把值赋给 v
    
    ```

  * 默认情况下，通道是不带缓冲区的。发送端发送数据，同时必须有接收端相应的接收数据

  * 实例，通过两个goroutine来计算数字之和，在goroutine完成计算后，它会计算两个结果的和：

    ```go
    func sum(s []int, c chan int) {
            sum := 0
            for _, v := range s {
                    sum += v
            }
            c <- sum // 把 sum 发送到通道 c
    }
    
    func main() {
            s := []int{7, 2, 8, -9, 4, 0}
    
            c := make(chan int)
            go sum(s[:len(s)/2], c)
            go sum(s[len(s)/2:], c)
            x, y := <-c, <-c // 从通道 c 中接收
    
            fmt.Println(x, y, x+y)  //-5 17 12
    }
    ```

* 通道缓冲区

  * 通道可以设置缓冲区，通过make的第二个参数指定缓冲区大小：`ch := make(chan int, 100)`

  * 带缓冲区的通道允许发送端的数据发送和接收端的数据获取处于异步状态，即发送端发送的数据可以放在缓冲区里面，等待接收端去获取数据，而不需要接收端立刻去获取数据

  * 由于缓冲区的大小是有限的，所以必须有接收端来接收数据，否则缓冲区一满，数据发送端就无法再发送数据

  * 如果通道不带缓冲，发送方会阻塞直到接收方从通道中接收了值。如果通道带缓冲，发送方则会阻塞直到发送的值被拷贝到缓冲区内；如果缓冲区已满，则意味着需等待直到某个接收方获取到一个值。接收方在有值可以接收之前会一直阻塞

  * 实例

    ```go
    func main() {
        // 定义一个可以存储整数类型的带缓冲通道，缓冲区大小为2
    		ch := make(chan int, 2)
    
    		// 因为ch是带缓冲的通道，可以同时发送两个数据，不需要立刻去同步读取数据
    		ch <- 1
    		ch <- 2
    
    		// 获取这两个数据
    		fmt.Println(<-ch)  // 1
    		fmt.Println(<-ch)  // 2
    }
    ```

* 遍历通道与关闭通道

  * 通过range关键字来实现遍历读取到的数据，格式：`v, ok := <-ch`

  * 如果通道接收不到数据后ok就为false，这时通道就可以使用close()函数来关闭

  * 实例

    ```go
    func fibonacci(n int, c chan int) {
    		x, y := 0, 1
    		for i := 0; i < n; i++ {
        				c <- x
    						x, y = y, x+y
    		}
    		close(c)
    }
    
    func main() {
    		c := make(chan int, 10)
    		go fibonacci(cap(c), c)
    		// range函数遍历每个从通道接收到的数据，因为c在发送完10个数据之后就关闭了通道，所以range 函数在接收到10个数据之后就结束了。如果上面的c通道不关闭，那么range函数就不会结束，从而在接收第11个数据的时候发生阻塞
    		for i := range c {
    						fmt.Println(i)  // 0 1 1 2 3 5 8 13 21 34
    		}
    }
    ```

