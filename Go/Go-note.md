# Go 笔记

> 由 `note.go` 整理而来，主要保留原笔记内容，只转换为更适合阅读的 Markdown 结构。

- `package main`：必须写,写出该文件的包名

- `fmt`：导入,导入什么包名
- )

- 区分: Go中以换行作为结束语句,因此不用打";",{}也不能轻易换行

## 函数

- func 表示命名一个函数
- 如fmt.Println 是引用fmt包中的Println函数z

## 变量

- 变量申明 -> var n int = 3
- 批量申明 -> var (
- a string
- b int
- c bool
- d float32
- )
- 类型推导,让编译器根据初始值推导类型
- var name = "hello"
- var num = 1
- 短变量申明,只能在函数内部申明的短周期变量申明
- num := 10
- name := "hello"
- 匿名变量,可以忽略部分无用信息,用"_"表示
- x, _ := 3, "quit" -> 此时 _ = "quit"用来省略该赋值
- _ -> 不会额外开辟空间,也不占用命名空间,因此不会重复定义

## 常量

- 常量申明 -> const pi = 3.1415926
- 批量申明 -> const (
- a = 4
- b = 5
- e = 2.7
- n1 = 10 <- 常量申明若未有明确赋值则,与上行相同(n1 = n2 = n3)
- n2
- n3
- )
- iota 常量计数器,只能在常量表达式中使用,便于对tag索引
- const (
- n1 = iota // 0
- n2        // 1
- _         // 2
- n4        // 3
- //
- n5 = 100
- n6 = iota // 4
- //
- a, b = iota + 1, iota + 2 // 6,7
- c, d                      // 7,8
- ....
- )
- const n7 = iota // 0

## 数据类型

- uint8,uint16,uint32,uint64
- int8,int16,int32,int64
- uint -> uint64
- int -> int64
- uintptr -> 无符号整型,存放指针
- //
- float32,float64
- //
- complex64,complex128 -> 复数(其中位数拆分一半,如complex64 32位分给实数 32位分给虚数)
- //
- bool -> 默认为false,无法参与数值运算,0/1并非指向false,true
- //
- string -> 默认utf-8编码
- s1 :=
- `
- 多行字符串,以``为标头
- 原样输出内部内容
- 包括\n \t....
- `
- s1 := []string{"how","do","you","do"} <- 切片申明形式
- //
- byte,rune 字符
- byte -> uint8  -> 便于存放符号,以及便于计算
- rune -> uint32 -> 一般用来存放字符,中文,宽字符

## 数组

- 定义 var a [3] int
- 只有两个数组类型完全一致才能进行数组间的赋值操作
- a[3]int = b[3]int (√)
- a[4]int = b[3]int (×)
- 定义时也可以不强求写入数组大小如:
- var boolArray = [...]bool {true, false, true} -> 编译器自动检测为3
- 使用索引值方式赋值:
- var langArray = [...]string {1: "Golang", 3: "Python", 7: "Java"}

## 切片(slice) -> 基于数组的封装,属于引用容器需要初始化

- 定义 var name [] <- 不用输入具体的数组大小
- 切片赋值:
- a := [5]int {1,2,3,4,5}
- b := a[1 : 4] <- 此时b为切片,是a中索引[1,4)的子数组
- 切片可以再切片:
- c := b[0 : len(b)]
- make构造切片:
- d := make([]int , 5, 10) -> 此时创造了个容量为10,长度为5,的int切片d
- 若一个切片仅申明未初始化,此时该切片并未开辟空间,便可通过nil来进行判断
- var a[] int
- if a == nil {
- fmt.Println("a未开辟空间")
- }
- 切片赋值/拷贝(类似引用)
- b := a <- 此时a,b共用底层数组
- append()方法       -> 为切片添加元素
- append(a, 10,2,3,4) -> 对切片a添加'10','2','3','4'数据
- append(a, b...)     -> 对a增加b的切片内容
- copy()方法         -> 复制切片
- copy(a, b)         -> 将b切片复制到a切片
- 切片删除
- a := []string {"北京","上海","广州","深圳"}
- a = append(a[0 : 2], a[3: ]...) -> 实现了删去索引2,"广州"的效果

## map 属于引用容器需要初始化

- 定义 map[KeyType]ValueType
- 声明 b := map[int]bool {
- 1: true,
- 2: false,
- }
- 添加键值对:
- a["grade"] = 100
- 判断键值对是否存在:
- value, ok := Map["grade"] -> 返回存在结果给ok,返回值给value,若未存在value为数据默认值
- for遍历与添加顺序无关
- delete() 方法:
- delete(a, "grade") -> 删除a中的grade键值对

## 函数

- 定义 func 函数名(参数)(返回值变量名/返回值类型) { }
- go中支持可变参数输入,输入形式以切片呈现:
- func intSum(n ...int) (res int) {
- res := 0
- for _, value := range n {
- res += value
- }
- return
- } -> 固定参数和可变参数同时出现时,可变参数要放在最后
- go中支持多个返回值的函数,返回以切片形式呈现
- func calc(a, b int) (sum, sub int) {
- sum = a + b
- sub = a - b
- return
- }
- defer语句 <- 将该行行为在函数即将return前执行
- 执行顺序依照栈结构(最先defer的语句最后执行)
- func main() {
- fmt.Println(0)
- defer fmt.Println(1)
- defer fmt.Println(2)
- defer fmt.Println(3)
- defer fmt.Println(4)
- } -> 最后输出 0 4 3 2 1
- 函数参数输入 <- 可以将一个函数以参数形式输入
- func add(x, y int) int {
- return x + y
- }
- //
- func calc(x, y int , op func(int, int) int) int {
- return op(x, y)
- }
- //
- func main() {
- calc(100, 200, add) -> 以实现calc自定义算式方式
- }
- 匿名函数
- func() {
- fmt.Println("匿名函数")
- }() -> 加上 () 以表示直接运行
- //
- a := func() {
- fmt.Println("匿名函数")
- }
- a()  -> 直接把匿名函数赋值给a通过a调用
- 闭包 -> 函数 + 外层变量的引用
- 函数可以返回一个函数:
- func a() func() {
- name := "you"
- return func() {
- fmt.Println("hello", name)
- }
- }
- //
- func main() {
- r := a() -> r接收a中的匿名函数
- r()      -> r执行a中的匿名函数
- }

- 此时r作为匿名函数却能调用a中的name变量,即为闭包概念

## 指针

- new
- new(int) -> 返回int类型指针
- make
- make(容器定义,当前容器元素个数,容器容量) -> 对容器创建指针,返回为容器本身slice,map,channel

## 自定义类型

- type name int -> 定义一个name类型属性为int
- 类型别名:
- type NewInt = int -> NewInt为int的别名

## 结构体 -> 当前字段若为大写则为公有,包外可见,若为小写则为私有

- 键值对初始化:
- p1 := person {
- name: "you",
- age: 18,
- }
- 列表初始化:
- p2 := person { <- 严格依照定义顺序填写
- "you",
- 18,
- }
- 构造:
- 匿名构造:
- type Person struct {
- string,
- int,
- Address, -> 匿名构造嵌套可以直接调用该结构体内部属性
- }            -> 调用方法和显示构造相同(直接用数据类型名调用)
- 嵌套:
- type Address struct {
- Province string,
- City string,
- }
- //
- type Person struct {
- Name string,
- Gender string,
- Age int,
- Address address,
- }
- 标签Tag:
- type class struct {
- Title string 'json: "title" db: "title_list" xml: "tt"' <- 严格按照此格式 'key: "value"'
- Students []student
- }

## Json序列化(使用json包进行跨数据传输)

- data, err := json.Marshal(c1)  -> c1结构体序列化为json格式字符串
- err := json.Unmarshal(c1, &c2) -> c1字符串反序列为go数据,赋值地址给c2

## 方法/接收者 <- 为特定接收者(数据类型)写入方法(类似 类 中写入行为函数)

- Method定义:
- func (接收者变量 接收者类型) 方法名(参数列表) (返回参数) {
- 函数体
- }
- 接收者:
- var p1 Person
- 值接收者:
- func (p Person) SetAge1(newAge int8) {
- p.age = newAge
- } -> 返回时不会对传入数据p1进行数值修改
- //
- 指针接收者:
- func (p *Person) SetAge2(newAge int8) {
- p.age = newAge
- } -> 返回时会对原传入数据p1进行数值修改

- 只有本地包中定义的类型才能对此定义方法,非本地类型不可修改

## 语块

- if a := 5 ; a > 5 {
- //
- } else if a == 5 {
- //
- } else a < 5 {
- //
- }
- //
- for a := 0 ; a < n; a++ {}
- for idx, r := range s1 {} <- 遍历s1并将子串赋值给r (此时r为rune), 索引值赋值给idx
- for {} <- 无限死循环

## 包

- 导入:
- improt (
- myPrt "fmt" -> 此时标准fmt包可通过别名myPrt调用
- "自定义包"
- )
- init函数:
- 当包被导入时自动执行,无参数也无返回值,可自定义函数体
- 基础语句
- fmt.Println(a,b,c,d,e)  -> 控制台输出
- fmt.Printf("%d\n", n)   -> C系输出
- fmt.Scanf(" %d", &n)    -> C系输入
- fmt.Sprintf("%s - %s", s1, s2)
- //
- os.Exit(0) -> 控制台退出
- //
- math.MaxFloat32 -> 代表浮点32的最大值
- math.MaxFloat64 -> 代表浮点64的最大值
- rand.Intn(100)  -> 0-99的随机数生成
- //
- strings.Contains(s1, " ")  -> 判断字符串是否包含指定内容
- strings.Split(s1, " ")     -> 将字符串按照指定内容分割为切片
- "how do you do" -> [how do you do]
- strings.Join(s1, " ")      -> 将切片以指定内容连接
- [how do you do] -> "how do you do"
- strings.HasPrefix(s1, " ") -> 判断字符串是不是以指定内容为开头
- strings.HasSuffix(s1, " ") -> 判断字符串是不是以指定内容为结尾
- strings.Index(s1, " ")     -> 给予字串第一次出现的位置
- strings.LastIndex(s1, " ") -> 最后子串出现的位置的索引
- //
- sort.Ints(a) -> 对a的整数切片进行排序
- sort.Strings(a) -> 对a的字符串切片进行排序
- //
- now := time.Now() -> 返回公历当前时间的时间对象
- now.Year()/now.Month()/now.Day()/now.Hour()/now.Minute()/now.Second() -> 各返回具体字符串
- now.Unix()   -> 返回距离时间戳的秒数 (1970.01.01 ~ current)
- now.UnixNano -> 返回当前距离时间戳的纳秒数
- now.Add(time.Hour) -> 对当前时间对象增加1h时间
- now.Sub(time.Hour) -> 对当前时间对象减少1h时间
- now.Equal(t)  -> 判断两时间对象的时间是否一致
- now.Before(t) -> 判断now是否在t之前
- now.After(t)  -> 判断now是否在t之后
- now.Format("2006-01-02 15:04:05") -> 将当前时间按指定格式返回 @ 格式化内部时间固定必须以2006 01 02 15 04 05这几个数进行格式设置 @
- time.Unix(sec:1565150154) -> 将输入sec秒数按照时间戳延后计算日期,并返回字符串
- time.Duration(t)          -> 将int t 转换为,时间间隔类型,便于后续时间方面计算
- time.Sleep(time.Duration(t) * time.Second)     -> 程序停止t秒
- time.Tick(time.Duration(t) * time.Second)      -> 时间计时器,计时t秒
- loc, err := time.LoadLocation("Asia/Shanghai") -> 拿到一个时区
- timeObj, err := time.ParseInLocation("2006/01/02 15:04:05", s, loc) -> 对于给定时间字符串s,以自定格式,解析loc地区当地时间并返回时间对象
- //
- fileObj, err := os.Open("文件路径") -> 打开文件只读
- {
- fileObj.Close()                -> 关闭文件
- n, err := fileObj.Read([]byte) -> 读取文件,并将读取字节数返回给n,错误信息返回给err,读取内容返回给字节切片容器
- { 特别的
- if err == io.EOF {
- fmt.Println(string(temp[:n]))
- return
- }
- } 对读取到EOF之前的内容进行输出
- //
- reader :=bufio.NewReader(fileObj)
- line, err := reader.ReadString('\n') -> 指定以 什么 为结尾符读取
- }
- content, err := ioutil.ReadFile("文件路径")            -> 返回读取文件内容
- err := ioutil.WriteFile("文件路径", []byte(str), perm) -> 将文本写入文件
- fileObj, err := os.OpenFile("文件路径", flag int, perm FileMode)
- {
- flag 为打开模式:
- os.O_WRONLY 只写
- os.O_CREATE 创建文件
- os.O_RDONLY 只读
- os.O_RDWR   读写
- os.O_TRUNC  打开文件时清空
- os.O_APPEND 追加
- perm 文件权限(一个八进制数):
- 04 r 读
- 02 w 写
- 01 x 执行
- fileObj.Write([]byte(str)) -> 对文件写入操作
- fileObj.WriteString("string")
- //
- writer := bufio.NewWriter(fileObj)
- writer.WriteString("string") -> 将内容写入缓冲区
- writer.Flush()               -> 将缓冲区的内容写入磁盘
- }
- //
- len()       -> 返回字节数
- cap()       -> 返回容器容量
- close()     -> 关闭channel
- new()       -> 用来分配内存
- panic(内容) -> 终止程序输出指定内容
- recover()   -> 恢复panic状态,继续执行程序,一般与defer共用

## 反射

- t := reflect.TypeOf(x)  -> 对x的数据类型进行判断,并且返回一个类型给t
- t.Name()            -> 返回一个数据的名字
- t.Kind()            -> 返回一个数据的种类
- v := reflect.ValueOf(x) -> 对x的数据值进行判断
- t.kind()            -> 拿到值对应类型的种类
- res := float32(v.Float()) -> 把一个值转换成一个float32类型的变量
- v.SetInt(100) -> 将v的值设为100
- v.IsNil()     -> 判断容器是否为空,指针是否有效
- v.IsValid()   -> 判断数据是否有效(是否为初始零值)
- //
- 若v为传入指针 v.Elem().SetInt(100) -> 将v通过.Elem解引
- @ 格式化输出该数据一般用%v @
- 结构体反射
- 遍历结构体所有字段:
- t := reflect.TypeOf(student)
- for i := 0; i < t.NumField(); i++{
- fieldObj := t.Field(i)
- }      ↑
- 这个类型可以获取很多信息: .Name .Tag .Type
- 遍历结构体所有方法:
- v := reflect.ValueOf(student)
- for i := 0; i < v.NumMethod(); i++ {
- fieldObj := v.Method(i)
- ↑
- 这个类型可以获取很多信息: .Name .Tag .Type
- var args = []reflect.Value {}
- v.Method(i).Call(args) -> 调用方法
- }
- 根据成员名字获取字段:
- field, ok := t.FieldByName("Score")  -> 查找结构体中的成员名是否存在
- field, ok := t.MethodByName("Score") -> 查找结构体中的成员名是否存在

## 接口(interface)      -> 无论传入是什么类型,只管能实现的方法

- type sayer interface { ->  一个类型中,只要含有say()方法,都归并为sayer类型
- say()
- }
- 接口类型变量也可以承接实现该接口的变量:
- var s sayer
- d := dog{}
- s = d
- 嵌套:
- type animal interface {
- mover,
- sayer, -> 两个都为接口收纳在animal接口下
- }
- 类型断言:
- res, ok := x.(string) -> 对x接口的数据类型进行判断,判断是否为string
- //
- switch t := x.(type) {
- case string:
- fmt.Println("是字符串")
- case int:
- fmt.Println("是int类型")
- default:
- fmt.Println("未知类型")
- }

## 并发 -> 同一时间段内执行多个任务

- go: 开线程
- func hello() {
- fmt.Println("hello you!")
- }
- //
- func main() {  -> 开启了一个主goroutine去执行main
- go hello() -> 开启了一个goroutine去执行hello函数
- fmt.Println("hello main")
- } -> 输出顺序不固定,因为多线程下触发顺序没设置

- 当主routine结束程序时,由主routine创建的子线程会一并结束

- var wg sync.WaitGroup
- wg.Add(1)             -> 计数器+1 表明线程计数+1
- wg.Down()             -> 计数器-1
- wg.Wait()             -> 等所有子线程结束后才结束(计数器为1)
- runtime.GOMAXPROCS(1) -> 只占用一个cpu核心进行线程操作(即:只有一个线程工作)
- //
- channel: 建立通道实现线程数据独立传输,结构类似队列,属于引用类型
- 声明:
- var ch chan int         // 一个传递整形的通道ch
- ch := make(chan int)    // 无缓冲区通道,同步通道
- ch := make(chan int, 1) // 带缓冲区通道,异步通道
- ch <- 10   // 把10发送给ch
- x := <- ch // 从ch中接收并赋值给x
- <- ch      // 从ch中接收值,忽略结果
- close(ch)  // 关闭channel通道
- channel间数据传输:
- func f1(ch chan<- int){ -> 单向通道,表明该通道只能发送
- for i := 0; i < 100; i++ {
- ch <- i
- }
- close(ch)
- }
- //
- func f2(ch1 <-chan int, ch2 chan int) { -> 表明ch1通道只能取值
- for {
- tmp, ok := <- ch1
- if !ok {
- break
- }
- ch2 <- tmp * tmp
- }
- close(ch2)
- }
- //
- func main() {
- ch1 := make(chan int, 100)
- ch2 := make(chan int, 200)
- //
- go f1(ch1)
- go f2(ch2)
- //
- for res := range ch2 {
- fmt.Println(res)
- }
- } -> 此时ch1入通道,ch2出通道,main通道输出,实现数据间的传输
- work pool 工作池: 创建有限 | < 任务数的线程 来完成多任务 避免channel使用过多
- select 语句使用: -> 跟switch格式类似,当有多个条件符合时,会在符合的条件内随机执行一条
- ch := make(chan int, 1)
- for i := 0; i < 10; i++ {
- select {
- case x := <-ch:
- fmt.Println(x)
- case <-i:
- default:
- fmt.Println("无效行为")
- }
- }
- //
- 锁: -> 避免不同线程同时占用一个内存导致的异常(锁的区域内,只要线程进入该区域便可执行,锁把部分线程分离在可执行区域之外)
- lock sync.Mutex       -> 互斥锁
- lock.Lock()      -> 加锁,锁的情况下其他线程会等待解锁再进行下一动作,唤醒策略为随机
- lock.Unlock()    -> 解锁
- rwLock sync.RWMutex   -> 读写锁
- reLock.RLock()   -> 对读上锁(此时只可进入新的读锁,不可进入写锁)
- reLock.RUnlock() -> 对读解锁
- reLock.Lock()    -> 读写上锁
- reLock.Unlock()  -> 读写解锁
- loadOnce sync.Once    -> 只让某一语句执行一次(可用于初始化,关闭channel)
- loadOnce.Do(loadIcons)
- var map  = sync.Map{} -> 并发安全的内置map类型
- map.Store(i, 2)  -> 设置键值对[i, 2]
- map.Load(i)      -> 查找键i并返回value,ok

## socket编程

- TCP/IP 协议 -连接协议
- listen, err := net.Listen("tcp", "127.0.0.1:20000") -> 建立TCP监听,用某一协议监听某一端口
- conn, err := listen.Accept()                        -> 接听,与客户端来建立连接
- conn.Close()                                        -> 关闭通信连接
- Nagle算法:当提交数据给tcp时tcp会等待确认是否还需传输,随后一同传输
- 粘包: 若是数据没被及时传输或是及时提取会使得数据滞留,形成粘包
- solve: 使用封包/拆包的操作
- 封包: 将数据包分为包头与包体,包头存储数据包的长度
- 拆包: 根据包头提供的信息,拆分出完整的数据包,对数据包传/收合理调控避免粘包
- UDP 协议 -无连接协议
- listen, err := net.ListenUDP("udp", &net.UDPAddr { -> 建立UDP监听
- IP: net.IPv4(0,0,0,0),
- Port: 30000,
- Zone: "",
- })
- listen.Close()                              -> 关闭监听
- n, addr, err := listen.ReadFromUDP(buf[:])  -> 接收信息赋给buf,接收发送方地址给addr
- n, err := listen.WriteToUDP(buf[:n], addr)  -> 发送数据给addr地址处
- c, err := net.DialUDP("udp", &net.UDPAddr { -> 建立UDP拨号构建连接
- IP: net.IPv4 (127.0.0.1),
- Port: 30000,
- Zone: "",
- })

## 单元测试

- 测试文件命名格式: name_test.go (包文件_test.go格式)
- 区分test函数名:
- {
- 前缀为Test      -> 测试程序一些逻辑行为是否正常
- 前缀为Benchmark -> 测试函数性能
- 前缀为Example   -> 为文档提供示例文档
- }
- 写入test函数:
- func TestName(t *testing.T) { -> 函数名必须以Test开头
- 测试内容
- } -> 功能测试
- func BenchmarkName(b *testing.B) {
- for i := 0; i < b.N; i++ {
- 测试内容       ↑
- }                 动态变化数保证该测试跑够 1次 & 1s 用来测试性能
- } -> 性能测试
- 运行test文件:
- 命令行框输入 go test 即可调用当前根文件内的所有test测试
- go test -bench = Split 在性能测试下测试Split包
- 子测试:
- 如: "simple": test {input: "a:b:c", sep: ":", wnat: []string{"a", "b", "c"}},
- "chinese": test {input: "你好不好", sep: "好", want:[]string{"你", "不"}},
- 使用 go test -run = Split/chinese -> 即可单个测试Split切片下chinese测试
- 覆盖率:
- go test -cover 会返回当前test运行在整个test文件下的代码覆盖率,也可理解为测试数(一般60%为正常)
