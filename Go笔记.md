Go算法笔记

#### 知识点：

1. 

#### 一、简单数据类型；

1. **数据类型**

   - 简单数据类型：int、float、complex、bool、string
   - 数据结构：struct、array、slice、map、channel
   - 接口：interface

2. **Number类型**：最常用的是int

   - int8(-128~127), int16, int32, int64    //数字指的是bit
   - uint8(0~255), uint16, uint32, uint64
   - byte 相当于uint8
   - rune相当于int32
   - int 由CPU位数决定，表示一个机器字长，64bit或者32bit
   - uint 无符号的int，同样由CPU位数决定

3. **byte类型**：

   - 用单引号  ‘ ’ 包围的单个字符；
   - go中没有字符类型，所有的字符都用整数保存，ASCII字符最大值是127，常见字符（英文字母等）用byte就可以，中文的话，采用Unicode编码，一个字符占3个字节，所以需要用rune或者int;
   - 对字符直接输出就是实际的ASCII码或者Unicode码，可通过string(a)转为“字符”，但是数据类型还是string。

4. **folat和complex**:

   - float32：7位小数
   - float64：15位小数    //常用它，第一精确，第二包中的方法基本都是这个方法；比较float类型相等不相等时，不要用==或者！=，要用差值小于一个足够小的值
   - complex64  complex128   //复数	

5. **string类型**：

   - 保存UTF-8的字符序列，动态大小，字母占1个字节，Unicode字符占2-4个字节，中文占3个字节；

   - 用双引号 "" 反引号``包围，双引号是弱引用，可以用反斜线 \ 进行转义；反引号是强引用，就是字面意思，无法转义；

   - **string底层是byte数组**，其实string占用两个机器字长，一个指针，一个长度，但是这个指针不可见，所以string底层是byte数组的值类型，而不是指针类型。string和byte[]切片可以相互转换

     - 可以将string使用append()或者copy()添加或者复制给一个[] byte slice，也可以使用slice的切片功能截取string中的片段；

       ```go
       
       func main(){
           var a = "Hello Gaoxiaofang"
           println(a[2:3])     //输出:1
       
           s1 :=make ([]byte, 30)
         	copy (sl,a)        //将字符电保存到slice中   
           println(string(s1)) //输出"Hello Gaoxiaofang"
         	
           str:=string(s1)//可以将byte[]转为string
         	println(str)   ////输出"Hello Gaoxiaofang"
         
       }
       ```

   - **string的截取**（索引按照字节算，不是字符，从0开始）：

     - “string”[x]: 返回第x个字节对应字符的二进制**数值**；字母ASCII码。其他的Unicode码；

     - "string[x:y]":返回第x个字节到y字节中间的**字符串**；左闭右开

       ```go
       func main(){
           println ("abcde"[1])    	//(1).输出"98"
           print1n ("我是中国人"[1])    //(2).输出"136"
           printIn ("abcde" [0:2])    //(3).输出"ab"
           printin("我是中国人"[0:31)   //(4)．输出“我
           print1n ("abcde" [3:4])    //(5).输出"d"         
       ```
     
   - **string拼接**：

     - 用 + 连接两个string类型，string(97)函数只能是字符和他的ASCII或Unicode值的转换，如果将97转为字符串“97”而不是‘a’，需要用strconv包中的函数；
     - 用strings包中的Join函数进行连接；strings.Join(elems []string, sep string) string

   - **string长度**：len取的是字节数量，不是字符数量。

   - **string遍历**：

     - 数组形式遍历：

       - 如果字符串全是ASCII字符，直接遍历即可；因为一个字符一个字节；

       - 如果包含了多字节字符，先用[]rune(str)转换再遍历；

         ```go
         str:="Hello 你好"
         	r:=[]rune(str)
         	for i:=0;i<len(r);i++ {
         		fmt.Printf("%d,%c",i,r[i])
         	}
         //0H1e2l3l4o5 6你7好
         ```

     - range遍历：

       - 返回index和value，index是按照字节来返回的，value是按照字符一个一个返回的。

         ```go
         str:="Hello 你好"	
         for i,v:=range str{
         		fmt.Print(i,string(v))
         	}
         //0H1e2l3l4o5 6你9好
         ```

   - **string的比较**：

     - 可以使用 < <= > >= == !=进行比较，字母是以A-Z a-zASCII码进行排列比较。

   - **string的修改**：字符串是不可变的对象，不能直接修改字符串，只能将字符串拷贝到[]byte slice中，之后按照索引修改相应位置的字符，然后再将byte slice转为string。

     ```go
     s:= "gaoxiaofang" 
     bs := []byte(s)
     bs [0] ='ml'   //必须使用单引号
     s=string(bs)
     println(s)
     ```

   - 

6. **数据类型及大小**

   - 数据类型：reflect.TypeOf( )
   - 数据所含空间大小（字节）：unsafe.Sizeof( )

7. **Array**：数组

   ```go
   //声明
   var arr_name [5]int  //类型是[5]int ,默认赋值为“零”.
   //声明并初始化
   arr_name := [5]int{3,5,22,12,23}
   // 声明长度为3的数组
   arr_name1 := [...]int{2,3,4} //...表示根据值来推断数组长度。
   ```

   

   - 

8. 

#### 二、strconv包；

1. **简单的转换**：

   - go不会进行隐式转换，只能手动转换；
   - 底层结构相同的两个类型直接可以直接通过类型进行转换： valueOfTypeB=typeB(valueOfTypeA)；byte int,int float等, string 和[]byte
   - 低精度向高精度转换安全，高精度向低精度转换不安全

2. **string和简单数据类型之间的转换**：

   - 所有函数都在strconv包中；
   - string转为其他的形式可能转换不了，所以string转为其他形式的函数一般都返回多个结果；

   ```go
   - **string与int的转换**：
     - int转为string： func Itoa(i int) string；
     - string转为int： func Atoi(s string) (int, error)
   --------------------------------------------------------------------------------------
   - **string转为换其他类型(Parse函数)**：
   	- string转为bool： func ParseBool(str string) (bool, error)
   	- string转为float64: func ParseFloat(s string, bitSize int) (float64, error) //bitSize只能是64位
   	- string转为int族类： func ParseInt(s string, base int, bitSize int) (i int64, err error) //base 表示转为什么进制，0、2-36，当base=0的时候，表示根据string的前缀来判断以什么进制去解析；bitSize表示转为多少位的int, 0、8、16、32、64,当bitSize=0的时候，表示转换为int
   	- string转为uint族类： func ParseUint(s string, base int, bitSize int) (uint64, error)
   --------------------------------------------------------------------------------------
   - **其他类型转为string(Format函数)**：
   	- bool转为string: func FormatBool(b bool) string
   	- float转为string： func FormatFloat(f float64, fmt byte, prec, bitSize int) string  //fmt表示格式，prec控制精度，bitSize位数。
   	- int转为string： func FormatInt(i int64, base int) string //base是转为什么进制的数，然后将这个数变为string类型
   	- unit转为string： func FormatUint(i uint64, base int) string //同上
   --------------------------------------------------------------------------------------
   - **其他类型转为string并加到slice中**：
   	-func AppendBool(dst []byte, b bool) []byte
   	-func AppendFloat(dst []byte, f float64, fmt byte, prec, bitSize int) []byte
   	-func AppendInt(dst []byte, i int64, base int) []byte
   	-func AppendUint(dst []byte, i uint64, base int) []byte
   ```

   - 

3. 

#### 三、**流程控制结构**；

1. 条件判断结构：if else；

2. 分支选择结构：switch case；

3. 循环结构：for；    //无while；

   - 配合range 可以遍历，array、slice、map、channel以及字符串；
   - range遍历取出的值是从被遍历结构**拷贝**值，直接修改值无效，但可以通过索引来修改值。
   - range遍历array、slice、channe索引都是直接从0,1,2这种递增的；遍历map索引就是key；遍历字符串取出的索引是按照**字节**的大小对每个元素进行索引，而不是字符；

4. break：退出for或switch结构(以及select)；

5. continue：进入下一次for迭代，只能用于for循环中；

   注意，Go对语法要求很严格。左大括号`{`必须和if、else或else if在同一行，右大括号`}`必须换行，如果有else或else if，则必须紧跟这两个关键字。

9. 

#### 四、函数；

1. **种类**：普通函数、匿名函数、方法（定义在struct上）；

2. **形参**：都是值传递，形参可以没名字，只不过slice、map、channel、interface本身就是指针，但是也是值传递；

3. **变长参数（variadic）**：可以在函数定义语句的参数部分使用`ARGS...TYPE`的方式。这时会将`...`代表的参数全部保存到一个名为ARGS的slice中，注意这些参数的数据类型都是TYPE。

   - **同种类型的参数**：相当于一个slice

     ```go
     func main{
         //变长参数第1种调用方式
         f1("lll","kkk","mmm")
         
         //变长参数第2种调用方式
         s:=make([]byte,30)
         f1(s...)
         
         //slice参数调用方式
         s:=make([]byte,30)
         f2(s)
     }
     
     // 声明f1()，变长参数
     func f1(s...string){}
     // 声明f2()，是slice形式
     func f2(s []string){}
     ```

   - **不同类型的参数**：

     - 第一种方式使用struct：

       ```go
       type args struct {
           arg1 string
           arg2 int
           arg3 type3
       }
       func main{
           //调用
           f(a,b,args{})
           f(a,b,args{arg1:"hello",arg2:22})
       }
       //声明函数f
       f(a,b int,args{})
       ```

     - 第二种方式使用interfac：

4. **返回值**：0个或多个；多个时需要用括号包围，逗号分隔；

   - 返回值和return的关系：

   ```go
   // 单个返回值：return关键字中指定了参数时，返回值可以不用名称。如果return省略参数，则返回值部分必须带名称；
   func func_a() int{
       return a
   }
   
   // 只要命名了返回值名称，必须括号包围；此外，命名的返回值a已经存在了，函数内部可以直接使用，不用再声明；
   func func_b() (a int){
       // 变量a int已存在，无需再次声明
       a = 10
       return
       // 等价于：return a
   }
   
   // 多个返回值，且在return中指定返回的内容
   func func_c() (int,int){
       return a,b
   }
   
   // 多个返回值
   func func_d() (a,b int){
       return
       // 等价于：return a,b
   }
   
   // return覆盖命名返回值
   func func_e() (a,b int){
       return x,y
   }
   ```

   - 返回过多时，返回同样类型的返回值可以放到slice中，不同类型的可以放到map中；
   - 不需要的返回值可以用下划线`_`接收并丢弃；
   - 

5. 函数也可以作为一种type类型，然后赋值给一个变量；

6. go作用域是词法作用域，函数位置决定了他能看见的变量；

7. **不允许重载**（voerload）函数，也就是不能有重名函数；

8. 函数中不允许嵌套函数，但是可以嵌套匿名函数；

9. 函数**不支持泛型**，但可以通过接口，type switch,reflection的方式来进行解决；

10. go函数是高阶函数，实现了一阶函数，所以：

    - 函数是一个值，可以将函数赋值给变量，使得这个变量也成为函数；
    - 函数可以作为参数传递给另外一个函数；
    - 函数的返回值可以是个函数；
    - 回调函数、闭包等功能都是依赖这些特性；闭包是指在函数内定义的匿名函数可以使用该函数的参数。

11. **内置函数**：builtin包中存在一些内置函数；

    ```go
    func len(v Type) int
    /*  - slice的长度
    	- map的元素个数
    	- array的元素个数
    	- 指向array的指针时，获取array的长度
    	- string的字节数
    	- channel的channel buffer中的未读队列长度
    */
    func cap(v Type) int
    //用于获取array、slice、channel的容
    func copy(dst, src []Type) int
    //用于拷贝slice
    func append(slice []Type, elems ...Type) []Type
    //用于追加slice
    -----------------------------------------------------------
    func delete(m map[Typek]Typev, key Type)
    //删除map中的元素
    -----------------------------------------------------------
    func close(c chan<- Type)
    //关闭通道
    -----------------------------------------------------------
    func print(args ...Type)
    func println(args ...Type)
    //底层的输出函数，用来调试用。在实际程序中，应该使用fmt中的print类函数
    -----------------------------------------------------------
    func complex(r, i FloatType) ComplexType
    func imag(c ComplexType) FloatType
    func real(c ComplexType) FloatType
    //complex、imag、real：操作复数(虚数)
    -----------------------------------------------------------
    func panic(v interface{})
    func recover() interface{}
    //panic和recover：处理错误
    -----------------------------------------------------------
    func new(Type) *Type
    //new适用于为值类(value type)的数据类型(如array,int等)和struct类型的对象分配内存并初始化，并返回它们的指针给变量。如v := new(int)
    func make(t Type, size ...IntegerType) Type
    //make适用于为内置的引用类的类型(如slice、map、channel等)分配内存并初始化底层数据结构，并返回它们的指针给变量，同时可能会做一些额外的操作
    ```

12. **匿名函数**：没有名称的函数。匿名函数嵌套在函数内部，或者赋值给一个变量，或者作为一个表达式。

    ```go
    // 声明匿名函数，需要在函数内声明
    func(args){
        ...CODE...
    }
    
    // 声明匿名函数并直接执行，有()
    func(args){
        ...CODE...
    }(parameters)
    //实例
    func main() {
        msg := "Hello World"
        func(m string) {//m表示匿名函数的参数
            fmt.Println(m)
        }(msg)//msg表示传递msg变量给匿名函数，并执行
    }
    
    //先定义了匿名函数，将其赋值给了一个变量，然后在需要的地方再去调用执行它。
    func main() {
        // 匿名函数赋值给变量
        a := func() {
            fmt.Println("hello world")
        }
        // 调用匿名函数
        a()
        fmt.Printf("%T\n", a) // a的type类型：func()
        fmt.Println(a)        // 函数的地址
    }
    
    ```

13. **func type**：可以将func作为一种type，以后可以直接使用这个type来定义函数。

    ```go
    package main
    import "fmt"
    
    type add func(a,b int) int
    func main() {
        var a add = func(a,b int) int{
            return a+b
        }
        s := a(3,5)
        fmt.Println(s)
    }
    ```

15. 

#### 五、defer关键字；

1. defer可以让函数或者语句延迟到函数的最结尾处进行执行，即便函数已经报错panic()或者return返回了，都在函数退出最后一刻执行；

   ```go
   func a() TYPE{
       ...CODE...
       defer b()
       ...CODE...
       // 函数执行出了错误
       return args
       // 函数b()都会在这里执行
   }
   ```

2. 每个defer函数都相当于创建了一个独立的defer栈帧，所以defer是LIFO的，逆序执行；

   ```go
   func main() {
   	println("start...")
   	defer println("1")
   	defer println("2")
   	defer println("3")
   	defer println("4")
   	println("end...")
   }
   // 输出：
   start...
   end...
   4
   3
   2
   1
   ```

3. 并且，go的作用域采用词法作用域，defer的位置决定了它能看到的变量值，而不是调用位置所看到的值；

   ```go
   package main
   var x = 10
   func main() {
       a()
   }
   
   func a() {
   	println("start a:",x)   // 输出10
   	x = 20
   	defer b(x)// 压栈，并按值拷贝20到栈中
   	x = 30
       println("leaving a:",x)  // 输出30
       // 调用defer延迟的对象b()，输出20
   }
   
   func b(x int) {
       println("start b:",x)
   }
   ```

   ```go
   package main
   var x = 10
   func main() {
   	a()
   }
   
   func a() int {
   	println("start a:", x) // 输出10
   	x = 20
   	defer func(x int) {
   		println("in defer:", x)  // 输出20
   	}(x)
   	x = 30
   	println("leaving a:", x) // 输出30
   	return x
       // 调用defer匿名函数，输出20
       
   }
   ```

   ```go
   package main
   var x = 10
   func main() {
   	a()
   }
   
   func a() int {
   	println("start a:", x) // 输出10
   	x = 20
   	defer func() {      // 压栈，但并未传值，所以内部引用x
   		println("in defer:", x)  // 输出30
   	}()
   	x = 30
   	println("leaving a:", x) // 输出30
   	return x
       // 调用defer匿名函数，输出30,这是因为的defer那个函数是无参函数，没有值copy的过程，里面的x是外面的那个x变量，而这个x变量在之后被改成30了，所以最后输出是30。
   }
   ```

   ```go
   func a() int {
   	println("start a:", x) // 输出10
   	x = 20
   	{
   		x := 40
   		defer func() {
   			println("in defer:", x)  // 输出40
   		}()
   	}
   	x = 30
   	println("leaving a:", x) // 输出30
   	return x
       //调用defer匿名函数，输出40；上面的defer定义在语句块中，它能看见的x是语句块中x=40，它的x指向的是语句块中的x。另一方面，当语句块结束时，x=40的x会消失，但由于defer的函数中仍有x指向40这个值，所以40这个值仍被defer的函数引用着，它直到defer执行完之后才会被GC回收。
   }
   ```

4. **defer的应用场景**：

   - 打开关闭文件
   - 锁定、释放锁
   - 建立连接、释放连接
   - 作为结尾输出结尾信息
   - 清理垃圾(如临时文件)

5. 

#### 六、**panic()与recover()**;

1. panic()用于产生错误信息并终止**当前的goroutine**,一般将其看作是退出panic()所在函数以及退出调用panic()所在函数的函数。例如，G()中调用F()，F()中调用panic()，则F()退出，G()也退出；

   ```go
   func main() {
   	println("start main")
   	a()
   	println("end main")
   }
   
   func a() {
   	println("start a")
   	panic("panic in a")
   	println("end a")
   }
   //执行结果
   start main
   start a
   panic: panic in a
   ```

2. 可以使用recover()去捕获panic()并恢复执行。recover()用于捕捉panic()错误，并返回这个错误信息。但注意，即使recover()捕获到了panic()，但调用含有panic()函数的函数(即上面的G()函数)也会退出，所以如果recover()定义在G()中，则G()中调用F()函数之后的代码都不会执行(见下面的通用格式)，所以recover在G中应该放到F的前面。

   ```go
   package main
   import "fmt"
   func main() {
   	println("start main")
   	G()
   	println("end main")
   }
   
   func F() {
   	println("start F")
   	panic("panic in F")
   	println("end F")
   }
   
   func G() {
   	println("start G")
   	defer func() {
   		if str := recover(); str != nil {
   			fmt.Println(str)
   		}
   	}()
       // F()的调用必须在defer关键字之后
   	F()
       // 该函数内下面的代码不会执行
   	println("end G")
   }
   //输出：
   start main
   start G
   start F
   panic in F
   end main
   ```

3. panic()是内置的函数(在包builtin中)，在`log`包中也有一个Panic()函数，它调用Print()输出信息后，再调用panic()，`func Panic(v ...interface{})`；

4. 

#### 七、回调函数和闭包；

1. 高阶函数的两个特性：

   - 函数可以作为另一个函数的参数(典型用法是回调函数)
   - 函数可以返回另一个函数，即让另一个函数作为这个函数的返回值(典型用法是闭包)

   一般来说，附带的还具备一个特性：函数可以作为一个值赋值给变量。

2. 由于Go中函数不能嵌套命名函数，所以函数返回函数的时候，只能返回匿名函数。函数作为参数的时候也需要作为匿名函数。

3. **回调函数**：将函数B作为另一个函数A的参数，可以使得函数A的通用性更强，可以随意定义函数B，只要满足规则，函数A都可以去处理，这比较适合于回调函数。B函数不一定在A函数中最后执行，可以放在任何位置执行。

   ```go
   package main
   import "fmt"
   
   type Callback func(x, y int) int
   
   func testCallback(x, y int, callback Callback) int {
   	return callback(x, y)
   }
   
   /* 
   //直接在testCallback里面定义也可以
   func testCallback(x, y int, callback func(x, y int) int) int {
   	return callback(x, y)
   }
   */
   
   func add(x, y int) int {
   	return x + y
   }
   
   func minus(x, y int) int {
   	return x - y
   }
   func main() {
   	fmt.Println(testCallback(4, 5, add))//这里调用的时候，直接写入函数名就行了。
   	fmt.Println(testCallback(4, 5, minus))
   }
   //输出：
   9
   -1
   ```

4. **闭包**：将函数B作为另一个函数A的返回值。作用是"一个函数+一个作用域环境"组成的特殊函数，使得函数B可以访问不是它自己内部的变量，可以访问并持有函数A中的局部变量。

   ```go
   package main
   import "fmt"
   
   func f(x int) func(int) int{
       g := func(y int) int{
           return x+y
       }
       // 返回闭包，g是闭包函数
       return g
   }
   
   func main() {
       // 将函数的返回结果"闭包"赋值给变量a
       a := f(3)
       // 调用存储在变量中的闭包函数
       res1 := a(5)
       res2 := a(6)
       fmt.Println(res1)
       fmt.Println(res2)
   
       // 可以直接调用闭包
       // 因为闭包没有赋值给变量，所以它称为匿名闭包
       fmt.Println(f(3)(5))
       fmt.Println(f(3)(5))
   }
   //上面的f()返回的g之所以称为闭包函数，是因为它是一个函数，且引用了不在它自己范围内的变量x，这个变量x是g所在作用域环境内的变量，
   //因为x是未知、未赋值的自由变量。如果x在传递给g之前是已经赋值的，那么闭包函数就不应该称为闭包，因为这样的闭包已经失去意义了。
   //闭包的作用很明显，在f(3)退出后，它返回的闭包函数g()仍然记住了原本属于f()中的`x=3`。这样就可以让很多闭包函数共享同一个自由变量x的值。但是下面的f(3)(5)属于匿名闭包，两个匿名闭包中的x=3是相互独立的。
   ```

   ```go
   package main
   import "fmt"
   
   func main() {
   	// 自由变量x
   	var x int
   	// 闭包函数g
   	g := func(i int) int {
   		return x + i
   	}
   	x = 5
   	// 调用闭包函数
   	fmt.Println(g(5))
   	x = 10
   	// 调用闭包函数
   	fmt.Println(g(3))
   }
   //之所以这里的g也是闭包函数，是因为g中访问了不属于自己的变量x，而这个变量在闭包函数定义时是未绑定值的，也就是自由变量。
   ```

5. 

#### 八、**struct**；

1. struct定义结构，结构由字段(field)组成，每个field都有所属数据类型，在一个struct中，每个字段名都必须唯一。

2. Go中不支持面向对象，面向对象中描述事物的类的重担由struct来挑。比如面向对象中的继承，可以使用组合(composite)来实现：struct中嵌套一个(或多个)类型。Go中的组合则是外部struct与内部struct的关系、struct实例与struct的关系，它们是`has a`的关系。

3. **定义和实例化**：

   ```go
   //定义struct
   type identifier struct {
       field1 type1
       field2 type2
       …
   }
   //实例化
   //1
   var p person
   p.name = "longshuai"
   p.age = 23
   //2、实例化并赋值：必须要用 ， 分割
   var p person = person{name:"longshuai",age:23}
   p := person{"longshuai",23}//属性全写时，不用写字段名字
   //3、new()函数：只能实例化，之后再赋值，这个时候p是指针类型
   p := new(person)
   p.name = "longshuai"
   p.age = 23
   //4、&TYPE{}：可以实例化并赋值，这个时候p是指针类型
   p := &person{}
   p.name = "longshuai"
   p.age = 23
   	//实例化并赋值
   p := &person{
       name:"longshuai",
       age:23,//注意这里的逗号不能省略
   }
   ```

4. **struct的值和指针**：

   ```go
   package main
   import "fmt"
   
   type person struct {
   	name string
   	age  int
   }
   
   func main() {
   	p1 := person{}
   	p2 := &person{}
   	p3 := new(person)
   	fmt.Println(p1)
   	fmt.Println(p2)
   	fmt.Println(p3)
       //访问属性的方法都是一样的 p1.name,p2.name,p3.name
   }
   //输出
   { 0}
   &{ 0}
   &{ 0}
   //p1、p2、p3都是person struct的实例，但p2和p3是完全等价的，它们都指向实例的指针，指针中保存的是实例的地址，所以指针再指向实例，p1则是直接指向实例。
    变量名      指针     数据对象(实例)
   -------------------------------
   p1(addr) -------------> { 0}
   p2 -----> ptr(addr) --> { 0}
   p3 -----> ptr(addr) --> { 0}
   //var p4 *person, &person{}或new(person)赋值给p4。
   var p4 *person
   
   p4 = &person{
       name:"longshuai",
       age:23,
   }
   fmt.Println(p4) 
   ```

5. **传值 or 传指针**:Go函数给参数传递值的时候是以复制的方式进行的。

6. **struct field的tag属性**：

   ```go
   type TagType struct { // tags
       field1 bool   "An important answer"
       field2 string "The name of the thing"
       field3 int    "How much there are"
   }
   ```

7. **匿名字段和匿名struct嵌套**:

   ```go
   package main
   import "fmt"
   
   type inner struct {
   	in1 int
   	in2 int
   }
   
   type outer struct {
   	ou1 int
   	ou2 int
   	int		//匿名字段，相当于 int int,字段名字就是int
   	inner   //匿名struct嵌套
   }
   
   func main() {
   	o := new(outer)
   	o.ou1 = 1
   	o.ou2 = 2
   	o.int = 3    //匿名字段
   	o.in1 = 4
   	o.in2 = 5
   	fmt.Println(o.ou1)  // 1
   	fmt.Println(o.ou2)  // 2
   	fmt.Println(o.int)  // 3
   	fmt.Println(o.in1)  // 4
   	fmt.Println(o.in2)  // 5
   }
   //struct的嵌套类似于面向对象的继承。只不过继承的关系模式是"子类 is a 父类"，例如"轿车是一种汽车"。而嵌套struct的关系模式是外部struct has a 内部struct，正如上面示例中outer拥有inner。而且，从上面的示例中可以看出，Go是支持"多重继承"的。
   //也可以这么来创建外部struct
   o := outer{1,2,3,inner{4,5}}
   ```

8. **具名struct嵌套**：直接带名称嵌套struct时，不会再自动深入到嵌套struct中去查找属性和方法。想要访问内部struct属性时，必须带上该struct的名称。

   ```go
   package main
   import "fmt"
   
   type animal struct {
   	name string
   	age int
   }
   type Horse struct{
   	a animal
   	sound string
   }
   
   func main(){
   	var horse=Horse{animal{"baiju",2},"sisisi"}
   	fmt.Println(horse)
   	fmt.Println(horse.a.name)
   }
   //输出：
   {{baiju 2} sisisi}
   baiju
   ```

9. **嵌套struct的名称冲突**：

   - 外部struct覆盖内部struct的同名字段、同名方法；相当于重写（override），可以重写字段、方法。

   - 同级别的struct出现同名字段、方法将报错；

     ```go
     type A struct {
         a int
         b int
     }
     
     type B struct {
         b float32
         c string
         d string
     }
     
     type C struct {
         A
         B
         a string
         c string
     }
     
     var c C
     //按照规则(1)，直属于C的a和c会分别覆盖A.a和B.c。可以直接使用c.a、c.c分别访问直属于C中的a、c字段，使用c.d或c.B.d都访问属于嵌套的B.d字段。如果想要访问内部struct中被覆盖的属性，可以c.A.a的方式访问。
     //按照规则(2)，A和B在C中是同级别的嵌套结构，所以A.b和B.b是冲突的，将会报错，因为当调用c.b的时候不知道调用的是c.A.b还是c.B.b。
     ```

10. **嵌套自身**:递归struct

    1. 如果struct中嵌套的struct类型是自己的指针类型，可以用来生成特殊的数据结构：链表(单链表、双向链表)或二叉树(双端链表)。

    2. **单链**：定义一个单链表数据结构，每个Node都指向下一个Node，最后一个Node指向空。

       ```go
       type Node struct {
       	data string
       	ri   *Node
       }
       ```

    3. 嵌套两个自己的指针，每个结构都有一个左指针和一个右指针，分别指向它的左边节点和右边节点，就形成了二叉树或双端链表数据结构。

    4. **二叉树**:

       ```go
       type Tree struct {
       	le   *Tree
       	data string
       	ri   *Tree
       }
       // 生成两个新节点：初始为空
       newLeft := new(Tree)
       newLeft.data = "left node"
       newRight := &Tree{nil, "Right node", nil}
       // 添加到树中
       root.le = newLeft
       root.ri = newRight
       ```

11. **struct的导出问题**：struct本身的导出和嵌套struct中方法的访问是不同的。

    - **如果struct名称首字母是小写的，这个struct不会被导出。连同它里面的字段也不会导出，即使有首字母大写的字段名**。
    - **如果struct名称首字母大写，则struct会被导出，但只会导出它内部首字母大写的字段，那些小写首字母的字段不会被导出**。

12. **嵌套struct中方法的访问**：

    - 当内部struct嵌套进外部struct时，内部struct的方法也会被嵌套，也就是说外部struct拥有了内部struct的方法。

    - 但是需要注意方法的首字母大小写问题和是否同一个包的问题。

      - 如果内、外struct在同一包内，直接在该**包内**构建外部struct**实例**，外部struct实例是可以直接访问内部struct的所有方法的。

      - 但如果在**其它包内**构建外部struct实例，该**实例**将无法访问内部struct中首字母小写的方法。

        ```go
        package abc
        import "fmt"
        
        // 未导出的person
        type person struct {
        	name string
        	age  int
        }
        
        // 未导出的方法
        func (p *person) speak() {
        	fmt.Println("speak in person")
        }
        
        // 导出的方法
        func (p *person) Sing() {
        	fmt.Println("Sing in person")
        }
        
        // Admin exported
        type Admin struct {
        	person
        	salary int
        }
        //另一个包
        
        package main
        import "./abc"
        
        func main() {
        	a := new(abc.Admin)
        	// 下面报错
        //	a.speak()//要是在一个包里就ok
        
        	// 下面正常
        	a.Sing()
        }
        ```

        

    - 

#### 九、**Go方法**；

1. Go中的struct结构类似于面向对象中的类。面向对象中，除了成员变量还有方法。Go中也有方法，它是一种特殊的函数，定义于struct之上(与struct关联、绑定)，被称为struct的receiver。

   ```go
   type mytype struct{}
   
   func (recv mytype) my_method(para) return_type {}
   func (recv *mytype) my_method(para) return_type {}
   
   //调用
   mytype.my_method()
   ```

2. **注意事项**：

   - 方法的receiver type并非一定要是struct类型，type定义的类型别名、slice、map、channel、func类型等都可以（必须是别名）。但内置简单数据类型(int、float等)不行，interface类型不行。

     ```go
     // int类型
     package main
     import "fmt"
     
     type myint int
     func (i *myint) numadd(n int) int {
     	return n + 1
     }
     
     func main() {
     	n := new(myint)
     	fmt.Println(n.numadd(4))
     }
     
     //slice类型
     package main
     import "fmt"
     
     type myslice []int
     func (v myslice) sumOfSlice() int {
     	sum := 0
     	for _, value := range v {
     		sum += value
     	}
     	return sum
     }
     
     func main() {
     	s := myslice{11, 22, 33}
     	fmt.Println(s.sumOfSlice())
     }
     ```

   - struct结合它的方法就等价于面向对象中的类。只不过struct可以和它的方法分开，**并非一定要属于同一个文件，但必须属于同一个包**。所以，没有办法直接在int、float等内置的简单类型上定义方法，真要为它们定义方法，可以像上面示例中一样使用type定义这些类型的别名，然后定义别名的方法。

   - 方法就是函数，所以Go中没有方法重载(overload)的说法，也就是说同一个类型中的所有方法名必须都唯一。但不同类型中的方法，可以重名。

     ```go
     func (a *mytype1) add() ret_type {}
     func (a *mytype2) add() ret_type {}
     ```

   - type定义类型的别名时，别名类型不会拥有原始类型的方法。例如mytype上定义了方法add()，mytype的别名new_type不会有这个方法，除非自己重新定义。

   - 如果receiver是一个指针类型，则会自动解除引用。例如，下面的a是指针，它会自动解除引用使得能直接调用属于mytype1实例的方法add()。

     ```go
     func (a *mytype1) add() ret_type {}
     a.add()
     ```

   - `(T Type)`或`(T *Type)`的T，其实就是面向对象语言中的this或self，表示调用该实例的方法。指针的话会实际修改struct中的变量，实例的话，会拷贝生成副本。

   - 

3. **嵌套struct中的方法**：当内部struct嵌套进外部struct时，内部struct的方法也会被嵌套，也就是说外部struct拥有了内部struct的方法。

   ```go
   package main
   import "fmt"
   
   type person struct{}
   
   func (p *person) speak() {
   	fmt.Println("speak in person")
   }
   
   // Admin exported
   type Admin struct {
   	person   //实例
   	a int
   }
   
   func main() {
   	a := new(Admin)
   	// 直接调用内部struct的方法
   	a.speak()
   	// 间接调用内部stuct的方法
   	a.person.speak()
   }
   //输出
   speak in person
   speak in person
   //当person被嵌套到Admin中后，Admin就拥有了person中的属性，包括方法speak()。所以，a.speak()和a.person.speak()都是可行的。
   
   //如果Admin也有一个名为speak()的方法，那么Admin的speak()方法将掩盖内部struct的person的speak()方法。所以a.speak()调用的将是属于Admin的speak()，而a.preson.speak()将调用的是person的speak()。
   func (a *Admin) speak() {
   	fmt.Println("speak in Admin")
   }
   
   func main() {
   	a := new(Admin)
   	// 直接调用内部struct的方法
   	a.speak() 
   	// 间接调用内部stuct的方法
   	a.person.speak()
   }
   //输出
   speak in Admin
   speak in person
   ```

4. **嵌入方法的第二种方式**：

   - 除了可以通过嵌套的方式获取内部struct的方法，还有一种方式可以获取另一个struct中的方法：**将另一个struct作为外部struct的一个命名字段**。

     ```go
     package main
     import "fmt"
     
     type person struct {
     	name string
     	age  int
     }
     
     type Admin struct {
     	people *person   //指针
     	salary int
     }
     
     func main() {
         // 构建Admin实例
     	a := new(Admin)
     	a.salary = 2300
     	a.people = new(person)
     	a.people.name = "longshuai"
     	a.people.age = 23
         // 或a := &Admin{&person{"longshuai",23},2300}
     
         // 调用属于person的方法speak()
     	a.people.speak()
     }
     
     func (p *person) speak() {
     	fmt.Println("speak in person")
     }
     ```

     现在Admin除了自己的salary属性，还指向一个person。这和struct嵌套不一样，struct嵌套是直接外部包含内部，而这种组合方式是一个struct指向另一个struct，从Admin可以追踪到其指向的person。person是Admin type中的一个字段，person有方法speak()。

     或者，定义一个属于Admin的方法，在此方法中应用person的方法,然后只需调用`a.sing()`就可以隐藏person的方法。

     ```go
     func (a *Admin) sing(){
     	a.people.speak()
     }
     ```

   - 

5. **多重继承**:因为Go的struct支持嵌套多个其它匿名字段，所以支持"多重继承"。这意味着外部struct可以从多个内部struct中获取属性、方法。

6. **重写String()方法**：fmt包中的Println()、Print()和Printf()的`%v`都会自动调用String()方法将待输出的内容进行转换。

   可以在自己的struct上重写String()方法，使得输出这个示例的时候，就会调用它自己的String()。

   一定要注意，定义struct的String()方法时，String()方法里不要出现fmt.Print()、fmt.Println以及fmt.Printf()的`%v`，因为它们自身会调用String()，会出现无限递归的问题。

   例如，定义person的String()，它将person中的name和age结合起来：

   ```go
   package main
   import (
   	"fmt"
   	"strconv"
   )
   
   type person struct {
   	name string
   	age  int
   }
   
   func (p *person) String() string {
   	return p.name + ": " + strconv.Itoa(p.age)
   }
   
   func main() {
   	p := new(person)
   	p.name = "longshuai"
   	p.age = 23
       // 输出person的实例p，将调用String()
   	fmt.Println(p)
   }
   //输出
   longshuai: 23
   ```


#### 十一、**接口**；

1. 是一种类型，用来定义行为（方法），也是一个指针。

   ```go
   type Namer interface {
       my_method1()
       my_method2(para)
       my_method3(para) return_type
       ...
   }
   ```

2. 接口实际上是指针：当接口实例中保存了自定义类型的实例后，就可以直接从接口上调用它所保存的实例的方法，通过这种形式实现多态。

3. 接口实例中存什么：接口类型的数据结构是2个指针，占用2个机器字长。

   - 第二个指针指向实例对象地址；
   - 第一个指针是指向一个内部表结构**iTable**，iTable分为两部分：
     - 第一部分是实例对象c的类型信息：值或者指针
     
     - 第二部分是方法集，实例类型为T（值类型）的，方法集中只有receiver为T的方法；实例类型为\*T(指针类型)的，方法集中有receiver为\*T和T的方法；
     
     ![image](https://github.com/Liukemeng/Golang-Learning/blob/master/figures/WechatIMG33.png)
     
     ![image](https://github.com/Liukemeng/Golang-Learning/blob/master/figures/WechatIMG32.png)

4. 从receiver角度考虑：

   - **如果某类型实现接口的方法的receiver是`(T Type)`类型的，那么值类型的实例`T`和指针类型的实例`*T`都算实现了这个接口，都可以通过接口实例调用这个方法**。因为这个方法既在值类型的实例`T`方法集中，也在指针类型的实例`*T`方法集中

   - **如果某类型实现接口的方法的receiver是`(T *Type)`类型的，那么只有指针类型的实例`*T`才算是实现了这个接口，只有指针类型的实例才能调用这个方法**。因为这个方法不在值类型的实例`T`方法集中。
     
     ![image](https://github.com/Liukemeng/Golang-Learning/blob/master/figures/WechatIMG31.png)
     

     ```go
     package main
     import "fmt"
     
     // Shaper 接口类型
     type Shaper interface {
     	Area() float64
     }
     
     // Circle struct类型
     type Circle struct {
     	radius float64
     }
     
     // Circle类型实现Shaper中的方法Area()
     // receiver类型为指针类型
     func (c *Circle) Area() float64 {
     	return 3.14 * c.radius * c.radius
     }
     
     func main() {
     	// 声明2个接口实例
     	var ins1, ins2 Shaper
     
     	// Circle的指针类型实例
     	c1 := new(Circle)
     	c1.radius = 2.5
     	ins1 = c1
     	fmt.Println(ins1.Area())
     
     	// Circle的值类型实例
     	c2 := Circle{3.0}
     	// 下面的将报错
     	ins2 = c2
     	fmt.Println(ins2.Area())
     }
     //如果上面Area方法的receiver为值类型的话，c1和c2赋值给接口实例后，都可以调用。
     ```

5. **普通方法和实现接口方法的区别**：实现接口的方法按照上面的思路进行调用。

   ```go
   type person struct {}
   
   func (T Type) method1   // 值类型receiver
   func (T *Type) method2  // 指针类型receiver
   
   p1 := person{}       // 值类型的实例
   p2 := new(person)    // 指针类型的实例
   ```

   - p1和p2均可以调用method1()和method2()。
     - 只不过，p1.method1()和p2.method1()都是拷贝实例；然后p2.method1()自动解除引用，相当于(*p2).method2()。
     - p1.method2()和p2.method2()都是拷贝引用；然后p1.method2()自动创建引用，相当于(&p1).method2()。

6. **接口类型作为参数（其实同4）**：本质也是将结构体实例复制给接口实例。

   ```go
   package main
   
   import (
   	"fmt"
   )
   
   // Shaper 接口类型
   type Shaper interface {
   	Area() float64
   }
   
   // Circle struct类型
   type Circle struct {
   	radius float64
   }
   
   // Circle类型实现Shaper中的方法Area()
   func (c *Circle) Area() float64 {
   	return 3.14 * c.radius * c.radius
   }
   
   func main() {
   	// Circle的指针类型实例
   	c1 := new(Circle)
   	c1.radius = 2.5
   	myArea(c1)
   }
   
   func myArea(n Shaper) {
   	fmt.Println(n.Area())
   }
   //上面myArea(c1)是将c1作为接口类型参数传递给n，然后调用c1.Area()，因为实现了接口方法，所以调用的是Circle的Area()。
   
   //如果实现接口方法的receiver是指针类型的，那么实参只能是指针类型的实例，不能是值类型的实例；
   //如果实现接口方法的receiver是值类型的，那么实参可以是指针类型的实例，也可以是值类型的实例；
   ```

7. **空接口**：没有定义任何接口方法的接口。没有定义任何接口方法，意味着Go中的任意对象都可以实现空接口(因为没方法需要实现)，任意对象都可以保存到空接口实例变量中。

   ```go
   // 声明一个空接口实例
   var i interface{}
   i="hello world"
   i=11
   //空接口是一种接口，它是一种指针类型的数据类型，虽然不严谨，但它确实保存了两个指针，一个是对象的类型(或iTable)，一个是对象的值。
   //所以上面的赋值过程是让空接口i保存各个数据对象的类型和对象的值。
   ```

8. **空接口数据结构**:可以定义一个空接口类型的array、slice、map、struct等，这样它们就可以用来存放任意类型的对象，因为任意类型都实现了空接口。

   ```go
   package main
   import "fmt"
   
   func main() {
   	any := make([]interface{}, 5)
   	any[0] = 11
   	any[1] = "hello world"
   	any[2] = []int{11, 22, 33, 44}
   	for _, value := range any {
   		fmt.Println(value)
   	}
   }
   //输出
   11
   hello world
   [11 22 33 44]
   <nil>
   <nil>
   ```

   再比如，某个struct中，如果有一个字段想存储任意类型的数据，就可以将这个字段的类型设置为空接口：

   ```go
   type my_struct struct {
   	anything interface{}
   	anythings []interface{}
   }
   ```

9. **拷贝数据结构到空接口数据结构**：

   ```go
   package main
   import "fmt"
   
   func main() {
   	testSlice := []int{11,22,33,44}
   
   	// 成功拷贝
   	var newSlice []int
   	newSlice = testSlice
   	fmt.Println(newSlice)
   
   	// 拷贝失败
   	var any []interface{}
   	any = testSlice
   	fmt.Println(any)
       
       // 拷贝成功
       var any []interface{}
   		for _,value := range testSlice{
   				any = append(any,value)
   		}
     	
     	//成功拷贝
     var any interface{}
     any=testSlice     //这个时候any类型就是[]int了
     fmt.Println(any)
     
   }
   //这是因为每个空接口的内存布局都占用两个机器字长的内容。对于长度为N的空接口slice来说，它的每个元素都是以2机器字长为单元的连续空间，共占用N*2个机器字长的空间。
   //而普通的slice，例如上面的testSlice，它的每个元素是int类型的，int类型的内存布局和空接口不一样。
   //这些对象的内存布局在编译期间就已经确定好了，所以没法直接将不同内存布局的数据结构进行拷贝。
   //要想完成期待的拷贝，可以使用for-range的方式，将testSlice中的每个元素赋值给空接口slice的空接口元素：也就是一个个的空接口实例。
   //这样，空接口Slice中的每个空接口实例都指向更底层的各个数据对象。而不是像前面错误的拷贝方式：每个空接口元素想要当作这些数据对象。
   //不仅空接口的Slice如此，其它包含空接口的数据结构，也都类似。
   ```

10. **接口转回具体类型**：

    - 接口实例中可以存放各种实现了接口的类型实例，在有需要的时候，还可以通过`ins.(Type)`或`ins.(*Type)`的方式将接口实例ins直接转回Type类型的实例。

      ```go
      var i int = 30
      var ins interface{}
      
      // 接口实例ins中保存的是int类型
      ins = i
      x := ins.(int)  // 接口转回int类型的实例i
      println(x) //输出30
      println("i addr: ",&i,"x addr: ",&x)//输出0xc042049f68    0xc042049f60
      ```

    - 注意，这时候的i和x在底层不是同一个对象，它们的地址是不同的。

      ```go
      package main
      import "fmt"
      
      // Shaper 接口类型
      type Shaper interface {
      	Area() float64
      }
      
      // Square struct类型
      type Square struct {
      	length float64
      }
      
      // Square类型实现Shaper中的方法Area()
      func (s Square) Area() float64 {
      	return s.length * s.length
      }
      
      func main() {
      	var ins1, ins2 Shaper
      
      	// 指针类型的实例
      	s1 := new(Square)
      	s1.length = 3.0
      	ins1 = s1
      	if v, ok := ins1.(*Square); ok {
      		fmt.Printf("ins1: %T\n", v)
      	}
      
      	// 值类型的实例
      	s2 := Square{4.0}
      	ins2 = s2
      	if v, ok := ins2.(Square); ok {
      		fmt.Printf("ins2: %T\n", v)
      	}
      }
      //输出
      ins1: *main.Square
      ins2: main.Square
      ```

      

    - 注意，接口实例转回时，**接口实例中存放的是什么类型，才能转换成什么类型**。receiver是值类型的话，可以对值类型和指针类型的实例进行转换；receiver是指针类型的话，只可以对指针类型的实例进行转换。（跟上面的4一样，这是因为结构体实例先转为接口实例，之后才是接口实例转为结构体实例）

    - 类型探测的方式和类型转换的方式都是`ins.(Type)`和`ins.(*Type)`。当处于单个返回值上下文时，做的是类型转换，当处于两个返回值的上下文时，做的是类型探测。类型探测的第一个返回值是类型转换之后的类型实例，第二个返回值是布尔型的ok返回值。

      

    - **type Switch结构**：直接用`if v,ok := ins.(Type);ok {}`的方式做类型探测在探测类型数量多时不是很方便，需要重复写if结构。Golang提供了switch...case结构用于做多种类型的探测，所以**这种结构也称为type-switch**。这是比较方便的语法，比如可以判断某接口如果是A类型，就执行A类型里的特有方法，如果是B类型，就执行B类型里的特有方法。

      ```go
      package main
      import "fmt"
      
      // Shaper 接口类型
      type Shaper interface {
      	Area() float64
      }
      
      // Circle struct类型
      type Circle struct {
      	radius float64
      }
      
      // Circle类型实现Shaper中的方法Area()
      func (c *Circle) Area() float64 {
      	return 3.14 * c.radius * c.radius
      }
      
      // Square struct类型
      type Square struct {
      	length float64
      }
      
      // Square类型实现Shaper中的方法Area()
      func (s Square) Area() float64 {
      	return s.length * s.length
      }
      
      func main() {
      	s1 := &Square{3.3}
      	whichType(s1)
      
      	s2 := Square{3.4}
      	whichType(s2)
      
      	c1 := new(Circle)
      	c1.radius = 2.3
      	whichType(c1)
      }
      
      func whichType(n Shaper) {
      	switch v := n.(type) {//n.(type)中的小写type是固定的词语。
      	case *Square:
      		fmt.Printf("Type Square %T\n", v)
      	case Square:
      		fmt.Printf("Type Square %T\n", v)
      	case *Circle:
      		fmt.Printf("Type Circle %T\n", v)
      	case nil:
      		fmt.Println("nil value: nothing to check?")
      	default:
      		fmt.Printf("Unexpected type %T", v)
      	}
      }
      //上面的type-switch中，之所以没有加上case Circle，是因为Circle只实现了指针类型的receiver，根据Method Set对接口的实现规则，只有指针类型的Circle示例才算是实现了接口Shaper，所以将值类型的示例case Circle放进type-switch是错误的。
      ```

11. nihao



-------------------------------------------------------------------**Go 并发-**----------------------------------------------------------------------------

#### 十二、Waitgroup的用法；

1. 新激活的goroutine的结束过程是不可控制的，唯一可以保证终止goroutine的行为是main goroutine的终止。也就是说，我们并不知道哪个goroutine什么时候结束。但很多情况下，我们需要知道goroutine是否完成。这需要借助sync包的WaitGroup来实现。

2. WaitGroup的结构和方法：

   ```go
   type WaitGroup struct {
           // Has unexported fields.
   }
   
   func (wg *WaitGroup) Add(delta int)
   func (wg *WaitGroup) Done()
   func (wg *WaitGroup) Wait()
   ```

   - Add()：每次激活想要被等待完成的goroutine之前，先调用Add()，用来设置或添加要等待完成的goroutine数量;

     - 例如Add(2)或者两次调用Add(1)都会设置等待计数器的值为2，表示要等待2个goroutine完成

   - Done()：每次需要等待的goroutine在真正完成之前，应该调用该方法来人为表示goroutine完成了，该方法会对等待计数器减1

   - Wait()：在等待计数器减为0之前，Wait()会一直阻塞当前的goroutine

     ```go
     package main
     import (  
         "fmt"
         "sync"
         "time"
     )
     
     func process(i int, wg *sync.WaitGroup) {  
         fmt.Println("started Goroutine ", i)
         time.Sleep(2 * time.Second)
         fmt.Printf("Goroutine %d ended\n", i)
         wg.Done()
     }
     
     func main() {  
         no := 3
         var wg sync.WaitGroup
         for i := 0; i < no; i++ {
             wg.Add(1)
             go process(i, &wg)
         }
         wg.Wait()
         fmt.Println("All go routines finished executing")
     }
     //process()中使用指针类型的*sync.WaitGroup作为参数，这里不能使用值类型的sync.WaitGroup作为参数，因为这意味着每个goroutine都拷贝一份wg，每个goroutine都使用自己的wg。这显然是不合理的，这3个goroutine应该共享一个wg，才能知道这3个goroutine都完成了。
     ```

   - 

3. 

#### 十三、channel基础；

1. 底层实现：一个环形数组实现的队列，用于存储消息元素；两个链表实现的 goroutine 等待队列，用于存储阻塞在 recv 和 send 操作上的 goroutine；一个互斥锁，用于各个属性变动的同步。

   <img src="/Users/liukemeng/Library/Application Support/typora-user-images/image-20200718181800154.png" alt="image-20200718181800154" style="zoom:50%;" />
   
2. **channel用于goroutines之间的通信，让它们之间可以进行数据交换。**channel是指针类型的数据类型，通过make来分配内存。

   ```go
   //创建通道
   ch := make(chan Type, cap int)
   ```

3. **channel的三种操作**：

   - send：表示sender端的goroutine向channel中投放数据，channel满了，就阻塞；
   - receive：表示receiver端的goroutine从channel中读取数据，channel没有数据了，就阻塞；
   - close：表示关闭channel，关闭的时候会移除sendq和receiveq中的所有的goroutine，并唤醒他们。
     - 只在sender端上显式使用close()关闭channel，因为关闭通道意味着没有数据再需要发送；
     - 关闭channel后，send操作将导致painc，关闭一个已经关闭或者是只读channel或者空channel也会引发panic。
     - 关闭channel后，recv操作将返回channel里面的剩余元素，没有剩余元素了，就返回对应类型的0值以及一个状态码false，（因为关闭通道也会成功读取通道里面的内容，所以可以使得原本阻塞的gorountine变得不阻塞）
     - close并非强制需要使用close(ch)来关闭channel，在某些时候可以自动被关闭；
     - 如果使用close()，建议条件允许的情况下加上defer；
     - 

4. **channel的分类**：**unbuffered channel和buffered channel**

   - **unbuffered channel**：阻塞、同步模式； ch:=make(chan type)，容量为0的buffered channel,发送一个数据就被阻塞。
     - sender端向channel中send一个数据，如果接收协程队列不为空，直接移出第一个接收协程，然后把值直接传递给协程。如果接收协程队列为空，这个发送协程将被阻塞，被放倒channel的发送协程队列里面。
     - receiver端从channel获取数据，如果发现发送队列不为空，唤醒发送队列的第一个协程，然后该发行协程将数据直接发送给接收协程。
   - **buffered channel**：非阻塞、异步模式；ch:=make(chan type, cap int)
     - sender端可以向channel中send多个数据(只要channel容量未满)，容量满之前不会阻塞，容量满了就阻塞，加入发送序列。
     - receiver端按照队列的方式(FIFO,先进先出)从buffered channel中按序receive其中数据，如果有数据直接取出，没有的话接收协程阻塞，加入到协程的接收队列里面。
   - **unbuffered channel和buffered channel的区别**：
     - 对于unbuffered channel，sender发送一个数据，channel暂时不会向sender的请求返回ok消息，而是等到receiver准备接收channel数据了，channel才会向sender和receiver双方发送ok消息。在sender和receiver接收到ok消息之前，两者一直处于阻塞。
     - 对于buffered channel，sender每发送一个数据，只要channel容量未满，channel都会向sender的请求直接返回一个ok消息，使得sender不会阻塞，直到channel容量已满，channel不会向sender返回ok，于是sender被阻塞。对于receiver也一样，只要channel非空，receiver每次请求channel时，channel都会向其返回ok消息，直到channel为空，channel不会返回ok消息，receiver被阻塞。
     - 

5. **两种特殊的channel**：nil channel和channel类型的channel。

   - 当未为channel分配内存时，channel就是nil channel，例如`var ch1 chan int`。nil channel会永远阻塞对该channel的读、写操作。

   - 当channel的类型为一个channel时，就是channel的channel，也就是双层通道。例如：

     ```go
     // chch1是通道类型的通道
     var chch1 chan chan int
     // 发送通道给外层通道
     chch1 <-ch1
     chch1 <-ch2
     
     // 从外层通道取出内层通道
     c <-chch1
     
     // 操作取出的内层通道
     c <-123
     val := <-c
     
     //channel of channel的妙用之一是将外层通道作为通道的加工厂：在某个goroutine中不断生成通道，在其它goroutine可以不断取出通道来操作。
     ```

   - 

6. **死锁**:

   - channel的某一端(sender/receiver)期待另一端的(receiver/sender)操作，另一端正好在期待本端的操作时，也就是说两端都因为对方而使得自己当前处于阻塞状态，这时将会出现死锁问题。更通俗地说，**只要所有goroutine都被阻塞，就会出现死锁**。
   - 

7. **for range迭代channel**：

   - 它会返回每次迭代过程中所读取的数据，直到channel被关闭。必须注意，只要channel未关闭，range迭代channel就会一直被阻塞。不像array、slice、map，channel只会返回一个变量。但是<-ch会返回两个变量。
   - 也可以外层直接用for无限循环来迭代读取channel中的数据。

8. **指定channel的方向**：

   - `in <-chan int`：表示channel in通道只用于接收数据（站在goroutine的角度），用户从in中读取数据。
   - `out chan<- int`：表示channel out通道只用于发送数据，用户向out中发送数据。

9. **buffered channel异步队列请求示例**：

   ```go
   package main
   import (
   	"fmt"
   	"math/rand"
   	"sync"
   	"time"
   )
   
   type Task struct {
   	ID         int
   	JobID      int
   	Status     string
   	CreateTime time.Time
   }
   
   func (t *Task) run() {
   	sleep := rand.Intn(1000)
   	time.Sleep(time.Duration(sleep) * time.Millisecond)
   	t.Status = "Completed"
   }
   
   var wg sync.WaitGroup
   // worker的数量，即使用多少goroutine执行任务
   const workerNum = 3
   
   func main() {
   	wg.Add(workerNum)
   
   	// 创建容量为10的buffered channel
   	taskQueue := make(chan *Task, 10)
   
   	// 激活goroutine，执行任务
   	for workID := 0; workID <= workerNum; workID++ {
   		go worker(taskQueue, workID)
   	}
   	// 将待执行任务放进buffered channel，共15个任务
   	for i := 1; i <= 15; i++ {
   		taskQueue <- &Task{
   			ID:         i,
   			JobID:      100 + i,
   			CreateTime: time.Now(),
   		}
   	}
   	close(taskQueue)
   	wg.Wait()
   }
   
   // 从buffered channel中读取任务，并执行任务
   func worker(in <-chan *Task, workID int) {
   	defer wg.Done()
   	for v := range in {
   		fmt.Printf("Worker%d: recv a request: TaskID:%d, JobID:%d\n", workID, v.ID, v.JobID)
   		v.run()
   		fmt.Printf("Worker%d: Completed for TaskID:%d, JobID:%d\n", workID, v.ID, v.JobID)
   	}
   }
   ```

10. **select多路监听**:很多时候想要同时操作多个channel，比如从ch1、ch2读数据。Go提供了一个select语句块，它像switch一样工作，里面放一些case语句块，用来轮询每个case语句块的send或recv情况。

   - 在一个select语句中，Go语言会按顺序从头至尾评估每一个发送和接收的语句。如果其中有多个case没有被阻塞，那么就从这些可以执行的语句中**随机选择**一条来执行。对于这一次的选择，其它的case都不会被阻塞，而是处理完被选中的case后进入下一轮select(如果select在循环中)或者结束select(如果select不在循环中或循环次数结束)

   - 如果没有case可以执行(即所有的通道都被阻塞)，那么有两种可能的情况：

     - 如果给出了default语句，那么就会执行default语句，同时程序的执行会从select语句后的语句中恢复；

     - 如果没有default语句，那么当前goroutine会阻塞，当前的goroutine会挂接到所有关联的channel内部的协程队列上。当一个阻塞的goroutine拿到了数据解除阻塞的时候，它会从所有相关的channel队列中移除掉。（ 所以说单个goroutine是可以同时挂接到多个channel上的，甚至可以同时挂接到同一个channel的发送协程队列和接收协程队列上）；

     - 

   - 注意：

     - 所有的case块都是按源代码书写顺序进行评估的。当select未在循环中时，它将只对所有case评估一次，这次结束后就结束select。某次评估过程中如果有满足条件的case，则所有其它case都直接结束评估，并退出此次select；
     - 其实如果注意到select语句是在某一个goroutine中评估的，就不难理解只有所有case都不满足条件时，select所在goroutine才会被阻塞，只要有一个case满足条件，本次select就不会出现阻塞的情况。
     - 一般来说，select会放在一个无限循环语句中，一直轮询channel的可读事件。
     - 

     ```go
     package main
     import (
     	"fmt"
     	"sync"
     )
     
     func push1(ch chan int, wg *sync.WaitGroup){
     	for i:=0;i<30;i++{
     		if i%2==0 {
     			ch <- i
     		}
     	}
     	wg.Done()
     }
     
     func push2(ch chan int,wg *sync.WaitGroup){
     	for i:=0;i<30;i++{
     		if i%2==1 {
     			ch <- i
     		}
     	}
     	wg.Done()
     }
     func get(ch1,ch2 chan int,){
     	for{
     		select {
     
     		case v:= <-ch2:
     			fmt.Println(v)
     		case v:= <-ch1:
     			fmt.Println(v)
     		}
     	}
     }
     
     func main() {
     	var wg sync.WaitGroup
     	wg.Add(2)
     	ch1:=make(chan int,15)
     	ch2:=make(chan int,15)
     	go push1(ch1,&wg)
     	go push2(ch2,&wg)
     
     	go  get(ch1,ch2)
     	wg.Wait()
     	fmt.Println("end")
     }
     ```

   - 你好

     

#### 十四、After()和TIck()方法；

1. ​	time.After()的作用：在d时间之后的时间点放到一个只读通道里面，来保障在select是不会永久阻塞的。这个方法只等待了一次，定义如下：

   ```go
   func After(d Duration) <-chan Time
   ```

   - 实例代码：

     ```go
     func main() {
     	ch1 := make(chan string)
     
     	// 激活一个goroutine，但5秒之后才发送数据
     	go func() {
     		time.Sleep(5 * time.Second)
     		ch1 <- "put value into ch1"
     	}()
     
     	select {
     	case val := <-ch1:
     		fmt.Println("recv value from ch1:",val)
     		return
     	// 只等待3秒，然后就结束
     	case <-time.After(3 * time.Second):
     		fmt.Println("3 second over, timeover")
     	}
     }
     //输出： 
     //3 second over, timeover
     //main协程到select这个地方，select中没有可执行的case，main协程阻塞在两个通道上，因为上面那个协程等了5秒，随后3s后下面那个case可执行。
     ```

     

   - 

2. time.TIck()的作用：间隔的地多次等待，示例代码如下：

   ```go
   package main
   
   import (
   	"fmt"
   	"time"
   )
   
   func main() {
   	tick := time.Tick(1 * time.Second)
   	after := time.After(7 * time.Second)
     
   	fmt.Println("start second:",time.Now().Second())
   	for {
   		select {
   		case <-tick:
   			fmt.Println("1 second over:", time.Now().Second())
   		case <-after:
   			fmt.Println("7 second over:", time.Now().Second())
   			return
   		}
   	}
   }
   /*
   输出：
   start second: 9
   1 second over: 10
   1 second over: 11
   1 second over: 12
   1 second over: 13
   1 second over: 14
   1 second over: 15
   1 second over: 16
   7 second over: 16
   */
   ```

3. 

4. 

#### 十五、nil channel的用法示例；

1. ​	当未为channel分配内存时，channel就是nil channel，nil channel会永久阻塞对该channel的所有读、写。所以，可以将某个channel设置为nil，进行强制阻塞，对于select分支来说，就是强制禁用此分支。

   ```go
   package main
   
   import (
   	"fmt"
   	"math/rand"
   	"time"
   )
   
   // 不断向channel c中发送[0,10)的随机数
   func send(c chan int) {
   	for {
   		c <- rand.Intn(10)
   	}
   }
   
   func add(c chan int) {
   	sum := 0
   
   	// 1秒后，将向t.C通道发送时间点，使其可读
   	t := time.NewTimer(1 * time.Second)
   
   	for {
   		// 一秒内，将一直选择第一个case
   		// 一秒后，t.C可读，将选择第二个case
   		// c变成nil channel后，两个case分支都将一直阻塞
   		select {
   		case input := <-c:
   			// 不断读取c中的随机数据进行加总
   			sum = sum + input
   		case <-t.C:
   			c = nil
   			fmt.Println(sum)
   		}
   	}
   }
   
   func main() {
   	c := make(chan int)
   	go add(c)
   	go send(c)
   	// 给3秒时间让前两个goroutine有足够时间运行
   	time.Sleep(3 * time.Second)
   }
   ```

   

2. 

#### 十六、互斥锁和读写锁；

​	注意：读写锁与互斥锁不会和goroutine进行关联，所以锁的申请和释放可以在不同的goroutine中。

1. 互斥锁：sync.Mutex中的Lock()和Unlock()方法实现对临界区（critical section）的加锁和解锁，保障同一时间只有一个goroutine进入到临界区，但是无法保障顺序。

   ```go
   package main
   
   import (
   	"fmt"
   	"sync"
   	"time"
   )
   
   // 共享变量
   var (
   	m  sync.Mutex
   	v1 int
   )
   
   // 修改共享变量
   // 在Lock()和Unlock()之间的代码部分是临界区
   func change(i int) {
   	m.Lock()
   	time.Sleep(time.Second)
   	v1 = i
   	m.Unlock()
   }
   
   // 访问共享变量
   // 在Lock()和Unlock()之间的代码部分是是临界区
   func read() int {
   	m.Lock()
   	a := v1
   	m.Unlock()
   	return a
   }
   
   func main() {
   	var numGR = 21
   	var wg sync.WaitGroup
   
   	fmt.Printf("%d", read())
   
   	// 循环创建numGR个goroutine
   	// 每个goroutine都执行change()、read()
   	// 每个change()和read()都会持有锁
   	for i := 0; i < numGR; i++ {
   		wg.Add(1)
   		go func(i int) {
   			defer wg.Done()
   			change(i)
   			fmt.Printf(" -> %d", read())
   		}(i)
   	}
   
   	wg.Wait()
   }
   //输出：0 -> 3 -> 20 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 12 -> 13 -> 14 -> 15 -> 16 -> 17 -> 18 -> 2 -> 0 -> 19 -> 1
   //有两个0，可以保障只有一个协程在读或者写，但是不能保障读写的原子性。
   ```

2. 适合sync.Mutex的数据类型

   - 对于内置类型的共享变量来说，使用sync.Mutex和Lock()、Unlock()来保护也是不合理的，因为它们自身不包含Mutex属性。真正合理的共享变量是那些包含Mutex属性的struct类型。例如：

     ```go
     type mytype struct {
     	m   sync.Mutex
     	var int
     }
     
     x := new(mytype)
     //这时只要想保护var变量，就先x.m.Lock()，操作完var后，再x.m.Unlock()。这样就能保证x中的var字段变量一定是被保护的。
     ```

     

   - 

3. 读写锁：sync.RWMutex是基于Mutex加上了读写的信号量，底层结构是：

   ```go
   type RWMutex struct {
   	w           Mutex  // held if there are pending writers
   	writerSem   uint32 // 写锁需要等待读锁释放的信号量
   	readerSem   uint32 // 读锁需要等待写锁释放的信号量
   	readerCount int32  // 读锁后面挂起了多少个写锁申请
   	readerWait  int32  // 已释放了多少个读锁
   }
   ```

   - 读锁和读锁是兼容的；读锁和写锁互斥；写锁和写锁、读锁都互斥。而且

   - 所以：有读锁可以再申请读锁，不能申请写锁；有写锁不能申请写锁、读锁。

     ```go
     func (rw *RWMutex) Lock()     //申请写锁
     func (rw *RWMutex) Unlock()   //释放写锁
     func (rw *RWMutex) RLock()    //申请读锁
     func (rw *RWMutex) RUnlock()  //释放读锁
     func (rw *RWMutex) RLocker() Locker   //返回一个实现了Lock()和Unlock()方法的Locker接口
     ```

   - 示例代码

     ```go
     package main
     
     import (
     	"fmt"
     	"sync"
     )
     
     type person struct{
     	age int
     	m sync.Mutex
     	rwm sync.RWMutex
     }
     func(p *person) changeWithMutex(age int,wg *sync.WaitGroup){
     	defer wg.Done()
     	p.m.Lock()
     	fmt.Printf("%s\n","写   Mutex")
     	p.age=age
     	fmt.Printf("%s\n","写完  Mutex")
     	p.m.Unlock()
     }
     func(p *person) readWithMutex(wg *sync.WaitGroup){
     	defer wg.Done()
     	p.m.Lock()
     	fmt.Printf("%s\n","读   Mutex")
     	fmt.Printf("%d\n",p.age)
     	fmt.Printf("%s\n","读完 Mutex")
     	p.m.Unlock()
     }
     //-----------------
     func(p *person) changeWithRWMutex(age int,wg *sync.WaitGroup){
     	defer wg.Done()
     	p.rwm.Lock()
     	fmt.Printf("%s\n","写   RWMutex")
     	p.age=age
     	fmt.Printf("%s\n","写完  RWMutex")
     	p.rwm.Unlock()
     }
     func(p *person) readWithRWMutex(wg *sync.WaitGroup){
     	defer wg.Done()
     	p.rwm.RLock()
     	fmt.Printf("%s\n","读   RWMutex")
     	fmt.Printf("%d\n",p.age)
     	fmt.Printf("%s\n","读完 RWMutex")
     	p.rwm.RUnlock()
     }
     func main(){
     	var mm sync.Mutex
     	var rwmm sync.RWMutex
     	var wg sync.WaitGroup
     	p:=person{20,mm,rwmm}
     	for i:=0;i<5;i++{
     		wg.Add(2)
     		go p.changeWithMutex(i,&wg)
     		//go p.readWithMutex(&wg)
     		go p.changeWithRWMutex(i,&wg)
     		//go p.readWithRWMutex(&wg)
     	}
     	wg.Wait()
     
     }
     ```

     **总结**：**sync.Mutex和sync.RWMutex是互补干扰的，各走各的。**

4. 

#### 十七、工作池；

1. 工作池的简介

   - 在线程池模型中，**有2个队列一个池子：任务队列、已完成任务队列和线程池**。其中已完成任务队列可能存在也可能不存在，依据实际需求而定。只要有任务进来，就会放进任务队列中。只要线程执行完了一个任务，就将任务放进已完成任务队列，有时候还会将任务的处理结果也放进已完成队列中。

     <img src="/Users/liukemeng/Library/Application Support/typora-user-images/image-20200718213948361.png" alt="image-20200718213948361" style="zoom:50%;" />

     

   - 线程池的两种实现方式：传统的互斥锁、channel

2. **传统互斥锁机制的线程池**：

   ```go
   //任务定义
   type Task struct {
   	...
   }
   //任务队列
   type Queue struct{
   	M     sync.Mutex  //最关键的锁，保障同一个时间点只有一个协程取得任务并更新任务列表
   	Tasks []Task
   }
   
   //执行任务的函数上面加上锁
   func Worker(queue *Queue) {
   	for {
   		// Lock()和Unlock()之间的是critical section
   		queue.M.Lock()
   		// 取出任务
   		task := queue.Tasks[0]
   		// 更新任务队列
   		queue.Tasks = queue.Tasks[1:]
   		queue.M.Unlock()
   		// 在此goroutine中执行任务
   		process(task)
   	}
   }
   //外层就是通过线程池激活n各goroutine来执行Worker方法。Lock()和Unlock()保证了在同一时间点只能有一个goroutine取得任务并随之更新任务列表，取任务和更新任务队列都是critical section中的代码，它们是具有原子性。然后这个goroutine可以执行自己取得的任务。于此同时，其它goroutine可以争夺互斥锁，只要争抢到互斥锁，就可以取得任务并更新任务列表。当某个goroutine执行完process(task)，它将因为for循环再次参与互斥锁的争抢。
   ```

3. **通过buffered channel实现线程池**：

   ```go
   package main
   
   import (
   	"fmt"
   	"math/rand"
   	"sync"
   	"time"
   )
   //任务
   type Task struct {
   	id      int
   	randnum int
   }
   //结果
   type Result struct {
   	task   Task
   	result int
   }
   //任务通道、结果通道   容量都是10
   var tasks = make(chan Task, 10)
   var results = make(chan Result, 10)
   
   //处理过程，就是算各个位的和  234  2+3+4=9
   func process(num int) int {
   	sum := 0
   	for num != 0 {
   		digit := num % 10
   		sum += digit
   		num /= 10
   	}
   	time.Sleep(2 * time.Second)
   	return sum
   }
   
   //worker,就是在外层要goroutine的函数，就是协程的主体，它调用了process
   func worker(wg *sync.WaitGroup) {
   	defer wg.Done()
   	for task := range tasks {
   		result := Result{task, process(task.randnum)}
   		results <- result
   	}
   }
   
   //创建线程池
   func createWorkerPool(numOfWorkers int) {
   	var wg sync.WaitGroup
   	for i := 0; i < numOfWorkers; i++ {
   		wg.Add(1)
   		go worker(&wg)
   	}
   	wg.Wait()
   	close(results)
   }
   
   //创建任务并添加到任务队列（通道）
   func allocate(numOfTasks int) {
   	for i := 0; i < numOfTasks; i++ {
   		randnum := rand.Intn(999)
   		task := Task{i, randnum}
   		tasks <- task
   	}
   	close(tasks)  //任务添加完成后，关闭了任务通道，这样就激活了下面getResult中的阻塞在range上的协程，然后向done里面放了信号量 true。然后main函数可以知道此时任务取完了。
   }
   
   //从结果队列（通道）里面取得结果
   func getResult(done chan bool) {
   	for result := range results {
   		fmt.Printf("Task id %d, randnum %d , sum %d\n", result.task.id, result.task.randnum, result.result)
   	}
   	done <- true
   }
   
   //主函数
   func main() {
   	startTime := time.Now()
   	numOfWorkers := 20
   	numOfTasks := 100
   
   	var done = make(chan bool)
   	go getResult(done)
   	go allocate(numOfTasks)
   	go createWorkerPool(numOfWorkers)
   	// 必须在allocate()和getResult()之后创建工作池
   	<-done
   	endTime := time.Now()
   	diff := endTime.Sub(startTime)
   	fmt.Println("total time taken ", diff.Seconds(), "seconds")
   }
   ```

   

4. 

#### 十八、；

#### 十九、；

















































