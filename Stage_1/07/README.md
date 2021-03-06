# 词法作用域VS动态作用域

这片文章主要讲解关于js作用域的一些基础知识。

(我们来走一下科学的研究方法)

### 抛出问题

两个平行级别的函数声明A和B，其中B在A的内部调用，那么，在A内部被调用的B函数能否获得外层函数A作用域的访问能力？

### 提出猜想

> 根据自己已有的知识积累和经验给出自己的猜想。

#####  我的猜想：

不能.

因为js遵循词法作用域，一个函数所能访问到的作用域是在它所被声明的地方决定，也就是说，它能访问到哪些作用域以及哪些变量，在词法分析阶段就已经确定了。词法分析是什么阶段？就是解释器对js进行解析的第一个阶段，把一个个无意义的字符组装为一个个有意义的单词的阶段。

针对这个问题去分析，就是B所能访问的作用域是在它声明的地方决定的，所以它能访问全局作用域，但是不能访问A里面的作用域，即便看起来B在被调用的时候是在A作用域的里面，但是也没有能力去访问A里面的变量。

### 实验验证

> 写出具有针对性验证这个问题的实验案例

    function A(){
        var a = 3;
        B();
    }

    function B(){
        console.log(a);
    }
    
    A();
    var a = 2;

### 记录结果

> 通过试验得出结果

    undefined

### 总结结论

> 分析试验的结果和自己的猜想所存在的差异，分析是什么原因导致了这些差异。得出自己的结论。然后再提出问题。

结果是符合预期的。因为没有访问到A里面的`a=3`

但是为什么会输出`undefined`呢？这就是下一个问题，你们自己去验证吧。

### 无聊的概念

真对函数，词法作用域的意思就是：它被“声明”的地方决定了它对作用域和变量的访问能力，而不是它被“嗲用”的地方。

### 举一反三

那么动态作用域呢？相对于词法作用域我们很容易就可以理解动态作用域是什么了，就是函数对作用域的访问能力是动态的，是根据函数的执行来决定的。也就是在动态作用域中，上面的实验案例会输出`3`;

那么js里面有没有动态作用域呢？

### “仿”动态作用域

eval( )函数。这个因性能低下而被广为人知的函数，但是的确有模拟动态作用域的能力。

    function A(str){
        eval(str);
        console.log(a);
    }
    A('var a=3;');   // 3

可以看出，这里的确是根据函数函数执行的时来确定了console.log能否访问到变量a;

如果`A('var b = 3;');`就会报引用错误了。




