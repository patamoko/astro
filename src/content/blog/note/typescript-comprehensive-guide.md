---
title: TypeScript 学习笔记
link: typescript-comprehensive-guide
catalog: true
date: 2026-06-12 15:30:00
description: 一份全面的 TypeScript 课程笔记，涵盖从发展史、基础语法到高级类型（泛型、条件类型、infer 等）的完整知识体系。
tags:
  - 前端
  - TypeScript
  - 笔记
categories:
  - 笔记
  - 前端
  - TypeScript
---

# 1.认识 TypeScript

# 1.发展史

TypeScript 是 JavaScript 的⼀个超集，⽀持 ECMAScript 6 标准（ES6 教程）。 

TypeScript 由微软开发的⾃由和开源的编程语⾔。 

TypeScript 设计⽬标是开发⼤型应⽤，它可以编译成纯 JavaScript，编译出来的 JavaScript 可以运⾏在任何 浏览器上 

中⽂官⽹：https://ts.nodejs.cn/ 

2013 年，微软的 Visual Studio 2013 开始内置⽀持 TypeScript 语⾔。 

2014 年，TypeScript 1.0 版本发布。同年，代码仓库搬到了 GitHub。 

2016 年，TypeScript 2.0 版本发布，引⼊了很多重⼤的语法功能。 

2018 年，TypeScript 3.0 版本发布。 

2020 年，TypeScript 4.0 版本发布。 

2023 年，TypeScript 5.0 版本发布。 


# 1.JavaScript 是动态类型、弱类型语⾔

动态类型是指在运⾏时才会进⾏类型检查，这种语⾔的类型错误往往会导致运⾏时错误。JavaScript 是⼀⻔ 解释型语⾔，没有编译阶段，所以它是动态类型，以下这段代码在运⾏时才会报错： 

# 2.TypeScript 是静态类型

静态类型是指编译阶段就能确定每个变量的类型，这种语⾔的类型错误往往会导致语法错误。TypeScript 在 运⾏前需要先编译为 JavaScript，⽽在编译阶段就会进⾏类型检查，所以 TypeScript 是静态类型 

# 3.安装

```bash
npm install -g typescript 
```

JavaScript 的运⾏环境（浏览器和 Node.js）不认识 TypeScript 代码。所以，TypeScript 项⽬要想运⾏，必须先转 为 JavaScript 代码，这个代码转换的过程就叫做“编译”，TypeScript 官⽅提供的编译器叫做 tsc，可以将 TypeScript 脚本编译成 JavaScript 脚本 

安装完成后，检查⼀下是否安装成功 

```txt
$ tsc -v
Version 5.1.6 
```

# 2.使⽤TS

通常我们使⽤ .ts 作为 TypeScript 代码⽂件的扩展名。 

然后执⾏以下命令将 TypeScript 转换为 JavaScript 代码： 

```txt
tsc app.ts
//
tsc app.ts -w //不需要每次重复执行编译命令 
```

执⾏过程如下 

![](/img/notes/4253077211849b4f8a43ae582157de3fdd8a478a432538e4a343a755be081d6a.jpg)


# 3.原始数据类型

1.原始数据类型：boolean number string undefined null symbol 

2.对象类型：对象（object）、数组（Array）、元组（Tuple）、枚举（enum）、函数（function） 

3.其他类型： any、never unknown void 值类型 

typescript 要求在变量声明的时候要声明值的类型，如下 

let 变量名:类型=数据 

注意，上⾯所有类型的名称都是⼩写字⺟，⾸字⺟⼤写的 Number、String、Boolean 等在 JavaScript 语⾔中都 是内置对象，⽽不是类型名称。 

原始数据类型的声明⽅式案例 

```typescript
//string
let a:string="hello"
let b:string
var c:string="aa"
const d:string="bb"
//number
let n:number=123
//boolean
let boo:boolean=true
//undefined
let u:undefined=undefined
//null
let nu:null=null
//symbol
let s:symbol=Symbol() 
```

# 4.其他类型

# 1.any 类型

any 表示的是任意类型，可以任意赋值，⼀个变量设置类型为 any，相当于对该变量关闭了类型检测 如果声明⼀个变量不设置类型，其实也是 any 类型。所以在开发中不建议使⽤该⽅法 

# 2.unknown 类型

unknown 相对于 any 是安全的 

要想正常使⽤unknown，必须保证操作是合法的 

```typescript
let a:unknown=5;
if(typeof(a)=="number") {
    console.log(a*2)
} else if(typeof(a)=="string") {
    a.substring(1)
} 
```

# 3.never 类型

never 表示永远不会有返回值的类型，⽐如抛出异常，或者死循环，那么该函数永远执⾏不完，不可能有返回 值 

# 4.值类型

TypeScript 规定，单个值也是⼀种类型，称为“值类型”。 

```typescript
let a:123=123
let b:"hello"="hello" 
```

# 5.关于 tsconfig.json⽂件

tsconfig.json ⽂件是 TypeScript 项⽬的配置⽂件。通过 tsconfig.json ⽂件，我们可以指定编译器的选项以 ⾃定义 TypeScript 编译的⾏为。 

放在项⽬的根⽬录 

tsconfig.json⽂件主要供 tsc 编译器使⽤ 

通过运⾏ 下⾯指令⽣成 tsconfig.json⽂件 

```txt
tsc --init 
```

以下是常⻅配置： 

compilerOptions （编译器选项）：这是 tsconfig.json ⽂件中最常⻅的选项之⼀。通过 compilerOptions，我们可以指定编译器的⾏为。常⻅的编译器选项包括： 

。 target （⽬标代码的 ECMAScript 版本）：指定编译后的 JavaScript 代码要符合的 ECMAScript 版本。 

module （模块系统）：指定 TypeScript 代码使⽤的模块系统，如 CommonJS、AMD、ES2015 等。 

。 outDir （输出⽬录）：指定编译后的 JavaScript ⽂件的输出⽬录。 

。 sourceMap （源码映射）：指定是否⽣成源码映射⽂件，以便在调试时能够⽅便地追踪 TypeScript 代 

码。 

strict （严格模式）：指定是否启⽤ TypeScript 的严格模式，以应⽤更严格的类型检查规则。 

include 和 exclude （包含和排除⽂件）：⽤于指定要包含和排除的⽂件，⽀持使⽤通配符模式。 

。 baseUrl 和 paths （模块解析）：⽤于配置 TypeScript 的模块解析规则，可以指定模块的基础路径和 路径别名。 

. include 和 exclude （包含和排除⽂件）：这两个选项⽤于指定要包含和排除的⽂件。在 include 中可以 使⽤通配符模式来匹配⽂件，⽽在 exclude 中可以指定要排除的⽂件或⽂件夹。 

. references （引⽤其他项⽬）：这个选项⽤于指定要引⽤的其他 TypeScript 项⽬。通过引⽤其他项⽬，我 们可以在⼀个项⽬中使⽤另⼀个项⽬的类型定义⽂件。 

. files （⽂件列表）：这个选项⽤于⼿动指定要编译的⽂件列表。如果指定了 files，则只会对这些⽂件进 ⾏编译，⽽不会考虑 include 和 exclude 。 

# 6.数组和元组

# 1.数组

ts 中规定，数组中所有成员的类型必须相同，数量可以不确定 

数组的声明语法： 

let arr:类型[ ]=[ ] 

或者 

let arr:Array<类型>=[ ] //这种写法本质上属于泛型 

使⽤案例 

//声明直接赋值 

let arr:string[]=["a","b"] 

//先声明再赋值 

let arr2:number[] 

let arr3:boolean[] 

arr2=[1,2,3] 

//后续添加的元素也必须符合数组的类型 

arr.push("8") 

//⼆维数组 

let arr4:string[][]=[["a","b"],["e","f"]] 

//对象数组 

let arr:{name:string,age:number}[]=[{name:"aa",age:18},{name:"bb",age:17}] 

//函数数组 

let arr: (()=>number)[]=[function(){return 123},function(){return 443}] 

//定义包含多个数据类型的数组 

let arr:(string|number|(()=>void))[]=["hh",123] 

# 2.元组

元组可以包含不同类型的元素，每个元素具有预定义的位置。需要为每⼀个元素规定类型 

let arr:[类型 1，类型 2]=[数据 1，数据 2] 

```txt
let arr:[string, number, boolean] = ["A", 123, true]
//先声明再赋值
let arr2:[boolean, string]

arr2[0] = true;
arr2[1] = "123"

//如果后续通过任何方法向数组添加数据，也可以，后添加的数据是前面每个数据的联合类型
arr2.push("123") 
```

# 7.对象

对于对象来说，我们应该限定对象中每个属性的类型，⽽不是限定对象这个整体的类型 

基本语法 

```txt
let obj: {name:string, age:number} = {name:"小明", age:18} 
```

案例 

```javascript
let a:{age:number,name:string}={name:"张三",age:18}
//先声明再赋值
let a2:{salary:number,address:string}
a2={salary:3500,address:"北京"}
//添加属性
a2.salary=4500 
```

# 1.可选属性

属性名后⾯加问号，代表该属性可有可⽆，⼀个对象中可以有任意多个可选属性 

```txt
let a:{name:string,age?:number}
a={name:"hello"} //正常执行，因为age可以不给 
```

# 2.任意属性

如果我只希望⼀个对象必须具有 a 属性，其他的属性有没有随意 

```txt
let a: {name:string,[propName:string]:any}
//propName代表属性名，肯定是字符串，propName只是形参，可以换成别的名字 任意属性只有一个没有多个，它代表了其他的所有属性
//任意属性的类型一定是其他类型（包含可选属性）的父类
a = {name:"张三",age:18,salary:3500} 
```

练习 

```typescript
let obj: {
    name:string,
    like:string[],
    pet:{name:string,age:number},
    [propName:string]:any
}

obj = {
    name:"张三",
    like:["唱歌","睡觉","打游戏"],
    pet:{name:"旺财",age:2},
    girlfriend:{name:"刘亦菲",
    age:18}
}

let obj: {
    run:(height:number)=>void
}

obj = {
    run:function(height:number){
    console.log("i can run")
    }
} 
```

# 3.只读属性

在对象属性的前⾯加上 readonly，代表该属性只能访问，不能修改，⼀个对象可以有任意多个只读属性 

```javascript
let obj: {readonly name:string,age:number}
obj = {name:"小红",age:18}
obj.age = 20; // 正确
obj.name="哈哈" // 错误，因为是只读的 
```

# 4.关于内置对象

```typescript
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div'); 
```

# 8.函数

# 1.基本使⽤

函数的类型声明，需要在声明函数时，给出参数的类型和返回值的类型。 

```txt
function fn(m:number,n:string):string{
    return n+m
} 
```

如果没有返回值，就是 void 类型 

```txt
function fn(m:number, n:string):void {
    console.log(m+n)
} 
```

通过变量赋值的形式声明函数 

```txt
let fn=function(m:number,n:string):void{console.log(m+n)}
//换成箭头函数的写法
let fn=(m:number,n:string):void=>{console.log(m+n)} 
```

单独定义函数类型 

```txt
let 变量名:(参数1: 类型, )=>返回值类型
let fn3:(a:number)=>number

// fn3=()=>{
    // console.log(123)
    // }
fn3=function(a:number):number{
    return 123
} 
```

事实上，上⾯的代码只对等号右侧的匿名函数进⾏了类型定义，⽽等号左边的 fn，是通过赋值操作进 ⾏类型推论⽽推断出来的。如果需要我们⼿动给 fn 添加类型，则应该是这样： 

```txt
let fn:(m:number,n:string)=>void=(m:number,n:string):void=>{console.log(m+n)} 
```

⼯作中声明函数，更推荐使⽤通过 function 声明的⽅式 

# 2.可选参数

如果某个参数是可选的，那么要在参数后⾯加问号 

⼀个函数可以有多个可选参数 

需要注意的是，可选参数必须接在必需参数后⾯。换句话说，可选参数后⾯不允许再出现必需参数了： 

```txt
function fn(m:number, n?:string):void {
    console.log(m+n)
}
fn(8) 
```

# 3.参数省略

函数的实际参数个数，可以少于类型指定的参数个数，但是不能多于，即 TypeScript 允许省略参数。 

```typescript
let fn:(a:number, b:number) => number;
fn = (a:number) => a; // 正确
fn = (a:number, b:number, c:number) => a; // 错误 
```

这是因为 JavaScript 函数在声明时往往有多余的参数，实际使⽤时可以只传⼊⼀部分参数。 

⽐如，数组的 forEach()⽅法的参数是⼀个函数，该函数默认有三个参数(item, index, array) => void，实际上 往往只使⽤第⼀个参数(item) => void。 

因此，TypeScript 允许函数传⼊的参数不⾜。 

# 4.rest 参数

rest 参数表示函数剩余的所有参数，它可以是数组（剩余参数类型相同），也可能是元组（剩余参数类型不同）。 

```typescript
// rest 参数为数组
function joinNumbers(...nums:number[]) {
    // ...
}
// rest 参数为元组
function f(...args:[boolean, number]) {
    // ...
} 
```

# 9.联合类型

联合类型（Union Types）表示取值可以为多种类型中的⼀种。 

```typescript
//联合类型使用 | 分隔每个类型。
var val:string|number |string[] | ()=>number
val = 12 //ok
val = "hello" //ok 
```

所以我们之前定义存储不同数据数组的时候，也可以使⽤联合类型 

```typescript
const arr: (number | string)[ ] = [1, "string", 2]; 
```

特殊的，如果访问联合类型的属性或⽅法： 

当 TypeScript 不确定⼀个联合类型的变量到底是哪个类型的时候 

我们只能访问此联合类型的所有类型⾥共有的属性或⽅法： 

```typescript
function getLength(something: string | number): number {
    return something.length;
}

// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
// Property 'length' does not exist on type 'number' 
```

访问 string 和 number 的共有属性是没问题的 

```typescript
function getString(something: string | number): string {
    return something.toString();
} 
```

联合类型的写法 

```txt
let a:string|number|string[]| {name:string,age:number} | ( )=>string);
a="hello";
a=123;
a=["a","b","c"]
a={name:"张三",age:18}
a=function(){
return "hello"
} 
```

# 10.类型别名

在 TypeScript 中，类型别名是给现有类型取⼀个新的名字。它可以⽤于提⾼代码的可读性和可维护性，以及 减少重复的类型定义。 

最基本的使⽤⽅式 

```txt
type MyString = string;
let a:MyString="hello" 
```

//type 关键字可以为原始值、联合类型、元组以及任何我们定义的类型起⼀个别名 

```typescript
type Add = (x: number, y: number) => number;
let add: Add = (arg1: string, arg2: string): string => arg1 + arg2; // error 不能
将类型“(arg1: string, arg2: string) => string”分配给类型“Add”
type Directions = 'up' | 'Down' | 'Left' | 'Right'
let toWhere : Directions = 'Down' 
```


对象数组使⽤类型别名


```typescript
const objArr: { name: string, age: Number }[] = [
{ name: "小明", age: 18 },
{ name: "小红", age: 28 },
];
//上面的案例使用类型别名简写
type Person = { name: string, age: Number };
const arr:Person[] = [
{ name: "小明", age: 18 },
{ name: "小红", age: 28 },
]; 
```


结合之前联合类型的写法


```txt
type A={name:string}
type B=number
type C=A|B|"哈哈"|string
let str:C=8 
```

# 11.类型推论

如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出⼀个类型。 

以下代码虽然没有指定类型，但是会在编译的时候报错： 

```typescript
let a=1;
a="hello" //error: Type 'string' is not assignable to type 'number'. 
```

如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型⽽完全不被类型检查： 

```javascript
let a;
a="hello"; //正确
a=7; //正确 
```

函数的参数的类型推论 

```javascript
function fn(x,y){ //x y被推断为any console.log(x+y) } 
```

# 12.typeof 和 keyof 运算符

# 1.typeof

JavaScript ⾥⾯， typeof 运算符只可能返回⼋种结果，⽽且都是字符串。 

```typescript
typeof undefined; // "undefined"
typeof true; // "boolean"
typeof 1337; // "number"
typeof "foo"; // "string"
typeof {}; // "object"
typeof parseInt; // "function"
typeof Symbol(); // "symbol"
typeof 127n // "bigint" 
```

TypeScript 将 typeof 运算符移植到了类型运算，它的操作数依然是⼀个值，但是返回的不是字符串，⽽是该值的 TypeScript 类型。 

```typescript
const a = { x: 0 };
type T0 = typeof a; // { x: number }
type T1 = typeof a.x; // number 
```

ts 中的 typeof 是 根据已有的值 来获取值的类型 来简化代码的书写 

```txt
const myVariable = 42;
type MyVariableType = typeof myVariable; // MyVariableType 的值为 "number" 
```

# 2.keyof

keyof 运算符接受⼀个对象类型作为参数，返回该对象的所有键名组成的联合类型。 

keyof 类型别名 

```txt
keyof {} 
```


练习


```typescript
type Person={name:string,age:number}
type MyType=keyof Person //name|age
//type MyType=keyof {name:string,age:number} //name|age
let a:MyType="name" 
```

# 13.映射类型

映射类型：基于旧类型创建新类型（对象类型），减少重复，提升开发效率。 

映射类型只能在类型别名中使⽤，不能在接⼝中使⽤。 

in 后⾯跟的是联合类型，也可以是通过 keyof⽣成的联合类型 

```typescript
//基本使用
type My="a" | "b" | "c"
type MyType={{key in My:number} // {a:number,b:number,c:number}
//配合keyof使用
type Props={a:number,b:string,c:boolean}
type MyType={{key in keyof Props}:Props[key]} // {a:number,b:string,c:boolean}
//此时的key不是关键字，可以随便命名
let obj:MyType={a:123,b:"2",c:true} 
```


配合可选属性使⽤


```typescript
type My="a" | "b" | "c"
type MyType={{key in My} // {a?:number, b?:number, c?:number}
let obj:MyType={a:123} 
```


配合只读属性使⽤


```txt
type My="a" | "b" | "c"
type MyType={readonly [key in My]:number} // {readonly a:number, readonly b:number, readonly c:number}
let obj:MyType={a:123,b:2,c:88}
obj.a=888 // 报错 
```

# 14.接⼝

# 1.基本使⽤

在 TypeScript 中，我们使⽤接⼝（Interfaces）来定义对象的类型。 

我们之前定义对象类型都是 形如{name:string,age:number}这种形式 

但是如果我的别的对象也是这种结构，我们不⾄于每个对象都重新声明⼀遍这个类型吧，所以就需要⽤到接⼝ 

接⼝其实就是相当于定义⼀个模板，以后声明的对象都得根据这个模板要求来 

```typescript
interface Person{
    name:string,
    age:number,
    salary:number
}
let obj:Person={name:"张三",age:18,salary:3500} 
```

接⼝中同样⽀持只读属性，可选属性,任意属性 

```typescript
interface Person{
    name:string,
    age:number,
    readonly salary:number,
    like:string[],
    run?:()=>void,
    [propName:string]:string|number|string[]||( ()=>void)
} 
```

# 2.接⼝的继承

如果你需要创建⼀个新的接⼝，⽽这个新的接⼝中的部分内容我已经在已存在的接⼝中定义过了，那么可以直接继 承，⽆需重复定义 

语法： 

```txt
interface 接口名 extends 另一个接口名 
```

只要存在接⼝的继承，那么我们实现接⼝的对象必须同时实现该接⼝以及他所继承的接⼝的所有属性 

```typescript
interface Person{
    name:string,
    age:number,
    address:string
}

interface Girl extends Person{
    height:number,
    hobby:string[]
}

interface Boy extends Person{
    salary:number,
    car:string
}

let xf:Girl={
    name:"小芳",
    age:18,
    address:"北京",
    height:170,
    hobby:["逛街","买买买"]
} 
```

⼀个接⼝可以被多个接⼝继承，同样，⼀个接⼝也可以继承多个接⼝，多个接⼝⽤逗号隔开 

继承多个接⼝，必须同时实现继承每⼀个接⼝定义的属性 

```typescript
interface Person{
    name:string,
    age:number,
    address:string
}
interface Pro{
phone:string,
coat:string
}

interface Girl extends Person,Pro{
    height:number,
    hobby:string[]
}

interface Boy extends Person{
    salary:number,
    car:string
}

let xf:Girl={
    name:"小芳",
    age:18,
    address:"北京",
    height:170,
    hobby:["逛街","买买买"],
    phone:"华为",
    coat:"安踏"
} 
```


多层继承:需要实现该接⼝以及所继承的接⼝和继承接⼝的接⼝


```typescript
interface Person{
    name:string,
    age:number,
    address:string
}

interface Girl extends Person{
    height:number,
    hobby:string[]
}

interface Xh extends Girl{
    hair:string
}

let xh:Xh={hair:"红色",height:170,hobby ["买买买"],name:"小红",age:18,address:"北京"} 
```

# 3.接⼝同名会合并

名字相同的接⼝不会冲突，⽽是会合并为⼀个 

```txt
interface Person{
    name:string,
    age:number,
    address:string
}

interface Person{
    salary:number
}

let xm:Person={name:"小明",age:17,address:"beijing",salary:3500} 
```

# 4.接⼝中使⽤联合类型

```typescript
interface Person{
    name:string,
    age:number,
    address:string
    hobby:string[] | ( ()=>void)
} 
```

# 5.接⼝也可以⽤于定义数组

但是不推荐，定义数组还是优先使⽤我们之前讲的⽅式 

```txt
interface MyArr{
    [index:number]:string
}
let arr:MyArr = ["@", "3"] 
```

# 6.接⼝也可以定义函数

```txt
interface MyArr{
    (a:number):number
}
let fn:MyArr=function(a:number){return 123} 
```

# 7.接⼝和类型别名的区别

interface 与 type 的区别有下⾯⼏点。 

（1） type 能够表示⾮对象类型，⽽ interface 只能表示对象类型（包括数组、函数等）。 

（2） interface 可以继承其他类型， type 不⽀持继承。 

（3）同名 interface 会⾃动合并，同名 type 则会报错 

（4） interface 不能包含属性映射（mapping）， type 可以 

```typescript
interface Point {
x: number;
y: number;
}

// 正确
type PointCopy1 = {
[Key in keyof Point]: Point[Key];
};

// 报错
interface PointCopy2 {
[Key in keyof Point]: Point[Key];
}; 
```

# 15.交叉类型

交叉类型（Intersection Types）⽤于组合多个类型，⽣成⼀个包含所有类型特性的新类型。可以 理解为将多个类型合并为⼀个更⼤的类型，新类型拥有所有原始类型的成员。使⽤ & 符号表示交 叉类型。 

```txt
type Person={name:string,age:number}
type Emp={salary:number,address:string}
type C=Person&Emp&{height:number}
let obj:C={name:"张三",age:18,salary:3500,address:"beijing",height:180} 
```

```txt
type C=string&number 
```

# 16.类型断⾔

# 1.基本⽤法

类型断⾔就是我明确的知道我这个数据肯定是字符串，告诉编译器你不⽤检测他了。 


语法：


```txt
值 as 类型
//或者
<类型>值
/*
在 tsx 语法（React 的 jsx 语法的 ts 版）中必须使用前者，即 值 as 类型。
形如 <Foo> 的语法在 tsx 中表示的是一个 ReactNode，在 ts 中除了表示类型断言之外，也可能是表示一个泛型。
故建议大家在使用类型断言时，统一使用 值 as 类型 这样的语法
*/ 
```

# 2..将⼀个联合类型断⾔为其中⼀个类型

当 TypeScript 不确定⼀个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型 中共有的属性或⽅法 

```txt
// type C=string|number
function fn(m:string|number){
    (m as string).substring(1)
}
fn(100)//错误 
```

注意：类型断⾔只能欺骗 ts 编译器，让他不报错，⽆法避免项⽬运⾏时的错误，所以使⽤断⾔要谨慎 

```txt
interface Boy{
    name:string,
    make:( )=>number 
```

```txt
}
interface Girl{
    name:string,
    cost: ()=>void
}
function fn(obj:Boy|Girl){
    (obj as Boy).make()
} 
```

案例 

```txt
let student = {} as {name: string}
student.name = "张三" 
```

# 3.将任何⼀个类型断⾔为 any

```txt
let num:number=1;
console.log((num as any).length) 
```

# 4.将 any 断⾔为任意类型

```typescript
let a:any=5;
console.log((a as number).length) // 报错 
```

# 5.将⽗类断⾔为⼦类

```javascript
class Students{
    make() {
    console.log("make")
    }
}
class Xm extends Students{
    run() {
    console.log("run")
    }
}
let a=new Students();
(a as Xm).run() //编译通过，运⾏报错
```

# 6.⾮空断⾔

```typescript
type MyFn=() => number
function fn(m:MyFn | undefined){
    let num = m!()
    let num2 = m() // 错误写法
} 
```

# 7.双重断⾔ (不推荐使⽤)

```typescript
interface Girl{
    name:string,
    cost: () => void
}

interface Boy{
    name:string,
    make: () => void
}

function fn(obj:Girl){
    obj as any as Boy
} 
```

# 17.enum 枚举类型

# 1.基本使⽤

枚举（enum）类型⽤于定义⼀些有名字的数字常量。当⼀个元素有固定的⼏个值可选时，可以使⽤Enum 来 改善代码的可读性和可维护性 

字符串存在数据库会⽐较⼤ 

我们常常会有这样的场景，⽐如与后端开发约定订单的状态开始是 0，未结账是 1，运输中是 2，运输完 成是 3，已收货是 4。这样的纯数字会使得代码缺乏可读性。枚举就⽤于这样的场景。枚举可以让我们定 义⼀些名字有意义的常量。使⽤枚举可以清晰地表达我们的意图。TypeScript⽀持基于数字枚举和字符 串的枚举。 

枚举类型默认第⼀项的值是 0，依次类推，每个+1 

```txt
enum OrderStatus{
Start = 1,
Unpaid,
Shipping,
Shipped,
Complete,
}
//取值语法 OrderStatus.Start 
```

当只写 Start = 1 时，后⾯的枚举变量就是递增的 

如果不写=1,那么默认初始值为 0，依次递增 

# 枚举类型解决了哪些问题

1.如果直接⽤数字，⽆法知道含义 

2.如果直接⽤字符，第⼀容易拼写错误，第⼆数据存起来太⼤ 

3.使⽤枚举类型更⽅便 

# 2.⼿动赋值

```javascript
enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};
console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true 
```

上⾯的例⼦中，未⼿动赋值的枚举项会接着上⼀个枚举项递增。 

如果未⼿动赋值的枚举项与⼿动赋值的重复了，TypeScript 是不会察觉到这⼀点的： 

```javascript
enum Days {Sun = 3, Mon = 1, Tue, Wed, Thu, Fri, Sat};
console.log(Days["Sun"] === 3); // true
console.log(Days["Wed"] === 3); // true
console.log(Days[3] === "Sun"); // false
console.log(Days[3] === "Wed"); // true 
```

⼿动赋值的实际使⽤场景，适合于⼀些对于数字有特殊要求的 

```typescript
enum Status {
    Success = 200,
    NotFound = 404,
    Error = 500
} 
```

# 3.使⽤计算值

数字枚举在定义值的时候，可以使⽤计算值和常量。但是要注意，如果某个字段使⽤了计算值或常 量，那么该字段后⾯紧接着的字段必须设置初始值，这⾥不能使⽤默认的递增值了， 

```javascript
const getValue = () => {
    return 0;
};
enum ErrorIndex {
    a = getValue(),
    b, // error 枚举成员必须具有初始化的值
    c
}
enum RightIndex {
    a = getValue(),
    b = 1,
    c
}
const Start = 1;
enum Index {
    a = Start,
    b, // error 枚举成员必须具有初始化的值
    c
} 
```

# 4.反向映射

我们定义⼀个枚举值的时候，可以通过 Enum[‘key’]或者 Enum.key 的形式获取到对应的值 

value。TypeScript 还⽀持反向映射，但是反向映射只⽀持数字枚举，我们后⾯要讲的字符串枚举是不⽀持的 

```javascript
enum Status {
    Success = 200,
    NotFound = 404,
    Error = 500
}
console.log(Status["Success"]); // 200
console.log(Status[200]); // 'Success'
console.log(Status[Status["Success"]]); // 'Success' 
```

# 5.字符串枚举

字符串枚举值要求每个字段的值都必须是字符串字⾯量，或者是该枚举值中另⼀个字符串枚举成员 

```txt
enum Message {
    Error = "Sorry, error",
    Success = "Hoho, success"
}
console.log(Message.Error); // 'Sorry, error' 
```

案例 

```txt
//订单
enum OrederStatus{
    Detail="/api/detail",
    List="/api/data/list",
    Owner="/user/owner",
} 
```

# 6.异构枚举 （不推荐）

简单来说异构枚举就是枚举值中成员值既有数字类型⼜有字符串类型，如下 : 

```typescript
enum Result {
    Faild = 0,
    Success = "Success"
} 
```

# 18.泛型

泛型（Generics）是指在定义函数、接⼝或类的时候，不预先指定具体的类型，⽽在使⽤的时候再指定 类型的⼀种特性 

简单来说泛型其实就是类型参数 

在定义的时候定义形参(类型变量) ，使⽤的时候传⼊实参(实际的类型) 

# 1.函数中使⽤泛型

```typescript
function identity<T>(arg: T): T {
return arg;
}
identity<Number>(100)
//或者identity(100) 在 TypeScript 中，在调用泛型函数时，如果没有显式地指定泛型类型参数，
//编译器会进行类型推断。根据传入的实际参数类型，编译器可以推断出泛型类型参数的类型，使得函数调用仍然是正确的 
```

```typescript
//多个类型参数
function identity<T,U>(arg: T,arg2 : U): T {
return arg;
}
identity<Number,String>(4,"hello") 
```

类型的推断不⼀定都是正确的，要谨慎使⽤，下⾯即是案例 

```txt
function fn2<T>(arr1:T[], arr2:T[]):T[] {
    return arr1.concat(arr2)
}
fn2([1,2],["a","b"])//错误
fn2<number|string>([1,2],["a","b"])//正确 
```

# 2.接⼝中使⽤泛型

```typescript
interface Person<N> {
    name: string;
    age: N;
}
const xiong: Person<string> = {
    name: "xiong",
    age: "18"
    // age: 18 // 报错
}; 
```

# 3.类中使⽤泛型

```typescript
class Person<T> {
    name:T,
    age:number
    constructor(name:T,age:number) {
    this.name = name
    this.age = age
    }
}
const xiong = new Person<string>("xiong",18)
console.log(xiong.name,xiong.age) // xiong 18 
```

泛型除了能使⽤基本类型 string number boolean 等，同时也可以是接⼝，数组等 


案例


```typescript
function fn<T>(a:T):T{
    return a
}

interface Person{
    name:string,
    age:number
}

type C="a" | "b" | "c"

fn<(()=>void)>( ()=>{console.log(1)})
fn<Person>( {name:"小明",age:18 })
fn<C>("b")
fn<string[ ]>(["a","b"]) 
```

泛型也可以⽤来定义数组 

let arr2:Array<number>=[2,3,4] //Array 是⼀个内置接⼝，接受⼀个 T 类型 

```typescript
interface Array<T> {
    /**
    * Returns the value of the first element 
```

# 4.类型别名中使⽤泛型

```txt
type C<T>={value:T}
let obj:C<string>={value:"hello"} 
```

# 5.多个类型参数

```txt
function fn<T,U>(arr:T[],f:(arg:T)=>U):U[ ] {
    return arr.map(f)
}

fn<string,number>(["1","2","3"], (item)=>parseInt(item)) 
```

# 6.类型参数默认值

```txt
function fn<T=string>(m:T){
    return m
}
fn(123)//正确 因为类型推断覆盖掉了默认类型 
```

类型参数默认值多⽤于 class 中 

```txt
class Person<T=string> {
    list:T[] = [];
    add(t:T) {
    this.list.push(t)
    }
}
let xm=new Person()
xm.add("4")//正确
xm.add(4)//错误 
```

# 7.泛型约束

在函数内部使⽤泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它的属性或⽅法： 

⼤类型 extends 更具体的类型 any extends number ⼤类型必须要包含后⾯更具体的类型 

```typescript
function loggingIdentity<T>(arg: T): T {
console.log(arg.length);
return arg;
}
// index.ts(2,19): error TS2339: Property 'length' does not exist on type 'T'. 
```

# 案例

```typescript
interface Person{
    name:string,
    age:number
}

function fn<T extends Person>(person:T){
}

fn<{name:string,age:number,salary:number>({name:"张三",age:18,salary:3500}) 
```

```javascript
let obj={a:1,b:2,c:3}
function fn<T extends keyof U ,U>(a:T,b:U){
}
fn("a",obj) 
```

# 8.关于泛型的嵌套

形如 <A<b>> 的嵌套写法表示在 TypeScript 中使⽤泛型进⾏多层嵌套。 

在泛型中，可以使⽤尖括号 < 和 > 将泛型参数括起来。当出现多层嵌套时，例如 <A<b>> ，它表示泛型类型 A 中的⼀个参数 b 是另⼀个泛型类型或带有泛型参数的类型。 

这种嵌套写法主要⽤于更复杂的数据结构和算法设计，它可以在泛型中引⽤其他泛型类型的参数，以实现更灵活和 抽象的泛型约束。 

```txt
interface Box<T> {
    item:T
}

interface Person<T> {
    name:T
}

let obj:Box<Person<string>>={
    item: {
    name:"hello"
    }
}

/////本质就是带泛型的类型整体看成一个类型
interface Box<T> {
    item:T
}

interface Person<T> {
    name:T
}

let obj:Box<Box<string>>={
    item: {
    item:"hello"
    }
} 
```

# 19.class 类

js 中的 class 

```javascript
class Parent {
    constructor(x) {
    this.x = x;
    this.sayHello = function () {
    console.log("sayHello")
    }
    }
    getX() {
    console.log("javax==>", this.x)
    }
} 
```


ts 中的 class


```javascript
//ts中class类构造函数里用到的所有属性，必须提前定义类型
class Person{
    //实例属性
    name:string="张三";//可以在定义的时候直接赋初始值
    age:number
    constructor(name,age){
    this.name=name;
    this.age=age
    }
    //类属性(静态属性) 只能通过类名访问和修改，对象实例访问不到
    static count:number=100
    //只读属性
    readonly sex:string="boy"
    eat(){
    console.log("我在吃饭")
    }
    //静态方法
    static sleep(){
    console.log("我睡觉呢")
    }
} 
```

# 1.关于继承

通过继承可以将多个类中共有的代码写在⼀个⽗类中 

这样就只需要写⼀次即可让所有的⼦类都同时拥有⽗类中的属性和⽅法 

如果⼦类和⽗类名字相同，则会覆盖掉⽗类⽅法（⽅法重写） 

```typescript
class Animal{
    name:string;
    age:number
    constructor(name,age){
    this.name=name;
    this.age=age
}
eat(){
    console.log("我在吃饭")
}
sleep(){
    console.log("我在睡觉")
}
class Dog extends Animal{
    gender:string
    constructor(name,age,gender){
    super(name,age)
    this.gender=gender
    }
    //方法的重写
    eat(){
    console.log("我在吃骨头")
    }
}

let wc=new Dog("旺财",2,"boy")
wc.eat() 
```

# 2.访问修饰符

TypeScript 可以使⽤三种访问修饰符（Access Modifiers），分别是 public、private 和 protected。 

public 修饰的属性或⽅法是公有的，可以在任何地⽅被访问到，默认所有的属性和⽅法都是 public 的 

private 修饰的属性或⽅法是私有的，不能在声明它的类的外部访问 

protected 修饰的属性或⽅法是受保护的，它和 private 类似，区别是它在当前类和⼦类中也是允许被访问的 

```typescript
class Animal{
    name:string; //默认就是public 没有使用限制
    protected age:number //只能在当前类和子类中用
    constructor(name,age){
    this.name=name;
    this.age=age
    }
    private eat(){ //只能在当前类中用
    console.log("我在吃饭")
    }
    sleep(){
    console.log("我在睡觉")
    } 
```

```txt
class Dog extends Animal{
    constructor(){
    super("a", 8)
    super.age
    }
} 
```

# 3.抽象类

抽象⽅法：就是没有⽅法体的⽅法 

抽象⽅法只能出现在抽象类中 ，包含了抽象⽅法的类，就⼀定是抽象类 

抽象类中的抽象⽅法必须被⼦类（可以是间接⼦类）实现的。 

抽象类中不⼀定包含抽象⽅法 

```typescript
abstract class Animal{
    abstract eat()
    abstract fn()
    sleep() {
    console.log(1)
    }
}

abstract class Cat extends Animal {

    abstract class Mimi {

} 
```

# 4.implements 关键字

在 TypeScript 中， implements 关键字⽤于检查⼀个类是否遵循特定的接⼝。接⼝（Interface）在 TypeScript 中是 ⼀个⾮常强⼤的⼯具，它描述了⼀组⽅法和属性的形状（Shape），但没有实现它们。 

⼀个类可以同时实现多个接⼝ 

```typescript
interface Person{
    id:number,
    name:string,
    play:( )=>void
}
interface Person1{
    gender:boolean
}

class XiaoMing implements Person,Person1{
    id:number
    name:string
    play:( )=>void
    gender:boolean
    age:number
    constructor(a:number,b:string,c:( )=>void){
    this.id=a;
    this.name=b;
    this.play
    }
} 
```

# 20.模块

ts 模块化是建⽴在 es6 模块化的基础上，与 JS 中的写法有许多的不同之处 

任何包含 import 或 export 语句的⽂件，就是⼀个模块（module）。相应地，如果⽂件不包含 export 语 句，就是⼀个全局的脚本⽂件。 

模块本身就是⼀个作⽤域，不属于全局作⽤域。模块内部的变量、函数、类只在内部可⻅，对于模块外部是 不可⻅的。暴露给外部的接⼝，必须⽤ export 命令声明；如果其他⽂件要使⽤模块的接⼝，必须⽤ import 命令来输⼊。 

如果⼀个⽂件不包含 export 语句，但是希望把它当作⼀个模块（即内部变量对外不可⻅），可以在脚本头部 添加⼀⾏语句。 

```typescript
export {}; 
```

# 1.基础的导⼊导出

导出 

```javascript
export let a=1;
export const b=function(){
    console.log(1)
}
let c="hello"
export default c 
```

引⼊ 

```javascript
import c, { a,b } from "./a";
console.log(a,b,c) 
```

# 2.类型导出

类型导出⼀般是复杂的类型，不要导出基本简单的类型 

对于接⼝来说，普通导出和默认导出语法后⾯⼀致 

类型的导出: 

```javascript
export {接口名, type名} 
```

```typescript
interface Person2{
    name:string,
    age:number
}
type obj={gender:boolean,like:string[]}
export type {obj,Person2} //方式1
export {obj,Person2}//方式2
export {type obj,type Person2}///方式3 
```

导出或者引⼊的时候加⼊type 关键字的好处 

1.减少编译后输出⽂件的⼤⼩ 

2.避免不必要的运⾏开销 

3.明确表达意图 

# 3.类型引⼊

```typescript
import type { obj, Person2, Person } from "./a";//方式1
import { obj, Person2, Person } from "./a";//方式2
import {type obj, type Person2, type Person } from "./a";//方式1 
```

# 4.关于接⼝的默认导出

```typescript
export default interface Person{
    name:string,
    age:number
}

export interface Person{
    name:string,
    age:number
}

//以上两种写法都是对的 
```

# 5.重导出

```typescript
export type{Person} from "./a"
export * from "./b" //重导出所有 
```

重导出的作⽤就是，当你的⽂件中需要⽤到好多模块定义的类型，那么可以单独定义⼀个⽂件，把这些模板进⾏重 导出，那么我们的项⽬⽂件只需要引⼊这⼀个重导出⽂件就可以了 

# 21.namespace

在 TypeScript 中，namespace 是⼀个早期的概念，⽤于提供命名空间功能，帮助组织代码和防⽌名称冲突。随着 ECMAScript 2015（ES6）模块的引⼊，namespace 的使⽤减少了，因为 ES6 模块提供了更好的代码组织机制。 不过，namespace 在⼀些特定场景下（特别是在没有模块系统的全局脚本环境中）还是有它的⽤武之地。 

官⽅也不再推荐使⽤namespace 了 

```typescript
namespace Utils{
    export function fn(){ //如果不加export,只能在该命名空间内使用
    console.log(1)
    }
    export var a=1
}
//使用命名空间内的函数/变量
Utils.fn() 
```

命名空间可以嵌套,可以定义数据，函数，类型等 

```typescript
namespace My{
    export namespace Girlfriend{
    export function angry(){
    console.log("爱生气")
    }
    }

    export interface Person {name:string}
}

let c:My.Person={} 
```

本质上作⽤跟 es6 模块化是⼀样的。 

# 22.declare 关键字

declare 关键字⽤来告诉编译器，某个类型是存在的，可以在当前⽂件中使⽤。 

它的主要作⽤，就是让当前⽂件可以使⽤其他⽂件声明的类型。举例来说，⾃⼰的脚本使⽤外部库定义的函数，编 译器会因为不知道外部函数的类型定义⽽报错，这时就可以在⾃⼰的脚本⾥⾯使⽤declare 关键字，告诉编译器外 部函数的类型。这样的话，编译单个脚本就不会因为使⽤了外部类型⽽报错。 

declare 关键字的重要特点是，它只是通知编译器某个类型是存在的，不⽤给出具体实现。⽐如，只描述函数的类 型，不给出函数的实现，如果不使⽤declare，这是做不到的。 

declare 只能⽤来描述已经存在的变量和数据结构，不能⽤来声明新的变量和数据结构。另外，所有 declare 语句 都不会出现在编译后的⽂件⾥⾯。 


.d.ts


```typescript
declare function fn(x:number,y:number):number
declare let a:number 
```

```typescript
declare class Person{
    name:string
}

interface Man{
    make: () => void,
    gender:boolean
}

declare let xm:Man 
```

# 1.关于.d.ts⽂件的加载机制

TypeScript 中的 .d.ts ⽂件是类型声明⽂件，它们⽤来为 TypeScript 提供有关 JavaScript 代码结构的类型信息 在 TypeScript 中， .d.ts ⽂件的引⼊机制有以下⼏种情况 

# 1.同名引⼊

对于你⾃⼰写的模块，TypeScript 会默认引⼊与该⽂件同名的类型声明⽂件。例如，如果你有⼀个 foo.js ⽂件和⼀个同⽬录下的 foo.d.ts ⽂件，当你在 TypeScript 中 import 该模块时，类型信息将由 foo.d.ts 提供。 

# 2.⾃动引⼊

对于 npm 安装的第三⽅库如 lodash，如果你安装了对应的类型声明（例如通过 npm install @types/lodash ），TypeScript 会⾃动识别这些 node_modules/@types 的声明⽂件，⽆需显式引⼊它 们。 

可以通过配置更改 

```txt
typeRoots设置类型模块所在的目录，默认是node_modules/@types 
```

该⽬录⾥⾯的模块会⾃动加⼊编译。⼀旦指定了该属性，就不会再⽤默认值 node_modules/@types⾥⾯的类 型模块。 

该属性的值是⼀个数组，数组的每个成员就是⼀个⽬录，它们的路径是相对于 tsconfig.json 位置。 

```json
{
    "compilerOptions": {
    "typeRoots": ["/typings", "/vendor/types"]
    }
} 
```

# 3.通过 tsconfig.json 配置⾃动加载

include 属性是⽤来指明 TypeScript 编译器应该包含哪些⽂件的 

```txt
{
    ...
    "include": [
    "src/**/*"
    ]
}
// src/**/* 表示包含 src 目录下的所有文件和子目录中的所有文件，无论文件的扩展名是什么，都应该纳入 TypeScript 编译的范围 
```

"src/*/"：这是⼀个 glob 模式，其中： 

src/ 表示源代码位于项⽬根⽬录下的 src ⽬录中。 

** 表示任意数量的⼦⽬录（包括零个），这是⼀个通配符。 

表示任意数量的字符（不含路径分隔符），也是⼀个通配符。 

如果 tsconfig.json 不做任何特殊设置，默认会加载所有的.d.ts⽂件包括根⽬录下和任何⽂件夹内 

# 4.三斜线指令

你可以在⽂件顶部添加如下指令来显式告诉 TypeScript 引⼊特定的 .d.ts 

⽂件： 

```typescript
/// <reference path="my-declaration-file.d.ts" /> 
```

这种⽅式并不推荐⽤于模块化的代码，适⽤于全局脚本的场景。 

# 2.第三⽅库

让我们全都⾃⼰加上类型，这显然不现实 

第⼀：很多第三⽅库默认都⾃带类型声明⽂件，这种就不⽤我们管了 

第⼆：如果没有⾃带，ts 社区基本上也提供了他们的类型⽂件，TypeScript 社区主要使⽤ DefinitelyTyped 仓库， 各种类型声明⽂件都会提交到那⾥，已经包含了⼏千个第三⽅库。 

这些声明⽂件都会作为⼀个单独的库，发布到 npm 的@types 名称空间之下。⽐如，jQuery 的类型声明⽂件就发 布成@types/jquery 这个库，使⽤时安装这个库就可以了。 

```bash
$ npm install @types/jquery --save-dev 
```

执⾏上⾯的命令， @types/jquery 这个库就安装到项⽬的 node_modules/@types/jquery ⽬录，⾥⾯的 index.d.ts ⽂件就是 jQuery 的类型声明⽂件 

# 3.declare module,declare namespace

# declare module

declare module ⽤于声明外部模块的类型。在使⽤第三⽅模块（⽐如某个 npm 包）时，你可以使⽤这种⽅ 法来定义模块的类型声明。通常，你会在类型声明⽂件（.d.ts ⽂件）中使⽤ declare module。 

是⽤来定义模块的类型声明，并且这种声明是可以被导⼊的。这通常适⽤于⽂件模块。在 ES6 模块系统中， 任何包含顶层 import 或 export 的⽂件都被视为⼀个模块 

# ts 中 declare module 这个 module 指的是哪种⽂件？

在 TypeScript 中， declare module 是⽤来声明外部模块的。这个"模块"指的是通过 import / require 等⽅式 引⼊的⽂件或者库。这可能是⼀个 npm 库，如 lodash 或 jQuery，或者是你的项⽬中的另⼀个 JavaScript/TypeScript ⽂件。 

这个 declare module 'jquery' ⾥的 'jquery' 就是所谓的"模块"。这⾥是⼀个字符串，可以写任何你希望的 模块名。这个字符串和你在代码中 import / require 的模块名全部匹配，如你写 import $ from 'jquery' ， 那么 TypeScript 就会去找 declare module 'jquery' ⾥有没有对应的类型声明。 

# declare namespace

declare namespace ⽤于声明全局代码的命名空间。⽐如，你有⼀些全局变量或全局库，你可以使⽤命名空 间来包围它们的类型声明。这适⽤于脚本环境，⽽不是模块环境。 

是⽤来定义全局变量的命名空间，⽽这些变量可能是由外部脚本或全局库引⼊的。它的使⽤场景⼀般是在没 有模块概念的全局代码（在⽹⻚中直接通过<script>标签引⼊的脚本） 

# 4.declare 可以 为外部模块添加属性和⽅法时，给出新增部分的 类型描述

下⾯是另⼀个例⼦。⼀个项⽬有多个模块，可以在⼀个模块中，对另⼀个模块的接⼝进⾏类型扩展。 

因为同名 interface 会⾃动合并类型声明。 

```typescript
import { A } from "./index";

declare module "./index" {
    interface A{
    y:number
    }
}

let count: A = { x: 5, y: 2 } 
```

# 5.declare module 描述的模块名可以使⽤通配符

```typescript
declare module 'my-plugin-*' {
    interface PluginOptions {
    enabled: boolean;
    priority: number;
    }

    function initialize(options: PluginOptions): void;
    export = initialize;
} 
```

上⾯示例中，模块名 my-plugin-* 表示适配所有以 my-plugin- 开头的模块名（⽐如 my-plugin-logger ）。 

# 6.三斜杠命令

三斜杠命令（///）是⼀个 TypeScript 编译器命令，⽤来指定编译器⾏为。它只能⽤在⽂件的头部，如果⽤在其他 地⽅，会被当作普通的注释 

最常⻅的三斜杠指令有以下两种： 

1. /// <reference path="..." /> ：⽤来指定要引⽤的⽂件路径，告诉编译器要在编译过程中包含这个⽂ 件。 

2. /// <reference types="..." /> ：⽤来声明对某个包的类型依赖，通常⽤于从 @types 包中引⽤声明。 

# 示例

# reference path

使⽤ /// <reference path="..." /> 注释来告诉 TypeScript 编译器引⼊⼀个额外的⽂件： 

```typescript
/// <reference path="some-library.d.ts" />
import * as someLibrary from 'some-library'; 
```

在该示例中， some-library.d.ts 包含了 'some-library' 模块的类型声明。当 TypeScript 编译器处理到该⽂ 件时，它将会同时考虑 some-library.d.ts 中的声明。 

这种⽅式最常⻅于在编写声明⽂件的时候，以保证在声明⽂件之间存在引⽤关系。 

# reference types

使⽤ /// <reference types="..." /> 当你希望从 @types 包或者其他类型声明⽂件中引⽤⼀套类型声明，且 在编译时期你并不想或不需要引⽤它的实际实现： 

```typescript
/// <reference types="node" />
import * as fs from 'fs'; 
```

在该示例中，我们通过声明 node 来让 TypeScript 知道 fs 这个名字指的是 Node.js 中的⽂件系统模块。 

# 注意事项

TypeScript 团队⿎励开发者尽量使⽤ES6⻛格的 import 语句来加载模块和类型，因为这是解析模块路径的标准⽅ 式。三斜杠指令应该只在严格需要的时候使⽤，⽐如当使⽤旧的或⾮模块化的代码时。 

随着 TypeScript 的演进和模块解析算法的改进，⼤多数情况下不再需要三斜杠指令 

# 23.tsconfig.json⽂件详细配置

# exclude

exclude 属性是⼀个数组，必须与 include 属性⼀起使⽤，⽤来从编译列表中去除指定的⽂件。它也⽀持使⽤ 与 include 属性相同的通配符。 

```json
{
    "include": ["**/*"], 
    "exclude": ["**/*.spec.ts"]
} 
```

# extends

tsconfig.json 可以继承另⼀个 tsconfig.json⽂件的配置。如果⼀个项⽬有多个配置，可以把共同的配置写 成 tsconfig.base.json，其他的配置⽂件继承该⽂件，这样便于维护和修改。 

extends 属性⽤来指定所要继承的配置⽂件。它可以是本地⽂件。 

```json
{
    "extends": "../tsconfig.base.json"
} 
```

如果 extends 属性指定的路径不是以 ./ 或 ../ 开头，那么编译器将在 node_modules ⽬录下查找指定的配置⽂ 件。 

extends 属性也可以继承已发布的 npm 模块⾥⾯的 tsconfig ⽂件。 

```json
{
    "extends": "@tsconfig/node12/tsconfig.json"
} 
```

extends 指定的 tsconfig.json 会先加载，然后加载当前的 tsconfig.json 。如果两者有重名的属性，后者会 覆盖前者。 

# files

files 属性指定编译的⽂件列表，如果其中有⼀个⽂件不存在，就会报错。 

它是⼀个数组，排在前⾯的⽂件先编译。 

```json
{
    "files": ["a.ts", "b.ts"]
} 
```

该属性必须逐⼀列出⽂件，不⽀持⽂件匹配。如果⽂件较多，建议使⽤include 和 exclude 属性。 

# include

include 属性指定所要编译的⽂件列表，既⽀持逐⼀列出⽂件，也⽀持通配符。⽂件位置相对于当前配置⽂件⽽ 定。 

```json
{
    "include": ["src/**/*", "tests/**/*"]
} 
```

include 属性⽀持三种通配符。 

?：指代单个字符 

*：指代任意字符，不含路径分隔符 

**：指定任意⽬录层级。 

如果不指定⽂件后缀名，默认包括 .ts 、 .tsx 和 .d.ts ⽂件。如果打开了 allowJs ，那么还包括 .js 和 .jsx 。 

# references

references 属性是⼀个数组，数组成员为对象，适合⼀个⼤项⽬由许多⼩项⽬构成的情况，⽤来设置需要引⽤的 底层项⽬。 

```json
{
    "references": [
    { "path": "../pkg1" },
    { "path": "../pkg2/tsconfig.json" }
]
} 
```

references 数组成员对象的 path 属性，既可以是含有⽂件 tsconfig.json 的⽬录，也可以直接是该⽂件。 

与此同时，引⽤的底层项⽬的 tsconfig.json 必须启⽤ composite 属性。 

```json
{
    "compilerOptions": {
    "composite": true
    }
} 
```

# compilerOptions

compilerOptions 属性⽤来定制编译⾏为。这个属性可以省略，这时编译器将使⽤默认设置。 

# allowJs

allowJs 允许 TypeScript 项⽬加载 JS 脚本。编译时，也会将 JS ⽂件，⼀起拷⻉到输出⽬录。 

```json
{
    "compilerOptions": {
    "allowJs": true
    }
} 
```

# alwaysStrict

alwaysStrict 确保脚本以 ECMAScript 严格模式进⾏解析，因此脚本头部不⽤写 "use strict" 。它的值是⼀个 布尔值，默认为 true 。 

# allowSyntheticDefaultImports

allowSyntheticDefaultImports 允许 import 命令默认加载没有 default 输出的模块。 

⽐如，打开这个设置，就可以写 import React from "react"; ，⽽不是 import * as React from "react"; 。 

# allowUnreachableCode

allowUnreachableCode 设置是否允许存在不可能执⾏到的代码。它的值有三种可能。 

undefined： 默认值，编辑器显示警告。 

true：忽略不可能执⾏到的代码。 

false ：编译器报错。 

# allowUnusedLabels

allowUnusedLabels 设置是否允许存在没有⽤到的代码标签（label）。它的值有三种可能。 

undefined： 默认值，编辑器显示警告。 

true：忽略没有⽤到的代码标签。 

false：编译器报错。 

# baseUrl

baseUrl 的值为字符串，指定 TypeScript 项⽬的基准⽬录。 

由于默认是以 tsconfig.json 的位置作为基准⽬录，所以⼀般情况不需要使⽤该属性。 

```json
{
    "compilerOptions": {
    "baseUrl": "./"
    }
} 
```

上⾯示例中，baseUrl 为当前⽬录./。那么，当遇到下⾯的语句，TypeScript 将以./为起点，寻 找 hello/world.ts 。 

```typescript
import { helloWorld } from "hello/world"; 
```

# checkJs

checkJS 设置对 JS ⽂件同样进⾏类型检查。打开这个属性，也会⾃动打开 allowJs。它等同于在 JS 脚本的头部添 加 // @ts-check 命令。 

```json
{
    "compilerOptions": {
    "checkJs": true
    }
} 
```

# composite

composite 打开某些设置，使得 TypeScript 项⽬可以进⾏增量构建，往往跟 incremental 属性配合使⽤。 

# declaration

declaration 设置编译时是否为每个脚本⽣成类型声明⽂件 .d.ts 。 

```json
{
    "compilerOptions": {
    "declaration": true
    }
} 
```

# declarationDir

declarationDir 设置⽣成的 .d.ts ⽂件所在的⽬录。 

```json
{
    "compilerOptions": {
    "declaration": true,
    "declarationDir": "./types"
    }
} 
```

# declarationMap

declarationMap 设置⽣成 .d.ts 类型声明⽂件的同时，还会⽣成对应的 Source Map ⽂件。 

```json
{
    "compilerOptions": {
    "declaration": true,
    "declarationMap": true
    }
} 
```

# emitBOM

emitBOM 设置是否在编译结果的⽂件头添加字节顺序标志 BOM，默认值是 false 。 

# emitDeclarationOnly

emitDeclarationOnly 设置编译后只⽣成 .d.ts ⽂件，不⽣成 .js ⽂件。 

# esModuleInterop

esModuleInterop 修复了⼀些 CommonJS 和 ES6 模块之间的兼容性问题。 

如果 module 属性为 node16 或 nodenext ，则 esModuleInterop 默认为 true ，其他情况默认为 false 。 

打开这个属性，使⽤import 命令加载 CommonJS 模块时，TypeScript 会严格检查兼容性问题是否存在。 

```javascript
import * as moment from 'moment'
moment(); // 报错 
```

上⾯示例中，根据 ES6 规范， import * as moment ⾥⾯的 moment 是⼀个对象，不能当作函数调⽤，所以第⼆ ⾏报错了。 

解决⽅法就是改写上⾯的语句，改成加载默认接⼝。 

```txt
import moment from 'moment'
moment(); // 不报错 
```

打开 esModuleInterop 以后，如果将上⾯的代码编译成 CommonJS 模块格式，就会加⼊⼀些辅助函数，保证编 译后的代码⾏为正确。 

注意，打开 esModuleInterop ，将⾃动打开 allowSyntheticDefaultImports 。 

# exactOptionalPropertyTypes

exactOptionalPropertyTypes 设置可选属性不能赋值为 undefined 。 

```txt
// 打开 exactOptionalPropertyTypes
interface MyObj {
    foo?: 'A' | 'B';
}
let obj:MyObj = { foo: 'A' };
obj.foo = undefined; // 报错 
```

上⾯示例中， foo 是可选属性，打开 exactOptionalPropertyTypes 以后，该属性就不能显式赋值 为 undefined 。 

# forceConsistentCasingInFileNames

forceConsistentCasingInFileNames 设置⽂件名是否为⼤⼩写敏感，默认为 true 。 

# incremental

incremental 让 TypeScript 项⽬构建时产⽣⽂件 tsbuildinfo ，从⽽完成增量构建。 

# inlineSourceMap

inlineSourceMap 设置将 SourceMap ⽂件写⼊编译后的 JS ⽂件中，否则会单独⽣成⼀个 .js.map ⽂件。 

# inlineSources

inlineSources 设置将原始的 .ts 代码嵌⼊编译后的 JS 中。 

它要求 sourceMap 或 inlineSourceMap ⾄少打开⼀个。 

# isolatedModules

isolatedModules 设置如果当前 TypeScript 脚本作为单个模块编译，是否会因为缺少其他脚本的类型信息⽽报 错，主要便于⾮官⽅的编译⼯具（⽐如 Babel）正确编译单个脚本。 

# jsx

jsx 设置如何处理.tsx⽂件。它可以取以下五个值。 

preserve ：保持 jsx 语法不变，输出的⽂件名为 .jsx 。 

react ：将 <div /> 编译成 React.createElement("div") ，输出的⽂件名为 .js 。 

react-native ：保持 jsx 语法不变，输出的⽂件后缀名为 .js 。 

react-jsx ：将 <div /> 编译成 _jsx("div") ，输出的⽂件名为 .js 。 

react-jsxdev ：跟 react-jsx 类似，但是为 _jsx() 加上更多的开发调试项，输出的⽂件名为 .js 。 

```json
{
    "compilerOptions": {
    "jsx": "preserve"
    }
} 
```

# lib

lib 值是⼀个数组，描述项⽬需要加载的 TypeScript 内置类型描述⽂件，跟三斜线指令 /// <reference lib="" /> 作⽤相同。 

```json
{
    "compilerOptions": {
    "lib": ["dom", "es2021"]
    }
} 
```

TypeScript 内置的类型描述⽂件，主要有以下⼀些，完整的清单可以参考 TypeScript 源码。 

ES2015 

ES6 

ES2016 

ES7 

ES2017 

ES2018 

ES2019 

ES2020 

ES2021 

ES2022 

. ESNext 

. DOM 

WebWorker 

ScriptHost 

# listEmittedFiles

listEmittedFiles 设置编译时在终端显示，⽣成了哪些⽂件。 

```json
{
    "compilerOptions": {
    "listEmittedFiles": true
    }
} 
```

# listFiles

listFiles 设置编译时在终端显示，参与本次编译的⽂件列表。 

```json
{
    "compilerOptions": {
    "listFiles": true
    }
} 
```

# mapRoot

mapRoot 指定 SourceMap ⽂件的位置，⽽不是默认的⽣成位置。 

```json
{
    "compilerOptions": {
    "sourceMap": true,
    "mapRoot": "https://my-website.com/debug/sourcemaps/"
    }
} 
```

# module

module 指定编译产物的模块格式。它的默认值与 target 属性有关，如果 target 是 ES3 或 ES5 ，它的默认值 是 commonjs ，否则就是 ES6/ES2015 。 

```json
{
    "compilerOptions": {
    "module": "commonjs"
    }
} 
```

它可以取以下值：none、commonjs、amd、umd、system、es6/es2015、es2020、es2022、esnext、 node16、nodenext。 

# moduleResolution

moduleResolution 确定模块路径的算法，即如何查找模块。它可以取以下四种值。 

node ：采⽤ Node.js 的 CommonJS 模块算法。 

node16 或 nodenext ：采⽤ Node.js 的 ECMAScript 模块算法，从 TypeScript 4.7 开始⽀持。 

classic ：TypeScript 1.6 之前的算法，新项⽬不建议使⽤。 

bundler ：TypeScript 5.0 新增的选项，表示当前代码会被其他打包器（⽐如 Webpack、Vite、esbuild、 Parcel、rollup、swc）处理，从⽽放宽加载规则，它要求 module 设为 es2015 或更⾼版本，详⻅加⼊该功能 的 PR 说明。 

它的默认值与 module 属性有关，如果 module 为 AMD 、 UMD 、 System 或 ES6/ES2015 ，默认值为 classic ；如 果 module 为 node16 或 nodenext ，默认值为这两个值；其他情况下,默认值为 Node 。 

# moduleSuffixes

moduleSuffixes 指定模块的后缀名。 

```json
{
    "compilerOptions": {
    "moduleSuffixes": [".ios", ".native", ""]
    }
} 
```

上⾯的设置使得 TypeScript 对于语句 import * as foo from "./foo"; ，会搜索以下脚 本 ./foo.ios.ts 、 ./foo.native.ts 和 ./foo.ts 。 

# newLine

newLine 设置换⾏符为 CRLF （Windows）还是 LF （Linux）。 

# noEmit

noEmit 设置是否产⽣编译结果。如果不⽣成，TypeScript 编译就纯粹作为类型检查了。 

# noEmitHelpers

noEmitHelpers 设置在编译结果⽂件不插⼊ TypeScript 辅助函数，⽽是通过外部引⼊辅助函数来解决，⽐如 NPM 模块 tslib 。 

# noEmitOnError

noEmitOnError 指定⼀旦编译报错，就不⽣成编译产物，默认为 false 。 

# noFallthroughCasesInSwitch

noFallthroughCasesInSwitch 设置是否对没有 break 语句（或者 return 和 throw 语句）的 switch 分⽀报 错，即 case 代码⾥⾯必须有终结语句（⽐如 break）。 

# noImplicitAny

noImplicitAny 设置当⼀个表达式没有明确的类型描述、且编译器⽆法推断出具体类型时，是否允许将它推断 为 any 类型。 

它是⼀个布尔值，默认为 true ，即只要推断出 any 类型就报错。 

# noImplicitReturns

noImplicitReturns 设置是否要求函数任何情况下都必须返回⼀个值，即函数必须有 return 语句。 

# noImplicitThis

noImplicitThis 设置如果 this 被推断为 any 类型是否报错。 

# noUnusedLocals

noUnusedLocals 设置是否允许未使⽤的局部变量。 

# noUnusedParameters

noUnusedParameters 设置是否允许未使⽤的函数参数。 

# outDir

outDir 指定编译产物的存放⽬录。如果不指定，编译出来的.js⽂件存放在对应的.ts⽂件的相同位置。 

# outFile

outFile 设置将所有⾮模块的全局⽂件，编译在同⼀个⽂件⾥⾯。它只有在 module 属性 为 None 、 System 、 AMD 时才⽣效，并且不能⽤来打包 CommonJS 或 ES6 模块。 

# paths

paths 设置模块名和模块路径的映射，也就是 TypeScript 如何导⼊ require 或 imports 语句加载的模块。 

paths 基于 baseUrl 进⾏加载，所以必须同时设置后者。 

```json
{
    "compilerOptions": {
    "baseUrl": "./",
    "paths": {
    "b": ["bar/b"]
    }
    }
} 
```

它还可以使⽤通配符“*”。 

```json
{
    "compilerOptions": {
    "baseUrl": "./",
    "paths": {
    "@bar/*": ["bar/*"]
    }
    }
} 
```

# preserveConstEnums

preserveConstEnums 将 const enum 结构保留下来，不替换成常量值。 

```json
{
    "compilerOptions": {
    "preserveConstEnums": true
    }
} 
```

# pretty

pretty 设置美化输出终端的编译信息，默认为 true 。 

# removeComments

removeComments 移除 TypeScript 脚本⾥⾯的注释，默认为 false 。 

# resolveJsonModule

resolveJsonModule 允许 import 命令导⼊ JSON ⽂件。 

# rootDir

rootDir 设置源码脚本所在的⽬录，主要跟编译后的脚本结构有关。rootDir 对应⽬录下的所有脚本，会成为输 出⽬录⾥⾯的顶层脚本。 

# rootDirs

rootDirs 把多个不同⽬录，合并成⼀个虚拟⽬录，便于模块定位。 

```json
{
    "compilerOptions": {
    "rootDirs": ["bar", "foo"]
    }
} 
```

上⾯示例中，rootDirs 将 bar 和 foo 组成⼀个虚拟⽬录。 

# sourceMap

sourceMap 设置编译时是否⽣成 SourceMap ⽂件。 

# sourceRoot

sourceRoot 在 SourceMap ⾥⾯设置 TypeScript 源⽂件的位置。 

```json
{
    "compilerOptions": {
    "sourceMap": true,
    "sourceRoot": "https://my-website.com/debug/source/" 
    }
} 
```

# strict

strict ⽤来打开 TypeScript 的严格检查。它的值是⼀个布尔值，默认是关闭的。 

```json
{
    "compilerOptions": {
    "strict": true
    }
} 
```

这个设置相当于同时打开以下的⼀系列设置。 

alwaysStrict 

strictNullChecks 

strictBindCallApply 

strictFunctionTypes 

strictPropertyInitialization 

noImplicitAny 

noImplicitThis 

useUnknownInCatchVariables 

打开 strict 的时候，允许单独关闭其中⼀项。 

```json
{
    "compilerOptions": {
    "strict": true,
    "alwaysStrict": false
    }
} 
```

# strictBindCallApply

strictBindCallApply 设置是否对函数的 call() 、 bind() 、 apply() 这三个⽅法进⾏类型检查。 

如果不打开 strictBindCallApply 编译选项，编译器不会对以上三个⽅法进⾏类型检查，参数类型都是 any，传 ⼊任何参数都不会产⽣编译错误。 

```txt
function fn(x: string) {
    return parseInt(x);
}

// strictBindCallApply:false
const n = fn.call(undefined, false);
// 以上不报错 
```

# strictFunctionTypes

strictFunctionTypes 允许对函数更严格的参数检查。具体来说，如果函数 B 的参数是函数 A 参数的⼦类型，那 么函数 B 不能替代函数 A。 

```txt
function fn(x:string) {
    console.log('Hello, ' + x.toLowerCase());
}

type StringOrNumberFunc = (ns:string|number) => void;

// 打开 strictFunctionTypes，下面代码会报错
let func:StringOrNumberFunc = fn; 
```

上⾯示例中，函数 fn() 的参数是 StringOrNumberFunc 参数的⼦集，因此 fn 不能替代 StringOrNumberFunc 。 

# strictNullChecks

strictNullChecks 设置对 null 和 undefined 进⾏严格类型检查。如果打开 strict 属性，这⼀项就会⾃动设 为 true ，否则为 false 。 

```txt
let value:string;
// strictNullChecks:false
// 下面语句不报错
value = null; 
```

它可以理解成只要打开，就需要显式检查 null 或 undefined 。 

```javascript
function doSomething(x:string|null) {
    if (x === null) {
    // do nothing
    } else {
    console.log("Hello, " + x.toUpperCase());
    }
} 
```

# strictPropertyInitialization

strictPropertyInitialization 设置类的实例属性都必须初始化，包括以下⼏种情况。 

设为 undefined 类型 

显式初始化 

构造函数中赋值 

注意，使⽤该属性的同时，必须打开 strictNullChecks。 

```typescript
// strictPropertyInitialization: true
class User {
    // 报错，属性 username 没有初始化
    username: string;
} 
```

```typescript
// 解决方法一
class User {
    username = '张三';
}

// 解决方法二
class User {
    username:string|undefined;
}

// 解决方法三
class User {
    username:string;

    constructor(username:string) {
    this.username = username;
    }
}

// 或者
class User {
    constructor(public username:string) {}

}

// 解决方法四：赋值断言
class User {
    username!:string;

    constructor(username:string) {
    this.initialize(username);
    }

    private initialize(username:string) {
    this.username = username;
    }
} 
```

# suppressExcessPropertyErrors

suppressExcessPropertyErrors 关闭对象字⾯量的多余参数的报错。 

# target

target 指定编译出来的 JavaScript 代码的 ECMAScript 版本，⽐如 es2021 ，默认是 es3 。 

它可以取以下值。 

. es3 

es5 

es6/es2015 

es2016 

es2017 

es2018 

es2019 

es2020 

es2021 

es2022 

esnext 

注意，如果编译的⽬标版本过⽼，⽐如"target": "es3"，有些语法可能⽆法编译，tsc 命令会报错。 

# traceResolution

traceResolution 设置编译时，在终端输出模块解析的具体步骤。 

```json
{
    "compilerOptions": {
    "traceResolution": true
    }
} 
```

# typeRoots

typeRoots 设置类型模块所在的⽬录，默认是 node_modules/@types，该⽬录⾥⾯的模块会⾃动加⼊编译。⼀ 旦指定了该属性，就不会再⽤默认值 node_modules/@types ⾥⾯的类型模块。 

该属性的值是⼀个数组，数组的每个成员就是⼀个⽬录，它们的路径是相对于 tsconfig.json 位置。 

```json
{
    "compilerOptions": {
    "typeRoots": ["./typings", "./vendor/types"]
    }
} 
```

# types

默认情况下，typeRoots⽬录下所有模块都会⾃动加⼊编译，如果指定了 types 属性，那么只有其中列出的模块 才会⾃动加⼊编译。 

```json
{
    "compilerOptions": {
    "types": ["node", "jest", "express"]
    }
} 
```

上⾯的设置表示，默认情况下，只有 ./node_modules/@types/node 、 ./node_modules/@types/jest 

和 ./node_modules/@types/express 会⾃动加⼊编译，其他 node_modules/@types/ ⽬录下的模块不会加⼊编 译。 

如果 "types": [] ，就表示不会⾃动将所有 @types 模块加⼊编译。 

# useUnknownInCatchVariables

useUnknownInCatchVariables 设置 catch 语句捕获的 try 抛出的返回值类型，从 any 变成 unknown 。 

```txt
try {
    someExternalFunction();
} catch (err) {
    err; // 类型 any
} 
```

上⾯示例中，默认情况下，catch 语句的参数 err 类型是 any，即可以是任何值。 

打开 useUnknownInCatchVariables 以后， err 的类型抛出的错误将是 unknown 类型。这带来的变化就是使 ⽤err 之前，必须缩⼩它的类型，否则会报错。 

```javascript
try {
    someExternalFunction();
} catch (err) {
    if (err instanceof Error) {
    console.log(err.message);
    }
} 
```---

# 24.条件类型与 infer 关键字（进阶补充）

> 以下为进阶内容。理解了前面的基础类型知识后，条件类型是 TypeScript 类型体操的核心工具。

## 24.1 基础语法

条件类型的语法和 JavaScript 三元表达式非常相似：

```typescript
type IsString<T> = T extends string ? true : false;

type A = IsString<"hello">; // true
type B = IsString<42>;      // false
```

核心结构：`A extends B ? C : D`——如果类型 `A` 可以赋值给类型 `B`，结果就是 `C`，否则是 `D`。

## 24.2 分配条件类型（Distributive Conditional Types）

当条件类型作用于**裸类型参数**且该参数是联合类型时，会自动分配到每个成员：

```typescript
type ToArray<T> = T extends unknown ? T[] : never;

// 传入联合类型时
type Result = ToArray<string | number>;
// 等价于: ToArray<string> | ToArray<number>
// 结果: string[] | number[]
```

这就是 `Exclude` 的工作原理：

```typescript
type MyExclude<T, U> = T extends U ? never : T;
type Result = MyExclude<"a" | "b" | "c", "a">;
// 结果: "b" | "c"
```

**阻止分配** — 用方括号包裹泛型参数：

```typescript
type ToArrayNoDistribute<T> = [T] extends [unknown] ? T[] : never;
type Result = ToArrayNoDistribute<string | number>;
// 结果: (string | number)[]  ← 不分配，整个联合类型被当作一个整体
```

## 24.3 infer 关键字：在条件中提取类型

`infer` 只能在 `extends` 子句中使用，用于从类型中提取子类型：

```typescript
// 提取数组元素类型
type ElementType<T> = T extends (infer U)[] ? U : never;
type A = ElementType<string[]>;  // string

// 实现 ReturnType
type MyReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
function greet() { return "hello"; }
type GreetReturn = MyReturnType<typeof greet>;  // string

// 提取函数第一个参数
type FirstParam<T> = T extends (first: infer F, ...rest: any[]) => any ? F : never;
```

## 24.4 递归条件类型

```typescript
// 展平嵌套数组
type Flatten<T> = T extends (infer U)[] ? Flatten<U> : T;
type Result = Flatten<number[][][]>;  // number

// 提取 Promise 最终值类型（递归展开）
type Awaited<T> = T extends Promise<infer U> ? Awaited<U> : T;
type R1 = Awaited<Promise<string>>;          // string
type R2 = Awaited<Promise<Promise<number>>>; // number
```

## 24.5 模板字面量 + infer 组合技

```typescript
// 提取路由参数（类似 Express 路由）
type RouteParams<T extends string> =
  T extends `${string}:${infer Param}/${infer Rest}`
    ? Param | RouteParams<`/${Rest}`>
    : T extends `${string}:${infer Param}`
      ? Param
      : never;

type Params = RouteParams<"/user/:id/post/:postId">;
// "id" | "postId"
```

## 24.6 实用工具类型

```typescript
// DeepPartial — 递归可选
type DeepPartial<T> = {
  [K in keyof T]?: T[K] extends object
    ? T[K] extends Function ? T[K] : DeepPartial<T[K]>
    : T[K];
};

// 只提取值类型为特定类型的键
type KeysOfType<T, V> = {
  [K in keyof T]: T[K] extends V ? K : never;
}[keyof T];
```

## 24.7 常见陷阱

1. **条件类型从不报错**，不匹配时返回 `never`
2. **递归深度限制**约 50 层，复杂递归可能导致编译性能问题
3. **`never` 在联合类型中自动移除**，在交叉类型中吞掉一切

> 类型是程序的文档，而条件类型是编写这份文档的笔。
