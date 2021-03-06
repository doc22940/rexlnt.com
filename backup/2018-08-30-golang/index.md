---
title: Golang
description: Basic and Concurrency
date: 2018-11-28T02:41:46.000Z
---

![](https://user-images.githubusercontent.com/9530963/65758532-d262d500-e14b-11e9-9ff1-55b10a6c9e24.png)

## Syntax

specification：[https://golang.org/ref/spec](https://golang.org/ref/spec)

```go
package main

import "fmt"

func main() {
    // 常用命令：
    // 命令行中查看函数签名：go doc fmt.Println
    // 测试 Go 程序： go run main.go

    // Go 中变量类型是不可更改的

    // 整数类型
    // uint8 : unsigned  8-bit integers (0 to 255)
    // uint16 : unsigned 16-bit integers (0 to 65535)
    // uint32 : unsigned 32-bit integers (0 to 4294967295)
    // uint64 : unsigned 64-bit integers (0 to 18446744073709551615)
    // int8 : signed  8-bit integers (-128 to 127)
    // int16 : signed 16-bit integers (-32768 to 32767)
    // int32 : signed 32-bit integers (-2147483648 to 2147483647)
    // int64 : signed 64-bit integers (-9223372036854775808 to 9223372036854775807)
    const age int64 = 40

    // 浮点类型: float32, float64
    const pi float64 = 3.14159265359

    // 布尔类型
    // := 变量声明、类型定义、变量赋值的简写形式
    isOver40 := true

    // 字符串包裹在引号或反引号中
    str := "Hello Go!"
    fmt.Println(str)

    // 单引号声明 rune 类型，使用 UTF-8 编码，是 int32 的别名
    // g := 'Σ'

    // fmt.Print(): 输出
    // fmt.Println(): 换行输出
    // fmt.Printf(): 格式化输出
    // Printf is used for format printing (%f is for floats)
    fmt.Printf("%f \n", pi)
    // You can also define the decimal precision of a float
    fmt.Printf("%.3f \n", pi)
    // %T prints the data type
    fmt.Printf("%T \n", pi)
    // %t prints booleans
    fmt.Printf("%t \n", isOver40)
    // %d is used for integers
    fmt.Printf("%d \n", 100)
    // %b prints in binary
    fmt.Printf("%b \n", 100)
    // %c prints the character associated with a keycode
    fmt.Printf("%c \n", 44)
    // %x prints in hexcode
    fmt.Printf("%x \n", 17)
    // %e prints in scientific notation
    fmt.Printf("%e \n", pi)

    // for 循环
    for age <= 1 {
        fmt.Println(age)
    }

    // if, else if , else
    if age > 1 {
        fmt.Println("age > 1")
    } else if age < 1 {
        fmt.Println("age < 1")
    } else {
        fmt.Println("something goes wrong")
    }

    // switch
    switch isOver40 {
    case true:
        fmt.Println("is over 40")
    case false:
        fmt.Println("is not over 40")
    default:
        fmt.Println("something goes wrong")
    }
}
```

#### Array / Slice

```go
package main

import "fmt"

func main() {
    // 数组具有固定的长度，且元素类型相同
    var nums[5] float64

    nums[0] = 163
    nums[1] = 78557
    nums[2] = 691
    nums[3] = 3.141
    nums[4] = 1.618
    // 通过索引使用数组元素
    fmt.Println(nums[3])

    // 另一种数组初始化方式
    anotherNums := [5]float64 { 1, 2, 3, 4, 5 }

    // 遍历数组，使用 _ 作为占位符
    for i, value := range anotherNums {
        fmt.Println(value, i)
    }

    // 分片和数组不同的地方在于它不需要指定长度
    slice := []int {5,4,3,2,1}
    // 从分片中创建新的分片
    fmt.Println("slice[3:5] =", slice[3:5])
    fmt.Println("slice[:2] =", slice[:2])
    fmt.Println("slice[2:] =", slice[2:])

    // 创建一个空分片，三个参数分别用于指定类型、长度和容量
    slice2 := make([]int, 5, 10)

    // 复制分片
    copy(slice2, slice)
    fmt.Println(slice2[0])

    // 分片追加数据
    slice2 = append(slice2, 0, -1)
    fmt.Println(slice2[6])
}
```

#### Map

```go
package main

import "fmt"

func main() {
    // 创建 Map
    // make(map[keyType] valueType)
    m := make(map[string]int)

    // 增加 key
    m["age"] = 42
    // 查询 key 数量
    fmt.Println(len(m))
    // 删除 key
    delete(m, "age")

    // 多层嵌套 Map
    superhero := map[string]map[string]string{
        "Superman": map[string]string{
            "realname": "Clark Kent",
            "city":     "Metropolis",
        },
        "Batman": map[string]string{
            "realname": "Bruce Wayne",
            "city":     "Gotham City",
        },
    }

    if hero, isExist := superhero["Superman"]; isExist {
        fmt.Println(isExist)
        fmt.Println(hero["realname"], hero["city"])
    }
}
```

#### Function

```go
package main

import "fmt"

func main() {
    // 函数可以返回多个值
    num1, num2 := next2Values(5)
    fmt.Println(num1, num2)

    // 不定参数函数
    fmt.Println(subtractThem(1, 2, 3, 4, 5))

    // 匿名函数
    doubleNum := func() int {
        return num1 * 2
    }
    fmt.Println(doubleNum())

    // 递归函数
    fmt.Println(factorial(3))

    // defer 常用于在函数执行结束时关闭文件、通道等连接
    // 同一个函数中多个 defer，后定义的先执行
    // 在函数中 return 先给返回值赋值，然后执行 defer 函数，最后 RET 指令携带返回值退出函数
    // defer file.close();

    // 使用 recover() 捕获错误
    // 3 / 0 触发错误，并没有显式执行 panic
    fmt.Println(safeDiv(3, 0))
    // 显式执行 panic
    demPanic()
}

func next2Values(number int) (int, int) {
    return number + 1, number + 2
}

func subtractThem(args ...int) int {
    finalValue := 0

    for _, value := range args {
        finalValue -= value
    }

    return finalValue
}

func factorial(num int) int {
    if num == 0 {
        return 1
    }
    return num * factorial(num-1)
}

// 通过 recover() 捕获错误信息，并允许后续的程序继续执行
func safeDiv(num1, num2 int) int {
    defer func() {
        fmt.Println(recover())
    }()
    solution := num1 / num2
    return solution
}

// 显式触发 panic，然后由 cover() 捕获错误信息
func demPanic() {
    defer func() {
        // If I didn't print the message nothing would show
        fmt.Println(recover())
    }()
    panic("PANIC")
}
```

#### Pointer

```go
package main

import "fmt"

func main() {
    // 当 & 和 * 加在变量之前时，& 表示址运算，* 表示值运算
    // 当 * 加在类型前面时，表示某种类型的指针

    x := 0
    // 通过向函数传递指针，实现对数据的修改
    changeX(&x)
    // 通过 & 符号获取变量的内存地址
    fmt.Println("Memory Address for x =", &x)

    // 通过 new 函数创建一个指针并返回内存地址
    y := new(int)
    // 通过 * 创建指针，此时指针是一个空指针，默认值为 nil
    // var y *int

    changeY(y)
    fmt.Println("y =", *y)
}

// * 表示函数接收的参数是一个指针
func changeX(x *int) {
    *x = 2
}

func changeY(y *int) {
    *y = 100
}
```

#### Struct / Interface

```go
package main

import "fmt"

// Shape ...
type Shape interface {
    area() float64
}

// Rectangle ...
type Rectangle struct {
    leftX  float64
    TopY   float64
    height float64
    width  float64
}

func main() {
    // 声明和赋值
    rect := Rectangle{leftX: 0, TopY: 50, height: 10, width: 10}
    // 简写形式
    // rect := Rectangle{0, 50, 10, 10}

    // 调用属性
    fmt.Println("Rectangle is", rect.width, "wide")
    // 调用函数
    fmt.Println("Area of the rectangle =", rect.area())

    // Golang 中 Interface 的作用是实现功能的组合（组合优于继承的偏好）
    // 如果某个数据类型满足了 Interface 定义的一组函数签名，则认为该函数类型实现了这一 Interface
    fmt.Println("Area of the rectangle =", getArea(&rect))
}

// (rect Rectangle) 是函数接收器
func (rect *Rectangle) area() float64 {
    return rect.width * rect.height
}

func getArea(shape Shape) float64 {
    return shape.area()
}
```

#### Strings, Input, Convert

```go
package main

import (
    "fmt"
    "strconv"
    "strings"
)

func main() {
    // 如果包含特定子串，则返回 true，否则返回 false
    fmt.Println(strings.Contains("Hello GO!", "lo"))

    // 交互式操作
    name := ""
    fmt.Println("What is your name? ")
    fmt.Scan(&name)
    fmt.Println("Hello", name)

    // CASTING

    randInt := 5
    randFloat := 10.5
    randString := "100"
    randString2 := "250.5"

    // 整数转浮点数
    fmt.Println(float64(randInt))
    // 浮点数转整数
    fmt.Println(int(randFloat))
    // 字符串转整数
    newInt, _ := strconv.ParseInt(randString, 0, 64)
    fmt.Println(newInt)
    // 字符串转浮点数
    newFloat, _ := strconv.ParseFloat(randString2, 64)
    fmt.Println(newFloat)
}
```

#### HTTP Server

```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello World\n")
}

func handler2(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello Earth\n")
}

func main() {
    http.HandleFunc("/", handler)
    http.HandleFunc("/earth", handler2)
    // 监听 8080 端口
    http.ListenAndServe(":8080", nil)
}
```

#### Go Routine / Channel

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    // 创建 channel: make(chan valueType, buffer)
    messages := make(chan string, 2)

    // 使用 go 关键字定义 go routine
    // go routine 是并行执行的函数
    go func() {
        messages <- "ping one"
        messages <- "ping two"
    }()

    // 通道消息的发送和接收操作默认都是阻塞的
    fmt.Println(<-messages)
    fmt.Println(<-messages)

    // 通过 channel 在 go routine 之间同步状态或数据
    isDone := make(chan bool)
    go worker(isDone)
    <-isDone
}

// 通过指定 channel 是否只用来发送或接收值，可以提升程序的类型安全
// worker 函数定义了一个只接收值的 channel
// chan<- typeValue: 表示通道接收值
// <-chan typeValue: 表示通道发送值
func worker(done chan<- bool) {
    fmt.Println("working...")
    time.Sleep(time.Millisecond * 100)
    fmt.Println("done!")
    done <- true
}
```

#### Select

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    c := make(chan string)

    go func() {
        time.Sleep(time.Second * 2)
        c <- "done"
    }()

    // Select 通道选择器可以同时等待多个通道操作
    select {
    case msg := <-c:
        fmt.Println("received", msg)
    // 超时处理：如果通道未在 1 秒内返回数据，则执行 timeout 操作
    case <-time.After(time.Second * 1):
        fmt.Println("timeout...")
        // 关闭通道
        close(c)
    }
}
```

## Use Cases

#### Timer / Ticker

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    go func() {
        // 200ms 之后触发
        <-time.NewTimer(time.Millisecond * 200).C
        fmt.Println("Timer 2 expired")
    }()

    go func() {
        // 每 100ms 触发一次
        for t := range time.NewTicker(time.Millisecond * 100).C {
            fmt.Println("Tick at", t)
        }
    }()

    time.Sleep(time.Second * 2)
}
```

#### Worker / Rate Limiting

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)

    // 批量启动工作池
    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }

    // 批量发送任务
    for j := 1; j <= 9; j++ {
        jobs <- j
    }
    close(jobs)

    // 批量处理结果
    for a := 1; a <= 9; a++ {
        <-results
    }
}

// 工作池，初始时阻塞
func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        // 通过限制执行速率，控制资源的利用率和工程质量
        <-time.Tick(time.Millisecond * 600)
        fmt.Println("worker", id, "processing job", j)
        results <- j * 2
    }
}
```

#### Atomic / Mutex

```go
package main

import (
    "fmt"
    "math/rand"
    "runtime"
    "sync"
    "sync/atomic"
    "time"
)

func main() {
    var state = make(map[int]int)
    // 互斥锁
    var mutex = &sync.Mutex{}
    // 记录操作次数
    var ops int64

    // 启动 100 个 go routine 读数据
    for r := 0; r < 100; r++ {
        go func() {
            total := 0
            for {
                key := rand.Intn(5)
                mutex.Lock()
                total += state[key]
                mutex.Unlock()
                // 原子级增加操作
                atomic.AddInt64(&ops, 1)
                runtime.Gosched()
            }
        }()
    }

    // 启动 10 个 go routine 写数据
    for w := 0; w < 10; w++ {
        go func() {
            for {
                key := rand.Intn(5)
                val := rand.Intn(100)
                mutex.Lock()
                state[key] = val
                mutex.Unlock()
                atomic.AddInt64(&ops, 1)
                // 由 runtime 调度 go routine 的执行
                runtime.Gosched()
            }
        }()
    }

    time.Sleep(time.Second)

    opsFinal := atomic.LoadInt64(&ops)
    fmt.Println("ops:", opsFinal)

    mutex.Lock()
    fmt.Println("state:", state)
    mutex.Unlock()
}
```

#### Stateful Goroutine

```go
package main

import (
    "fmt"
    "math/rand"
    "sync/atomic"
    "time"
)

type readOp struct {
    key  int
    resp chan int
}
type writeOp struct {
    key  int
    val  int
    resp chan bool
}

func main() {
    // 统计读操作次数
    var readOps uint64
    // 统计写操作次数
    var writeOps uint64

    reads := make(chan *readOp)
    writes := make(chan *writeOp)

    // 所有 state 保存在单独的 go routine 中
    // 其他 go routine 通过通信的方式来读写数据
    go func() {
        var state = make(map[int]int)
        for {
            select {
            case read := <-reads:
                read.resp <- state[read.key]
            case write := <-writes:
                state[write.key] = write.val
                write.resp <- true
            }
        }
    }()

    // 启动 100 个 go rountine 执行读操作
    for r := 0; r < 100; r++ {
        go func() {
            for {
                read := &readOp{
                    key:  rand.Intn(5),
                    resp: make(chan int)}
                reads <- read
                <-read.resp
                atomic.AddUint64(&readOps, 1)
                time.Sleep(time.Millisecond)
            }
        }()
    }

    // 启动 100 个 go rountine 执行写操作
    for w := 0; w < 100; w++ {
        go func() {
            for {
                write := &writeOp{
                    key:  rand.Intn(5),
                    val:  rand.Intn(100),
                    resp: make(chan bool)}
                writes <- write
                <-write.resp
                atomic.AddUint64(&writeOps, 1)
                time.Sleep(time.Millisecond)
            }
        }()
    }

    time.Sleep(time.Second)

    fmt.Println("readOps:", atomic.LoadUint64(&readOps))
    fmt.Println("writeOps:", atomic.LoadUint64(&writeOps))
}
```

#### Sort by function

```go
package main

import (
    "fmt"
    "sort"
)

// ByLength ...
type ByLength []string

func (s ByLength) Len() int {
    return len(s)
}
func (s ByLength) Swap(i, j int) {
    s[i], s[j] = s[j], s[i]
}
func (s ByLength) Less(i, j int) bool {
    return len(s[i]) < len(s[j])
}

func main() {
    fruits := ByLength{"peach", "banana", "kiwi"}
    sort.Sort(fruits)
    fmt.Println(fruits)
}
```

#### RegExp

```go
package main

import (
    "bytes"
    "fmt"
    "regexp"
)

func main() {
    // 检查字符串是否匹配正则表达式
    match, _ := regexp.MatchString("p([a-z]+)ch", "peach")
    fmt.Println(match)

    // 通过 Compile 构造一个正则表达式 Struct
    r, _ := regexp.Compile("p([a-z]+)ch")

    // 这个结构体自带多个辅助函数
    // 是否匹配字符串
    fmt.Println(r.MatchString("peach"))
    // 从字符串中返回匹配的子串
    fmt.Println(r.FindString("peach punch"))
    // 查找第一个匹配的子串，并返回该子串的起始位置
    fmt.Println(r.FindStringIndex("peach punch"))
    // 返回完全匹配和分组匹配的子串
    // 下面代码会返回匹配 `p([a-z]+)ch` 和 `([a-z]+)` 的子串
    fmt.Println(r.FindStringSubmatch("peach punch"))
    // 返回完全匹配和分组匹配的子串起始位置
    fmt.Println(r.FindStringSubmatchIndex("peach punch"))

    // 带 All 的函数表示全局查找，返回所有的匹配项
    // 查找所有的匹配子串，第二个参数用于限制匹配次数
    fmt.Println(r.FindAllString("peach punch pinch", -1))

    // 去掉函数名中 String，比如 MatchString => Math
    // 可以用于正则检索 []byte
    fmt.Println(r.Match([]byte("peach")))

    // MustCompile 与 Compile 相似，如果解析出现错误，会直接 panic
    r = regexp.MustCompile("p([a-z]+)ch")
    fmt.Println(r)
}
```

#### JSON

```go
package main

import (
    "encoding/json"
    "fmt"
    "os"
)

// J1 ...
type J1 struct {
    Page   int
    Fruits []string
}

// J2 ...
type J2 struct {
    Page   int      `json:"page"`
    Fruits []string `json:"fruits"`
}

func main() {
    // 基础类型转换为 JSON 字符串
    // 布尔型
    bolB, _ := json.Marshal(true)
    fmt.Println(string(bolB))

    // 整形
    intB, _ := json.Marshal(1)
    fmt.Println(string(intB))

    // 浮点型
    fltB, _ := json.Marshal(2.34)
    fmt.Println(string(fltB))

    // 字符串
    strB, _ := json.Marshal("gopher")
    fmt.Println(string(strB))

    // 字符串数组
    slcD := []string{"apple", "peach", "pear"}
    slcB, _ := json.Marshal(slcD)
    fmt.Println(string(slcB))

    // Map
    mapD := map[string]int{"apple": 5, "lettuce": 7}
    mapB, _ := json.Marshal(mapD)
    fmt.Println(string(mapB))

    // Struct 类型
    res1D := &J1{
        Page:   1,
        Fruits: []string{"apple", "peach", "pear"}}
    res1B, _ := json.Marshal(res1D)
    fmt.Println(string(res1B))

    // 通过给 Struct 声明标签，自定义编码的 JSON key 信息
    res2D := J2{
        Page:   1,
        Fruits: []string{"apple", "peach", "pear"}}
    res2B, _ := json.Marshal(res2D)
    fmt.Println(string(res2B))

    // 解码
    // 数据
    byt := []byte(`{"num":6.13,"strs":["a","b"]}`)
    // 构建一个存放解码数据的变量
    var dat map[string]interface{}
    // 解码
    if err := json.Unmarshal(byt, &dat); err != nil {
        panic(err)
    }
    fmt.Println(dat)

    // 对解码后 Map 中的值进行类型转换
    num := dat["num"].(float64)
    fmt.Println(num)

    // 对嵌套数据进行类型转换
    strs := dat["strs"].([]interface{})
    str1 := strs[0].(string)
    fmt.Println(str1)

    // 根据自定义 Struct 进行解码
    // 可以增强类型安全，消除访问数据时的类型断言
    str := `{"page": 1, "fruits": ["apple", "peach"]}`
    res := &J2{}
    json.Unmarshal([]byte(str), &res)
    fmt.Println(res)
    fmt.Println(res.Fruits[0])

    // 重定向 JSON 编码后的输出，比如替换为 `os.Stdout` 或直接作为 HTTP 响应体
    enc := json.NewEncoder(os.Stdout)
    d := map[string]int{"apple": 5, "lettuce": 7}
    enc.Encode(d)
}
```

#### Time

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    // 当前时间
    now := time.Now()
    fmt.Println(now)
    // 秒: now.Unix()
    // 纳秒: now.UnixNano()
    // 构造一个时间
    then := time.Date(2009, 11, 17, 20, 34, 58, 651387237, time.UTC)
    fmt.Println(then)
    // 输出年、月、日、时、分、秒、微妙、时区
    fmt.Println(then.Year())
    fmt.Println(then.Month())
    fmt.Println(then.Day())
    fmt.Println(then.Hour())
    fmt.Println(then.Minute())
    fmt.Println(then.Second())
    fmt.Println(then.Nanosecond())
    fmt.Println(then.Location())
    // 输出周一到周日的 Weekday 信息
    fmt.Println(then.Weekday())
    // 判断时间的先后顺序
    fmt.Println(then.Before(now))
    fmt.Println(then.After(now))
    fmt.Println(then.Equal(now))

    // Sub 函数返回两个时间的差值
    diff := now.Sub(then)
    fmt.Println(diff)
    fmt.Println(diff.Hours())
    fmt.Println(diff.Minutes())
    fmt.Println(diff.Seconds())
    fmt.Println(diff.Nanoseconds())
    // Add 对时间进行前移和后移
    fmt.Println(then.Add(diff))
    fmt.Println(then.Add(-diff))

    // 按照 RFC3339 标准进行格式化
    fmt.Println(now.Format(time.RFC3339))
}
```

#### Random

```go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    // 返回一个整形随机数
    fmt.Println(rand.Intn(100))
    // 返回一个浮点型随机数
    fmt.Println(rand.Float64())

    // 如果处于加密的目的使用随机数，建议使用 `crypto/rand` 包
    // 创建随机种子数
    s1 := rand.NewSource(time.Now().UnixNano())
    // 随机数生成器
    r1 := rand.New(s1)
    // 生成随机数
    fmt.Println(r1.Intn(100))
}
```

#### URL

```go
package main

import (
    "fmt"
    "net/url"
)

func main() {
    s := "postgres://user:pass@host.com:5432/path?k=v#f"

    // 解析 url
    u, err := url.Parse(s)

    if err != nil {
        panic(err)
    }

    // scheme
    fmt.Println(u.Scheme)
    // user 包含 username 和 password
    fmt.Println(u.User)
    // username
    fmt.Println(u.User.Username())
    // password
    p, _ := u.User.Password()
    fmt.Println(p)
    // Host 包括 hostname 和 port
    fmt.Println(u.Host)
    // hostname
    fmt.Println(u.Hostname())
    // port
    fmt.Println(u.Port())
    // path
    fmt.Println(u.Path)
    // hash fragment
    fmt.Println(u.Fragment)
    // 字符串形式 query string
    fmt.Println(u.RawQuery)
    // Map 形式 query string
    m, _ := url.ParseQuery(u.RawQuery)
    fmt.Println(m)
}
```

#### Sha1 / Base64

```go
package main

import (
    "encoding/base64"
    "fmt"
    "crypto/sha1"
)

func main()  {
    // 随机字符串
    s := "random string"
    // 创建散列生成器
    h := sha1.New()
    // 加载数据，如果是字符串，需要转换为 []byte
    h.Write([]byte(s))
    // 求取散列
    bs := h.Sum(nil)
    // 输出
    fmt.Println(s)
    fmt.Printf("%x\n", bs)

    // 创建标准 Base64
    sEnc := base64.StdEncoding.EncodeToString([]byte(s))
    fmt.Println(sEnc)
    // 创建 URL 兼容的 Base64
    uEnc := base64.URLEncoding.EncodeToString([]byte(s))
    fmt.Println(uEnc)
}
```

#### File I/O

```go
package main

import (
    "bufio"
    "fmt"
    "io"
    "io/ioutil"
    "os"
)

func main() {
    // 创建一个文件
    file, _ := os.Create("samp.txt")

    // 写入字符串
    file.WriteString("This is some random text")
    file.Write([]byte{115, 111, 109, 101, 10})

    // 将缓冲区的信息写入磁盘
    file.Sync()

    // 带缓冲的写入
    w := bufio.NewWriter(file)
    n, _ := w.WriteString("buffered\n")
    fmt.Printf("wrote %d bytes\n", n)

    // 通过 Flush 确保所有缓存写入底层写入器
    w.Flush()

    // 关闭文件
    file.Close()

    // 读取文件内容
    data, _ := ioutil.ReadFile("samp.txt")
    fmt.Println(string(data))

    // 打开文件，获取文件操作符
    f, _ := os.Open("samp.txt")

    // 从文件开始位置读取 5 个字节
    b1 := make([]byte, 5)
    n1, _ := f.Read(b1)
    fmt.Printf("%d bytes: %s\n", n1, string(b1))

    // 从第 6 个字符开始读取
    o2, _ := f.Seek(6, 0)
    b2 := make([]byte, 2)
    n2, _ := f.Read(b2)
    fmt.Printf("%d bytes @ %d: %s\n", n2, o2, string(b2))

    // 使用 io.ReadAtLeast 实现更安全
    o3, _ := f.Seek(6, 0)
    b3 := make([]byte, 2)
    n3, _ := io.ReadAtLeast(f, b3, 2)
    fmt.Printf("%d bytes @ %d: %s\n", n3, o3, string(b3))

    // 带缓冲的读取
    r4 := bufio.NewReader(f)
    b4, _ := r4.Peek(5)
    fmt.Printf("5 bytes: %s\n", string(b4))
}
```

#### Scanner

```go
package main

import (
    "bufio"
    "fmt"
    "os"
    "strings"
)

func main() {
    // 基于 os.Stdin 创建一个带缓冲的 scanner
    scanner := bufio.NewScanner(os.Stdin)

    // scanner 调用 Scan() 读取下一行数据
    for scanner.Scan() {
        // Text 返回当前行数据
        ucl := strings.ToUpper(scanner.Text())

        fmt.Println(ucl)
    }

    // 检查错误
    if err := scanner.Err(); err != nil {
        fmt.Fprintln(os.Stderr, "error:", err)
        os.Exit(1)
    }
}
```

#### Log

```go
package main

import (
    "io"
    "io/ioutil"
    "log"
    "os"
)

// 通过 Logger 定义日志记录器
var (
    Trace   *log.Logger
    Info    *log.Logger
    Warning *log.Logger
    Error   *log.Logger
)

func init() {
    log.SetPrefix("Trace: ")
    // 思考一下为什么这里使用位运算
    log.SetFlags(log.Ldate | log.Lmicroseconds | log.Llongfile)

    file, err := os.OpenFile("errors.txt", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0666)
    if err != nil {
        log.Fatalln("Failed to open error log file: ", err)
    }

    // 创建日志记录器并指定输出目标
    Trace = log.New(ioutil.Discard, "Trace: ", log.Ldate|log.Ltime|log.Lshortfile)
    Info = log.New(os.Stdout, "Info: ", log.Ldate|log.Ltime|log.Lshortfile)
    Warning = log.New(os.Stdout, "Warning: ", log.Ldate|log.Ltime|log.Lshortfile)
    Error = log.New(io.MultiWriter(file, os.Stderr), "Error: ", log.Ldate|log.Ltime|log.Lshortfile)
}

func main() {
    // 写到 stdout
    log.Println("message")

    Trace.Println("I have something standard to say")
    Info.Println("Special Information")
    Warning.Println("There is something you need to know about")
    Error.Println("Something has failed")

    // 调用 Println() 之后调用 panic()
    log.Panicln("panic message")

    // 调用 Println() 记录信息之后调用 os.Exit(1)
    log.Fatalln("fatal message")
}
```

#### Command

```go
package main

import (
    "flag"
    "fmt"
    "os"
    "strings"
)

func main() {
    // 通过 os.Args 获取命令行参数
    argsWithProg := os.Args
    // argsWithProg 的第一个参数是该程序的相对路径
    // argsWithProg[1:] 是所有传入的参数
    fmt.Println(argsWithProg)

    // 整数标志
    numbPtr := flag.Int("numb", 42, "an int")
    // 布尔值标志
    boolPtr := flag.Bool("fork", false, "a bool")
    // 字符串标志
    wordPtr := flag.String("word", "foo", "a string")
    // 使用现有变量声明一个标志
    svar := new(string)
    flag.StringVar(svar, "svar", "bar", "a string var")
    // 解析命令
    flag.Parse()

    fmt.Println("word:", *wordPtr)
    fmt.Println("numb:", *numbPtr)
    fmt.Println("fork:", *boolPtr)
    fmt.Println("svar:", svar)
    fmt.Println("tail:", flag.Args())

    // 设置环境变量
    os.Setenv("FOO", "1")
    // 获取环境变量
    fmt.Println("FOO:", os.Getenv("FOO"))
    // 打印所有环境变量
    // os.Environ() 返回所有变量的键值对
    // 形如：["k1=v1", "k2=v2", ...]
    for _, e := range os.Environ() {
        pair := strings.Split(e, "=")
        fmt.Println(pair[0] + "=" + pair[1])
    }
}
```

#### Process

```go
package main

import (
    "fmt"
    "io/ioutil"
    "os"
    "os/exec"
    "syscall"
)

func main() {
    // 创建外部进程执行命令
    lsCmd := exec.Command("bash", "-c", "ls -a -l -h")
    // 收集命令执行完成后的输出
    lsOut, _ := lsCmd.Output()
    fmt.Println(string(lsOut))

    // 初始化命令
    grepCmd := exec.Command("grep", "hello")
    // 获取输入
    grepIn, _ := grepCmd.StdinPipe()
    // 获取输出
    grepOut, _ := grepCmd.StdoutPipe()
    grepCmd.Start()
    grepIn.Write([]byte("hello grep\ngoodbye grep"))
    grepIn.Close()
    grepBytes, _ := ioutil.ReadAll(grepOut)
    grepCmd.Wait()
    fmt.Println(string(grepBytes))

    // 查找命令路径
    binary, _ := exec.LookPath("ls")
    // 设置命令参数
    args := []string{"ls", "-a", "-l", "-h"}
    // 设置环境变量
    env := os.Environ()
    // 执行命令
    execErr := syscall.Exec(binary, args, env)
    if execErr != nil {
        panic(execErr)
    }
}
```

#### Signal

```go
package main

import (
    "fmt"
    "os"
    "os/signal"
    "syscall"
)

func main() {
    sigs := make(chan os.Signal, 1)
    done := make(chan bool, 1)

    // 通过像一个通道发送 os.Signal 进行信号通知
    signal.Notify(sigs, syscall.SIGINT, syscall.SIGTERM)

    go func() {
        sig := <-sigs
        fmt.Println()
        fmt.Println(sig)
        done <- true
    }()

    fmt.Println("awaiting signal")
    <-done
    fmt.Println("exiting")

    // 使用 os.Exit 立即退出程序并设定推出状态
    // 使用 os.Exit 时 defer 将永远不会被调用
    defer fmt.Println("this line will never execute!")
    os.Exit(1)
}
```

## Test

- 测试文件需要使用 `_test.go` 作为结尾后缀，通过对于 `server.go` 文件，对应的测试文件命名为 `server_test.go`


#### Unit Test and Benchmark

创建 Add 函数：

```go
// main.go
package main

import (
    "fmt"
)

// Add ...
func Add(a int, b int) int {
    return a + b
}

func main() {
    fmt.Println(Add(1, 2))
}
```

创建 TestAdd 函数：

```go
// main_test.go
package main

import (
    "testing"
)

func TestAdd(t *testing.T) {
    tests := []struct {
        a      int
        b      int
        result int
    }{
        {1, 2, 3},
        {4, 9, 13},
        {-4, 9, 5},
    }

    for _, tt := range tests {
        actual := Add(tt.a, tt.b)

        if actual != tt.result {
            t.Errorf("Add(%d, %d) expects %d get %d\n", tt.a, tt.b, tt.result, actual)
        }
    }
}

func BenchmarkAdd(b *testing.B) {
    n1 := 1
    n2 := 2
    result := 3

    for index := 0; index < b.N; index++ {
        actual := Add(n1, n2)

        if actual != result {
            b.Errorf("Add(%d, %d) expects %d get %d\n", n1, n2, result, actual)
        }
    }
}
```

- `go test --coverprofile=coverprofile.out` 生成覆盖率文件

- `go tool cover -html=coverprofile.out` 通过浏览器查看覆盖率信息

- `go test -bench .` 基准测试


#### Profile

- `go test -bench . -cpuprofile cpu.out` 生成 CPU 执行的二进制信息

- `go tool pprof cpu.out` 查看 CPU 执行信息，可以在交互式命令行中输入 `web` 在浏览器中查看


#### Mock Function

```go
package main

import (
    "encoding/xml"
    "fmt"
    "net/http"
    "net/http/httptest"
    "testing"
)

var feed = `<?xml version="1.0" encoding="UTF-8"?>
<rss>
<channel>
    <title>Going Go Programming</title>
    <description>Golang : https://github.com/goinggo</description>
    <link>http://www.goinggo.net/</link>
    <item>
        <pubDate>Sun, 15 Mar 2015 15:04:00 +0000</pubDate>
        <title>Object Oriented Programming Mechanics</title>
        <description>Go is an object oriented language.</description>
        <link>http://www.goinggo.net/2015/03/object-oriented</link>
    </item>
</channel>
</rss>`

func mockServer() *httptest.Server {
    f := func(w http.ResponseWriter, r *http.Request) {
        w.WriteHeader(200)
        w.Header().Set("Content-Type", "application/xml")
        // 写入 mock 数据
        fmt.Fprintln(w, feed)
    }

    // 创建一个 mock server
    return httptest.NewServer(http.HandlerFunc(f))
}

type Item struct {
    XMLName     xml.Name `xml:"item"`
    Title       string   `xml:"title"`
    Description string   `xml:"description"`
    Link        string   `xml:"link"`
}

type Channel struct {
    XMLName     xml.Name `xml:"channel"`
    Title       string   `xml:"title"`
    Description string   `xml:"description"`
    Link        string   `xml:"link"`
    PubDate     string   `xml:"pubDate"`
    Items       []Item   `xml:"item"`
}

type Document struct {
    XMLName xml.Name `xml:"rss"`
    Channel Channel  `xml:"channel"`
    URI     string
}

func TestDownload(t *testing.T) {
    server := mockServer()
    defer server.Close()

    resp, err := http.Get(server.URL)
    if err != nil {
        t.Fatal("Error")
    }
    defer resp.Body.Close()
    t.Logf("Status Code %d\n", resp.StatusCode)

    var d Document
    if err := xml.NewDecoder(resp.Body).Decode(&d); err != nil {
        t.Fatal("Error")
    }

    if len(d.Channel.Items) == 1 {
        t.Log("Should have 1 item in the feed.")
    }
}
```

#### REST API

创建 API：

```go
package main

import (
    "encoding/json"
    "log"
    "net/http"
)

func main() {
    Routes()

    log.Println("Listening on :4000")
    http.ListenAndServe(":4000", nil)
}

// Routes ...
func Routes() {
    http.HandleFunc("/json", SendJSON)
}

// SendJSON ...
func SendJSON(res http.ResponseWriter, req *http.Request) {
    u := struct {
        Name  string
        Email string
    }{
        Name:  "Bill",
        Email: "xxx@xxx.xx",
    }

    res.Header().Set("Content-Type", "application/json")
    res.WriteHeader(200)
    json.NewEncoder(res).Encode(&u)
}
```

创建测试文件：

```go
package main

import (
    "encoding/json"
    "net/http"
    "net/http/httptest"
    "testing"
)

func init() {
    Routes()
}

func TestSendJSON(t *testing.T) {
    req, _ := http.NewRequest("GET", "/json", nil)

    // 创建一个 http.ResponseRecorder 值
    rw := httptest.NewRecorder()
    // 调用多路选择器 mux 的 ServeHTTP 方法模拟对 /json 接口的请求
    http.DefaultServeMux.ServeHTTP(rw, req)

    if rw.Code != 200 {
        t.Fatal("Should receive ", rw.Code)
    }

    u := struct {
        Name  string
        Email string
    }{}

    if err := json.NewDecoder(rw.Body).Decode(&u); err != nil {
        t.Fatal("Should decode the response.")
    }

    if u.Name == "Bill" {
        t.Log("Should have a Name.")
    }

    if u.Email == "xxx@xxx.xx" {
        t.Log("Should have an Email.")
    }
}
```

## Generate Doc and Example

- `godoc -http=":3000"` 在浏览器查看 Go 文档、package 文档

- `go doc` 在命令行中查看


库文件 `queue.go` :

```go
package queue

// Queue A FIFO queue.
type Queue []int

// Push Pushes the element into the queue.
// 		e.g. q.Push(123)
func (q *Queue) Push(v int) {
    *q = append(*q, v)
}

// Pop Pops element from head.
func (q *Queue) Pop() int {
    head := (*q)[0]
    *q = (*q)[1:]
    return head
}

// IsEmpty Returns if the queue is empty or not.
func (q *Queue) IsEmpty() bool {
    return len(*q) == 0
}
```

测试文件 `queue_test.go` :

```go
package queue

import "fmt"

func ExampleQueue_Pop() {
    q := Queue{1}
    q.Push(2)
    q.Push(3)
    fmt.Println(q.Pop())
    fmt.Println(q.Pop())
    fmt.Println(q.IsEmpty())

    fmt.Println(q.Pop())
    fmt.Println(q.IsEmpty())

    // Output:
    // 1
    // 2
    // false
    // 3
    // true
}
```

## 架构演进

#### single task

```go
package main

import (
    "fmt"
)

func main() {
    sum := 0

    for index := 0; index < 10000; index++ {
        sum = add(sum, index)
    }

    fmt.Println("Sum:", sum)
}

func add(a int, b int) int {
    return a + b
}
```

#### concurrency task

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    value := make(chan int)
    sum := 0

    for index := 0; index < 10; index++ {
        go func(i int) {
            start := i * 1000
            end := start + 1000
            intervalSum(value, start, end)
        }(index)
    }

    for index := 0; index < 10; index++ {
        select {
        case v := <-value:
            sum += v
        }
    }

    time.Sleep(time.Millisecond * 100)
    fmt.Println("sum: ", sum)
}

func intervalSum(value chan<- int, start int, end int) {
    sum := 0

    for index := start; index < end; index++ {
        sum = add(sum, index)
    }

    value <- sum
}

func add(a int, b int) int {
    return a + b
}
```

#### distribute task - RPC

定义 RPC Service：

```go
package xrpc

import (
    "errors"
)

// XService ...
type XService struct{}

// Args ...
type Args struct {
    A int
    B int
}

// Div ...
func (XService) Div(args Args, result *float64) error {
    if args.B == 0 {
        return errors.New("division by zero")
    }

    *result = float64(args.A) / float64(args.B)
    return nil
}
```

定义接收 RPC 请求的服务端：

```go
package main

import (
    "go-in-action"
    "log"
    "net"
    "net/rpc"
    "net/rpc/jsonrpc"
)

func main() {
    rpc.Register(xrpc.XService{})

    // 监听 tcp 请求
    listener, err := net.Listen("tcp", ":1234")

    if err != nil {
        panic(err)
    }

    for {
        // 创建连接
        conn, err := listener.Accept()

        if err != nil {
            log.Printf("accept error: %v", err)
            continue
        }

        // 将连接移交给 rpc 框架
        go jsonrpc.ServeConn(conn)
    }

}
```

定义发送 RPC 请求的客户端：

```go
package main

import (
    "fmt"
    "go-in-action"
    "net"
    "net/rpc/jsonrpc"
)

func main() {
    // 创建一个请求连接
    conn, err := net.Dial("tcp", ":1234")
    if err != nil {
        panic(err)
    }

    // 创建一个客户端
    client := jsonrpc.NewClient(conn)

    var result float64

    // 发送请求
    err = client.Call("XService.Div", xrpc.Args{A: 10, B: 3}, &result)
    if err != nil {
        fmt.Println(err)
    } else {
        fmt.Println(result)
    }

    err = client.Call("XService.Div", xrpc.Args{A: 10, B: 0}, &result)
    if err != nil {
        fmt.Println(err)
    } else {
        fmt.Println(result)
    }
}
```

## Concurrency

#### Race Data Check

当至少两个 goroutine 并发访问（读、写）同一个变量时，且其中一个为写操作，在 Golang 中就会对数据产品竞态：

```go
package main

import "fmt"

func main() {
    i := 0

    go func() {
        // 写操作
        i++
    }()

    // 读操作
    fmt.Println(i)
}
```

通过 `go run --race [package]` 进行检查：

```powershell
$ go run --race main.go

0
==================
WARNING: DATA RACE
# 写操作
Write at 0x00c0000180c8 by goroutine 6:
  main.main.func1()
      /Users/sean/go/src/go-in-action/main.go:10 +0x4e
# 读操作
Previous read at 0x00c0000180c8 by main goroutine:
  main.main()
      /Users/sean/go/src/go-in-action/main.go:14 +0x88

# Go routine 位置
Goroutine 6 (running) created at:
  main.main()
      /Users/sean/go/src/go-in-action/main.go:8 +0x7a
==================
Found 1 data race(s)
exit status 66
```

#### Deadlock Check

当一组 goroutines 互相等待对方执行完成时，在 Golang 中就会产生死锁：

```go
package main

import "fmt"

func main() {
    ch := make(chan int)
    // 发送消息并永久等待其他 goroutine 读取这个消息
    ch <- 1
    // 虽然此处申请读取消息，但由于上一步一直在等待，所以永远执行不到这一步
    fmt.Println(<-ch)
}
```

#### WaitGroup

`sync.WaitGroup` 可以用于等待一组 goroutine 结束执行：

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var wg sync.WaitGroup

    wg.Add(2)

    go func() {
        fmt.Println("1")
        wg.Done()
    }()
    go func() {
        fmt.Println("2")
        wg.Done()
    }()

    wg.Wait()
}
```

#### Parallel

- Each work unit should take about 100μs to 1ms to compute. If the units are too small, the adminis­trative over­head of divi­ding the problem and sched­uling sub-problems might be too large. If the units are too big, the whole computation may have to wait for a single slow work item to finish. This slowdown can happen for many reasons, such as scheduling, interrupts from other processes, and unfortunate memory layout. (Note that the number of work units is independent of the number of CPUs.)
- Try to minimize the amount of data sharing. Concurrent writes can be very costly, particularly so if goroutines execute on separate CPUs. Sharing data for reading is often much less of a problem.
- Strive for good locality when accessing data. If data can be kept in cache memory, data loading and storing will be dramatically faster. Once again, this is particularly important for writing.

## Third Party

- MySQL ORM: [https://github.com/jinzhu/gorm](https://github.com/jinzhu/gorm)
- Redis: [https://github.com/go-redis/redis](https://github.com/go-redis/redis)
- RxGO: [https://github.com/ReactiveX/RxGo](https://github.com/ReactiveX/RxGo)
- GRPC: [https://grpc.io](https://grpc.io/)
- WebSocket: [https://www.imooc.com/video/17603](https://www.imooc.com/video/17603)


## References

- [https://gobyexample.xgwang.me](https://gobyexample.xgwang.me/)
- [http://divan.github.io/posts/go_concurrency_visualize](http://divan.github.io/posts/go_concurrency_visualize/)
- [http://divan.github.io/posts/go_concurrency_visualize](http://divan.github.io/posts/go_concurrency_visualize/)
