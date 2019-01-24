# PHP vs Golang

## 为什么你应该学习Go语言呢？

作为多年的PHP开发工程师，你是不是总认为自己在堆砌业务代码？

一直以来，都是感觉无法提升自身的技术能力？

想要有所成长，总是控制不住自己，花费大量的时间去刷微博，朋友圈，抖音？

是时候，开始学习一门新的语言了，比如Go语言。

从PHP程序员的角度来说，我觉得有以下几点值得你入手Go语言：

1. Go语言简单，容易上手。你可以很快的上手，开发测试运维Go服务。
2. Go语言有效的提升了并发编程的体验，不再有复杂的并发和控制方式。
3. Go语言的常用库很丰富。基本Web开发，后端编程，网络编程基本上都有。
4. Go语言拥有C语言的灵活，拥抱底层，有着Python的简约，快速开发。

## 如何快速入门Go语言呢

我认为凡事都应该会有方法论的，除了方法论之外，剩下就是践行了。学好一门静态语言的方法论，大致有一下这些：

1. 基础且必要的知识概念
2. 用自己的熟悉的东西做类比
3. 最后就是亲自实践了

在经过一段时间的学习后，我从以下几个方面对比了PHP和Go的区别：

1. 变量申明
2. 包管理
3. 面向对象
4. 接口
5. 单元测试
6. 协程
7. Http服务

## PHP vs Go

### 变量申明

在PHP的语法里是没有类型的概念，变量是直接申明即可使用的。

```php
<?php
// 数字
$geeks = 11;
// 字符串
$geeks = 'hello world';
// 申明一个数组
$geeks = [];
// 申明一个空对象
$who = new stdClass();
```

但是Go语言有，你可以使用自动类型推断，也可以申明变量的类型。

```go
package main

import (
    "fmt"
)

func main() {
    // 申明变量geekwho,并自动赋值为0，默认Go会推断为int的类型
    geeks1 := 0
    // 你也可以直接申明为int类型的变量geekwho
    var geeks2 int
    // 申明两个变量i，j为int的类型并初始化
    var i, j int = 1, 2
    // 申明3个变量并初始化
    var go_, php, java = true, false, "no!"
    // 申明了变量，请立即使用
    fmt.Println(geeks1,geeks2,i,j,go_,php,java)
}
```

### 包管理

在PHP里，我们使用composer包来管理相关库。通过`composer require monolog/monolog` 命令安装包，通过`include_once` 加载包。

```php
<?php
// 在项目引入composer包
$autoload = "vendor/autoload.php";
if(file_exists($autoload)){
    include_once $autoload;
}
```

在Go语言是直接通过import导入相关的包，下面的例子，引入fmt包以及math包，进行自动格式化和数学相关的函数计算。

```go
package main

import "fmt"
/**
 * 每次导入一个包
 * import "fmt"
 * import "math"
 *
 * 也可以分组导入
 * import (
 *     "fmt"
 *     "math"
 * )
 */

func main() {
    fmt.Println("hello world")
}
```

### 面向对象

举例，PHP申明一个类helloWorld，有一个公共方法run计算平方根。

```php
<?php
    class HelloWorld
    {
        public $x;
        public $y;
        public function __construct($x , $y){
            $this->x = $x;
            $this->y = $y;
        }
        public function run()
        {
            echo sqrt($this->x * $this->x + $this->y * $this->y);
        }
    }
    (new HelloWorld(3,4))->run();
```

Go的世界里没有类的概念，只有结构体，也可以实现类。

```go
package main

import (
    "fmt"
    "math"
)
// 申明一个结构体
type HelloWorld struct {
    X, Y float64
}
// 添加一个Run方法，首字母大写为公用方法，小写为私有方法。
func (v HelloWorld) Run() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
    // 初始化类
    v := HelloWorld{3, 4}
    fmt.Println(v.Run())
}
```

### 接口

代码申明了一个开关的接口，有两个方法on和off。

```php
<?php
    interface switchs {
        public function on();
        public function off();
    }
    class Light implements switchs
    {
        public $status;
        public function __construct(){
        }
        public function on()
        {
            $this->status = 'light is on';
            echo $this->status . PHP_EOL;
        }
        public function off()
        {
            $this->status = 'light is off';
            echo $this->status . PHP_EOL;
        }
    }
    $light = new Light();
    $light->on();
    $light->off();
```

那么用Go如何实现呢？

```go
package main

import (
    "fmt"
)
// 申明接口有2个方法
type Switchs interface {
    On() string
    Off() string
}

// 申明一个结构体，类似PHP的Light类
type Light struct {
    Status string
}
//添加一个On方法
func (l Light) On() string {
    l.Status = "light is on"
    return l.Status
}
//添加一个Off方法
func (l Light) Off() string {
    l.Status = "light is off"
    return l.Status
}

func main() {
    // 初始化类
    l := Light{}
    fmt.Println(l.On())
    fmt.Println(l.Off())
}
```

### 单元测试

PHP采用phpunit实现一个单元测试。

```php
<?php
class PHPTest extends PHPUnitFrameworkTestCase
{
    public function testRun()
    {
        $tests = [
            'stdClass',
        ];
        foreach ($tests as $test) {
            $this->assertEquals(
                true,
                class_exists($test),
                "$test class not found"
            );
        }
    }
}
```

Go语言自带单元测试功能，只需要加载两个包即可。

```go
// unittest.go
package main

func Sum(a, b int) int {
    return a + b
}
// unittest_test.go
package main

import (
    "testing"
    "github.com/stretchr/testify/assert"
)

func TestRun(t *testing.T) {
    val := Sum(1, 2)
    assert.Equal(t, 3, val)
}
```

### 协程

PHP本身没有协程的概念，可以使用Swoole扩展实现。

```php
<?php
class SwooleCoroutine
{
    public function run()
    {
        extension_loaded('swoole') or die('swoole extension is not installed');
        $this->case1();
        $this->case2();
        $this->case3();
    }

    /**
     * 开启3个协程，无阻塞IO
     */
    public function case1()
    {
        echo "run " . __FUNCTION__ ." ". microtime(true). PHP_EOL;

        go(function () {
            echo "hello go1 " . PHP_EOL;
        });

        echo "hello main " . PHP_EOL;

        go(function () {
            echo "hello go2 " . PHP_EOL;
        });
    }


    /**
     * 开启3个协程，第一个协程阻塞
     */
    public function case2()
    {
        echo "run " . __FUNCTION__ ." " . microtime(true). PHP_EOL;

        go(function () {
            Co::sleep(1); // 只新增了一行代码
            echo "hello go1 " . PHP_EOL;
        });

        echo "hello main " . PHP_EOL;

        go(function () {
            echo "hello go2 " . PHP_EOL;
        });
    }

    /**
     * 开启3个协程，第3个协程阻塞
     */
    public function case3()
    {
        echo "run " . __FUNCTION__ ." ". microtime(true). PHP_EOL;

        go(function () {
            echo "hello go1 " . PHP_EOL;
        });

        echo "hello main " . PHP_EOL;

        go(function () {
            Co::sleep(1); // 只新增了一行代码
            echo "hello go2 " . PHP_EOL;
        });
    }
}
(new SwooleCoroutine())->run();
```

Go的协程内部是采用复杂的MPG调度器实现。最后的一句`time.Sleep(time.Second)` 是为了等待协程完成。

```go
package main

import (
    "fmt"
    "time"
)

func say(s string , b bool) {
    if b {
        time.Sleep(100 * time.Millisecond)
    }
    fmt.Println(s,time.Now().UnixNano())
}

func case1() {
    fmt.Println("In case1()-",time.Now().UnixNano())
    go say("case1 hello go1",false)
    say("case1 hello main",false)
    go say("case1 hello go2",false)
}

func case2() {
    fmt.Println("In case2()-",time.Now().UnixNano())
    go say("case2 hello go1" , true)
    say("case2 hello main",false)
    go say("case2 hello go2",false)
}

func case3() {
    fmt.Println("In case3()-",time.Now().UnixNano())
    go say("case3 hello go1",false)
    say("case3 hello main",false)
    go say("case3 hello go2" , true)
}

func main() {
    fmt.Println("In main()---",time.Now().UnixNano())
    case1()
    case2()
    case3()
    time.Sleep(time.Second)
}
```

### 实战Http服务

用PHP的Swoole扩展实现一个简单的Http服务。

```php
<?php
// 启动本地的2018端口的Http Server
$server = new SwooleHttpServer("0.0.0.0", 2018, SWOOLE_BASE);

// 设置worker 数量 和 守护进程化
$server->set([
    'worker_num' => 1,
    'daemonize' => 1,
]);

// 添加请求回调
$server->on('Request', function ($request, $response) {
    // 加载协程类
    include_once dirname(__DIR__) . DIRECTORY_SEPARATOR . 'coroutine/coroutine.php';
    // 从缓冲区获取数据
    ob_start();
    (new SwooleCoroutine())->run();
    $http = ob_get_contents();
    ob_end_clean();
    // 输出结果
    $response->end($http);
});
// 启动服务
$server->start();
```

Go语言的话，直接采用采用默认的http包就可以了。

```go
package main

import (
    "fmt"
    "net/http"
)

func main() {
    http.HandleFunc("/", func (w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "hello world!")
    })

    fs := http.FileServer(http.Dir("static/"))
    http.Handle("/static/", http.StripPrefix("/static/", fs))

    http.ListenAndServe(":2019", nil)
}
```

### 代码

1. [变量申明](./var)
2. [包管理](./package)
3. [面向对象](./object)
4. [接口](./interface)
5. [单元测试](./unittest)
6. [协程](./coroutine)
7. [Http服务](./http)

### 测试

执行单元测试

```shell
bash run.sh
```

### 总结

文章分享了如何快速入门Go语言的方法论，并亲自用熟悉的概念将PHP与Go进行了对比。当然，如果仅仅只是一篇文章，是不足以深入了解语言的真谛，还需要花费时间亲自去践行，实践才检验真理的唯一标准。

### 参考链接

1. [GO语言、DOCKER 和新技术](https://coolshell.cn/articles/18190.html)
2. [Why should you learn Go?](https://medium.com/@kevalpatel2106/why-should-you-learn-go-f607681fad65)
3. [Swoole PHP Coroutine](https://www.swoole.co.uk/coroutine)
4. [swoole| swoole 协程初体验](https://www.jianshu.com/p/745b0b3ffae7)
5. [The Go scheduler](http://morsmachine.dk/go-scheduler)
6. [The Go Programming Language](https://golang.org/)
7. [Go Web Examples](https://gowebexamples.com/)
8. [Package httptest](https://golang.org/pkg/net/http/httptest/)
