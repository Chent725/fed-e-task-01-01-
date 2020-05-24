# fed-e-task-01-01-

```
fed-e-task-01-01 ······················ 
├── code ······························  （无）
│   └── app.js ························  （无）
├── notes ····························· 笔记目录
│   ├── 内容总结.xmind ················· 学习总结
│   └── 学习笔记.md ···················· 学习笔记
├── explain.mp4 ······················· 作业解说
└── README.md ························· 简答题答案以及说明
```

# 1、变量提升

```ts
var a: any[] = [];
for (var i: number = 0; i < 10; i++) {
    a[i] = function (): void {
        console.log(i)
    }
}
a[6]()


//result  
//10
//reason
/*
    上面这段代码相当于
    var i:number=0
    for(var i:number=0;i<10;i++){
        i=1
        i=2
        i=3
        ……
        i=10
    a[i]=function():void{
        console.log(i) //10
    }
}
//因为var是全局声明的，for又循环赋值，最后一次为10，故此打印的为10
//闭包
var a:any[]=[];
for(var i:number=0;i<10;i++){
    (a[i]=function(i:number):void{
        console.log(i)
    })(i)
}
a[6]()
//或者let都可以解决这个问题得到预期的效果
var a:any[]=[];
for(let i:number=0;i<10;i++){
    a[i]=function():void{
        console.log(i)
    }
}
a[6]()
*/
```

# 2、let

```ts
//报错ref，
/*
	因为let不会变量提升且之前的都是临时性死区，
	相当于进行了rhs查询但没有找到lhs声明

*/
```

# 3、es6特性

```
Math.min(...arr)
```

# 4、声明器

```ts
/*
	var：作用域：全局，存在变量提升
*/
/*
	let：作用域：块级作用域，不存在变量提升
		存在临时性死区，在let声明之前使用声明的变量名赋值会报ref引用错误
		不能重复声明，其实就是临时性死区的原因
		
*/
/*
	const：作用域：块级作用域，不存在变量提升
           存在临时性死区，在let声明之前使用声明的变量名赋值会报ref引用错误
           不能重复声明，其实就是临时性死区的原因
		   定义常量
		   保证不可变的是内存地址
		   exp：
		      const a='12'  a=1//typeError
		      const a={} a.a=1 //{a:1}
		
*/
```

# 5、箭头函数

```ts
//箭头函数内部没有this，他的this指向是创建他时所处的对象
```

# 6、symbol

```ts
/*
	1、作为唯一键值
	2、使用生成器、迭代器的唯一入口
*/
```

# 7、浅拷贝和深拷贝

```
/*
	其实这种主要出现在嵌套的情况下
	1、浅拷贝
		只能拷贝一层
		仅拷贝了引用地址，当赋值时会影响被拷贝对象
	2、深拷贝
		可以遍历拷贝
		拷贝了数据结构，是一个全新的对象，赋值不会影响原对象，
*/
```

# 8、异步编程

 js语言是单线程的，但是目前的发展情况很多时候需要类似于多线程的操作。例如web端api请求，node环境大文件读取等等比较耗时的操作，因此js就创造了一些异步函数（回调、发布订阅、监听、promise）来提供异步编程。

## event loop 

事件循环，监听调用栈和消息队列

当同步函数执行完后，开始执行消息队列，遵循先进先出的原则抛出。

## 宏任务

| #                     | 浏览器 | Node |
| --------------------- | ------ | ---- |
| setTimeout            | √      | √    |
| setInterval           | √      | √    |
| setImmediate          | x      | √    |
| requestAnimationFrame | √      | x    |

## **微任务**

| #                          | 浏览器 | Node |
| -------------------------- | ------ | ---- |
| process.nextTick           | x      | √    |
| MutationObserver           | √      | x    |
| Promise.then catch finally | √      | √    |

宏任务也可以优于微任务进eventloop

如果存在多轮宏任务和微任务，那么执行顺序一定是第一轮宏任务执行完后，执行第一轮微任务；第二轮宏执行完后，执行第二轮微

![img](https://images2018.cnblogs.com/blog/1053223/201808/1053223-20180831162350437-143973108.png)

# 9、promise改造

```js
function fn1() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('hello')
        }, 10)
    })
}

function fn2(value) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            let b = 'lagou'
            resolve(value + b)
        }, 10)
    })
}

function fn3(value) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            let c = 'i love u'
            console.log(value + c);
        }, 10)
    })
}

function onerror(){
    console.log('error')
 }
 

fn1().then(fn2).then(fn3).catch(onerror)
```

# 10、简述TypeScript和JavaScript之间的关系

- TS是JS的超集、增加了类型检测
- TS支持es最新的语法也就是支持JS所有的语法
- .TS可以直接运行JS语法且运行阶段也不会出错

# 11、ts的优缺点

## 优点

- 渐进式：需要使用就用，不需要的时候，语法会报错，但是依旧可以运行
- 兼容：兼容js所有的语法，上手容易
- 可读性好：提供了类型检测，规范了项目中的变量，减少注释和类型判断代码
- 功能：TS输出时可选输出JS版本，可以为项目做兼容。

## 缺点

- 学习成本：类、泛型之类比较抽象偏向于强类型语言的概念，需要多使用了解
- 使用成本：改造项目需要重新配置webpack，可能还会踩坑。适用于大项目或者需要长期维护的项目，小型项目使用会浪费时间。
- 生态还不完善：许多周边生态还未使用ts，因此引入使用，语法成面会报错，小型的库还未用ts重构支持性差。