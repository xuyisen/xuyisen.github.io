---
title: 简单克隆代码检测(Scala函数式编程初探)
comment: true
copyright: true
date: 2018-08-30 09:44:56
categories: Scala 
tags: clone_detection  
---

函数式编程又称泛函编程的一种编程泛型，他将计算机运算视为数学上的函数计算，并且避免使用程序状态以及易变对象。函数编程语言最重要的基础是lambda演算。    

<!--more-->   

# 命令式编程(lmperative)  

命令"机器"如何去做事情，这样不管你想要的是什么，他都会按照你的命令实现。   

# 函数式编程(functional)  

告诉"机器"你想要什么，让机器想出如何去做   

# Example  

* Java Code \lmperative  
``` 
Integer[] nums = new Integer[]{1, 2, 3, 4};
int i=0;
for (Integer n : nums) {
    n*=2;
    nums[i++]=n;
}
```
遍历整个数组，取出每个元素，乘以二，然后把翻倍后的值放回数组，每次都要操作这个数组，直到计算完所有元素。  
* Scala code \ functional  
```
val nums = Array(1,2,3,4)
val doubleNums = nums.map(_*2)
```
利用Array的map函数，将所有元素*2，并生成了一个新的双倍数组。map函数所做的事情是把遍历整个数组的过程，归纳并抽离出来，让我们专注于描述我们想要的是什么"_*2"。我们传入map的是一个纯函数；它不具有任何副作用(不会改变外部状态)，它只是接收一个数字，返回乘以二后的值。PS： ‘_’ 字符是scala中的入参缩写形式。  

# 普通函数  

```
def max(x: Int, y: Int): Int = {
  if (x > y) x else y
}
```
def 是声明函数的关键字，小括号中的x:Int,y:int是函数的入参，紧跟着的:Int是函数返回值类型，花括号中的是函数体。  

# 匿名函数  

```
val increment = (x: Int) => x + 1
increment(2)
// output 3 
```
匿名函数是指一类无需定义标识符的函数，定义匿名函数的语法很简单，=>箭头左边是参数列表，右边是函数体。可以把命名函数赋值给一个变量使用。  
另一种方式  
```
val increment =new Function1[Int, Int] {
  def apply(x: Int): Int = x + 1
}
```

# 无参匿名函数  

```
val sayHello = () => { System.out.print("Hello World!") }
sayHello()
//output: Hello World!
```

# 高阶函数  

满足以下条件之一：   
* 接受一个或多个函数作为输入  
* 输出一个函数  

在数学领域他们也叫做算子或泛函。微积分中的导数就是常见的例子，它映射一个函数到另一个函数。  
```
val convert2String=(x:Int)=>"==[" + x.toString() + "]=="
def apply(f: Int => String, v: Int) = f(v)
apply(convert2String,36)
//output: ==[36]==
```

# 嵌入式函数(本地函数)  

在Scala中可以在函数体中再定义一个函数。  

```
def filter(xs: List[Int], threshold: Int) = {
    def process(ys: List[Int]): List[Int] =
    if (ys.isEmpty) ys
    else if (ys.head < threshold) ys.head :: process(ys.tail)
    else process(ys.tail)
    process(xs)
}
```

# 函数柯里化(Currying)  

在计算机科学中，柯里化是指把接受多个参数的函数变换成接受一个单一参数的函数，并且返回其余的参数和结果的函数。  

```
def curriedSum(x: Int)(y: Int)(z: Int) = x + y + z
val sum = curriedSum(1)(2)(3)
//output: 6

//the calling procedure of currying function
def first(x: Int) = (y: Int) => (z: Int) => x + y + z
def second = first(5)
//output: Int => (Int => Int) = 5 + y + z
def third = second(6)
//output: Int => Int = 5 + 6 + z
val fourth = third(7)
//output: 18

first(5)(6)(7)
//output: 18
```
代码范例中，curriedSum是一个currying函数的Scala写法，而first、second、third、fourth展示了currying函数调用过程。通过代码注释可看到second、third返回的都是函数，直到fourth才返回求和值。而这种特性也正是惰性求值的秘密所在。  

# 部分应用函数  

```
def sum(x: Int, y: Int, z: Int) = {
x + y + z
}
val sum2 = sum(10, _: Int, 15)
sum2(2)
//output: 27
```

# 偏函数  

## 定义  

从输入集合X到可能的输出集合Y的函数f(记作f(X)=>Y)是X与Y的关系，满足如下条件：  
1. f是完全的即对于集合X中的任一元素x都有集合Y中的元素y，从而满足f(x)=y,换句话说就是每一个输入值x都有y与之对应。  
2. f是多对一的即存在 f(x_{a})=y且f(x_{b})=z并且y=z。也就是多个输入可以映射一个输出，但一个输入不能映射多个输出    

X与Y的关系f满足条件1，则为`多值函数`。X与Y的关系f满足条件2，则为`偏函数`。  

```
val pf: PartialFunction[Int, String] = {
  case 1 => "One"
  case 2 => "Two"
  case 3 => "Three"
  case 9 => "Nine"
  case _=> "Nothing"
}
pf(1) // output: One
pf(2) // output: Two
pf(5) // output: Nothing
pf(9) // output: Nine
```
示例代码中，我们定义了一个Scala的偏函数，那么只有1、2、3、9有对应的输出值，而输入其它值只会打印 Nothing。  
```
val L1 = List(1, 2, 3, "A", 4, "B", 5)
val L2 = L1 map { 
case x: Int => x * x
case x: String => s"$x*$x"
}
//L2: List[Any] = List(1, 4, 9, A*A, 16, B*B, 25)
```

# 闭包  

又称函数闭包，是引用了自由变量的函数。这个被引用的自由变量将和这个函数一同存在，换句话说，闭包是由函数和与其相关的引用环境组合成的。    
另外一种解释，对象是带有函数的数据，闭包是带有数据的函数。
```
val more = 2
val addMore = (x: Int) =>  x + more
addMore(1)
```

# 传值调用函数(call by value)  

将未计算的参数表达式直接应用到函数内部即为传名调用。  
```
def assert(predicate: => Boolean) =
if (!predicate)
throw new AssertionError
assert(1 > 3)
def compare(x: Int, y: Int): Boolean = {
if (x > y) true else false
}
assert(compare(1, 3))
```

# 可变参数  

```
def echo(args:String*)={
for(arg <- args) println(arg)
}
echo("Hello","World","!")
//output: Hello World!
```

# 管道(pipeline)函数  

```
def even(x:Array[Int]):Array[Int]={
  x.filter(_%2==0)
}
def square(x:Array[Int]):Array[Int]={
  x.map(e=>e*e)
}
def total(x:Array[Int]):Int={
  x.reduce(_+_)
}
val nums = Array(1,2,3,4,5,6,7,8,9,10)
val pipeline = total(square(even(nums)))
//output: 220
```

# 递归函数  

```
def fun(i:Int):Unit=
{
if (i < 10 && i >= 0)
  {
println(s"i:$i");
    fun(i + 1);
  }
}
fun(1)
```
Scala编译器优化了尾递归调用，递归调用和执行一个while循环没有性能上区别。  

# 方法与函数  

在Scala中函数如果是某个对象的成员，那么称这种`函数`为`方法`。  
```
class Sample{
def sayHello():Unit={
print("Hello")
  }
}
```