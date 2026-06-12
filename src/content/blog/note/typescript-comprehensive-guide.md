---
title: TypeScript 学习笔记
link: typescript-comprehensive-guide
catalog: true
date: 2026-06-12 15:30:00
description: 一份全面的 TypeScript 学习指南，涵盖从基础语法到高级类型体操的核心知识点，包括类型系统、泛型、条件类型、infer 关键字等。
tags:
  - 前端
  - TypeScript
  - 笔记
categories:
  - 笔记
  - 前端
  - TypeScript
---

## 1. 认识 TypeScript

### 1.1 发展史

TypeScript 是 JavaScript 的一个超集，支持 ECMAScript 6 标准。由微软开发，设计目标是开发大型应用，可以编译成纯 JavaScript 运行在任何浏览器上。

- **2013 年**：Visual Studio 2013 开始内置支持 TypeScript
- **2014 年**：TypeScript 1.0 发布，代码仓库迁移到 GitHub
- **2016 年**：TypeScript 2.0 发布，引入重大语法功能
- **2018 年**：TypeScript 3.0 发布
- **2020 年**：TypeScript 4.0 发布
- **2023 年**：TypeScript 5.0 发布

中文官网：https://ts.nodejs.cn/

### 1.2 TypeScript 和 JavaScript 的区别

| 特性 | JavaScript | TypeScript |
|------|-----------|------------|
| 类型系统 | 动态类型、弱类型 | 静态类型 |
| 类型检查时机 | 运行时 | 编译阶段 |
| 运行方式 | 直接运行 | 需先编译为 JS |
| 错误发现 | 运行时报错 | 编译时就能发现 |

**JavaScript 是动态类型、弱类型语言**：在运行时才会进行类型检查，类型错误往往导致运行时错误。

**TypeScript 是静态类型**：编译阶段就能确定每个变量的类型，在编译阶段就会进行类型检查。

`★ Insight ─────────────────────────────────────`
静态类型的核心价值不在于「写类型标注多花的时间」，而在于**把 bug 的发现时间从运行时提前到编译时**。一个在 IDE 里标红的错误，永远比生产环境报错便宜。
`─────────────────────────────────────────────────`

## 2. 安装与使用

### 2.1 安装

```bash
npm install -g typescript
```

安装完成后检查版本：

```bash
tsc -v
# Version 5.1.6
```

TypeScript 官方提供的编译器叫做 `tsc`，可以将 TypeScript 脚本编译成 JavaScript 脚本。浏览器和 Node.js 不认识 `.ts` 文件，必须先编译。

### 2.2 基本使用

使用 `.ts` 作为 TypeScript 代码文件的扩展名，然后执行编译：

```bash
tsc app.ts           # 单次编译
tsc app.ts -w        # 监听模式，自动重新编译
```

## 3. 原始数据类型

TypeScript 的类型分类：

1. **原始数据类型**：`boolean` `number` `string` `undefined` `null` `symbol`
2. **对象类型**：对象（`object`）、数组（`Array`）、元组（`Tuple`）、枚举（`enum`）、函数（`function`）
3. **其他类型**：`any` `never` `unknown` `void` 值类型

TypeScript 要求在变量声明时声明类型：

```typescript
let 变量名: 类型 = 数据;
```

> 所有类型名称都是**小写字母**，首字母大写的 `Number`、`String`、`Boolean` 是 JavaScript 的内置对象，不是类型名称。

```typescript
// string
let a: string = "hello";
let b: string;
const d: string = "bb";

// number
let n: number = 123;

// boolean
let boo: boolean = true;

// undefined
let u: undefined = undefined;

// null
let nu: null = null;

// symbol
let s: symbol = Symbol();
```

## 4. 其他类型

### 4.1 any 类型

`any` 表示任意类型，可以任意赋值。设置 `any` 相当于对该变量关闭了类型检测。声明变量不设置类型，默认也是 `any`。

```typescript
let a: any = 5;
a = "hello";   // OK
a = true;      // OK
```

> ⚠️ 开发中不建议使用 `any`，它会丧失类型保护。

### 4.2 unknown 类型

`unknown` 相对于 `any` 是安全的——必须通过类型收窄才能使用：

```typescript
let a: unknown = 5;

if (typeof a === "number") {
  console.log(a * 2);          // OK
} else if (typeof a === "string") {
  console.log(a.substring(1)); // OK
}
```

### 4.3 never 类型

`never` 表示永远不会有返回值的类型，比如抛出异常或死循环：

```typescript
function error(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}
```

### 4.4 值类型（字面量类型）

单个值也是一种类型：

```typescript
let a: 123 = 123;
let b: "hello" = "hello";
```

## 5. tsconfig.json 文件

`tsconfig.json` 是 TypeScript 项目的配置文件，放在项目根目录，供 `tsc` 编译器使用。

```bash
tsc --init   # 生成 tsconfig.json
```

常见配置选项：

| 选项 | 说明 |
|------|------|
| `target` | 编译后的 ECMAScript 版本 |
| `module` | 模块系统（CommonJS、ES2015 等） |
| `outDir` | 编译输出目录 |
| `sourceMap` | 是否生成源码映射 |
| `strict` | 启用严格模式 |
| `include` / `exclude` | 包含/排除文件 |
| `baseUrl` / `paths` | 模块解析与路径别名 |
| `files` | 手动指定编译文件列表 |
| `references` | 引用其他 TypeScript 项目 |

## 6. 数组和元组

### 6.1 数组

数组中所有成员的类型必须相同：

```typescript
// 两种声明语法
let arr: 类型[] = [];
let arr: Array<类型> = [];  // 泛型写法

// 示例
let arr: string[] = ["a", "b"];
let arr2: number[];
arr2 = [1, 2, 3];

// 二维数组
let arr4: string[][] = [["a", "b"], ["e", "f"]];

// 对象数组
let arr5: { name: string; age: number }[] = [
  { name: "aa", age: 18 },
  { name: "bb", age: 17 }
];

// 联合类型数组
let arr6: (string | number)[] = ["hh", 123];
```

### 6.2 元组

元组可以包含不同类型的元素，每个位置有固定类型：

```typescript
let arr: [string, number, boolean] = ["A", 123, true];

// 先声明再赋值
let arr2: [boolean, string];
arr2[0] = true;
arr2[1] = "123";

// 后续添加元素必须是已有类型的联合类型
arr2.push("123");  // OK
```

## 7. 对象

限定对象中每个属性的类型：

```typescript
let obj: { name: string; age: number } = { name: "小明", age: 18 };
```

### 7.1 可选属性

属性名后加 `?`，代表该属性可有可无：

```typescript
let a: { name: string; age?: number };
a = { name: "hello" };  // age 可以不给
```

### 7.2 任意属性

```typescript
let a: { name: string; [propName: string]: any };
// propName 是形参，可以换成别的名字
// 任意属性的类型必须是其他所有属性类型的父类
a = { name: "张三", age: 18, salary: 3500 };
```

### 7.3 只读属性

```typescript
let obj: { readonly name: string; age: number };
obj = { name: "小红", age: 18 };
obj.age = 20;     // OK
obj.name = "哈哈"; // ❌ 只读属性不能修改
```

### 7.4 内置对象类型

```typescript
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');
```

## 8. 函数

### 8.1 基本使用

```typescript
function fn(m: number, n: string): string {
  return n + m;
}

// 没有返回值用 void
function fn(m: number, n: string): void {
  console.log(m + n);
}

// 变量赋值形式
let fn = function(m: number, n: string): void {
  console.log(m + n);
};

// 箭头函数
let fn = (m: number, n: string): void => {
  console.log(m + n);
};

// 单独定义函数类型
let fn3: (a: number) => number;
fn3 = function(a: number): number {
  return 123;
};
```

### 8.2 可选参数

```typescript
function fn(m: number, n?: string): void {
  console.log(m, n);
}
fn(8);  // OK
```

> 可选参数必须放在必需参数后面。

### 8.3 参数省略

TypeScript 允许实际参数少于类型指定的参数个数：

```typescript
let fn: (a: number, b: number) => number;
fn = (a: number) => a;         // OK
fn = (a: number, b: number, c: number) => a;  // ❌ 多了不行
```

### 8.4 rest 参数

```typescript
// rest 参数为数组
function joinNumbers(...nums: number[]) { }

// rest 参数为元组
function f(...args: [boolean, number]) { }
```

## 9. 联合类型

联合类型表示取值可以为多种类型中的一种，使用 `|` 分隔：

```typescript
let val: string | number | string[] | (() => number);
val = 12;          // OK
val = "hello";     // OK
```

访问联合类型的属性时，只能访问**所有类型共有的属性**：

```typescript
function getString(something: string | number): string {
  return something.toString();  // OK，string 和 number 都有 toString
}
```

`★ Insight ─────────────────────────────────────`
联合类型 + 类型收窄（type narrowing）是 TypeScript 中最常用的模式之一。`typeof`、`instanceof`、`in` 运算符和自定义类型守卫都是收窄联合类型的工具。掌握好这个模式，大部分日常类型问题都能解决。
`─────────────────────────────────────────────────`

## 10. 类型别名

使用 `type` 关键字给现有类型取一个新名字：

```typescript
type MyString = string;
let a: MyString = "hello";

// 联合类型别名
type Directions = 'up' | 'Down' | 'Left' | 'Right';
let toWhere: Directions = 'Down';

// 函数类型别名
type Add = (x: number, y: number) => number;
let add: Add = (x, y) => x + y;

// 对象类型别名
type Person = { name: string; age: number };
const arr: Person[] = [
  { name: "小明", age: 18 },
  { name: "小红", age: 28 },
];

// 组合使用
type A = { name: string };
type B = number;
type C = A | B | "哈哈" | string;
```

## 11. 类型推论

TypeScript 会根据赋值自动推断类型：

```typescript
let a = 1;
a = "hello";  // ❌ Type 'string' is not assignable to type 'number'
```

如果定义时没有赋值，会被推断为 `any`：

```typescript
let a;        // any
a = "hello";  // OK
a = 7;        // OK
```

## 12. typeof 和 keyof 运算符

### 12.1 typeof（类型运算）

TypeScript 将 `typeof` 移植到了类型运算，操作数是值，返回的是 TypeScript 类型：

```typescript
const a = { x: 0 };
type T0 = typeof a;    // { x: number }
type T1 = typeof a.x;  // number
```

> 根据已有的值来获取类型，简化代码书写。

### 12.2 keyof

`keyof` 接受对象类型，返回所有键名组成的联合类型：

```typescript
type Person = { name: string; age: number };
type MyType = keyof Person;  // "name" | "age"
let a: MyType = "name";      // OK
```

`★ Insight ─────────────────────────────────────`
`keyof` 是类型体操的基石之一。它让你可以在类型层面「遍历」对象的键，配合映射类型可以实现各种对象变换（`Pick`、`Omit`、`Partial` 等内置类型都依赖它）。
`─────────────────────────────────────────────────`

## 13. 映射类型

基于旧类型创建新类型，减少重复：

```typescript
// 基本使用
type My = "a" | "b" | "c";
type MyType = { [key in My]: number };
// { a: number; b: number; c: number }

// 配合 keyof —— 复制一个类型
type Props = { a: number; b: string; c: boolean };
type MyType2 = { [key in keyof Props]: Props[key] };
// { a: number; b: string; c: boolean }

// 配合可选
type MyType3 = { [key in My]?: number };
// { a?: number; b?: number; c?: number }

// 配合只读
type MyType4 = { readonly [key in My]: number };
// { readonly a: number; readonly b: number; readonly c: number }
```

> 映射类型只能在类型别名（`type`）中使用，不能在接口（`interface`）中使用。

## 14. 接口

### 14.1 基本使用

接口定义对象的类型模板：

```typescript
interface Person {
  name: string;
  age: number;
  salary: number;
}
let obj: Person = { name: "张三", age: 18, salary: 3500 };
```

接口同样支持只读属性、可选属性、任意属性：

```typescript
interface Person {
  name: string;
  age: number;
  readonly salary: number;
  like: string[];
  run?: () => void;
  [propName: string]: string | number | string[] | (() => void);
}
```

### 14.2 接口的继承

```typescript
interface Person {
  name: string;
  age: number;
  address: string;
}

interface Girl extends Person {
  height: number;
  hobby: string[];
}

let xf: Girl = {
  name: "小芳",
  age: 18,
  address: "北京",
  height: 170,
  hobby: ["逛街", "买买买"]
};
```

一个接口可以继承多个接口：

```typescript
interface Girl extends Person, Pro {
  height: number;
  hobby: string[];
}
```

### 14.3 同名接口会自动合并

```typescript
interface Person {
  name: string;
  age: number;
}

interface Person {
  salary: number;
}

// 相当于：
// interface Person { name: string; age: number; salary: number; }
```

### 14.4 接口与类型别名的区别

| 特性 | `type` | `interface` |
|------|--------|-------------|
| 表示非对象类型 | ✅ 可以 | ❌ 只能表示对象类型 |
| 继承 | ❌ 不支持（用 `&` 交叉） | ✅ 支持 `extends` |
| 同名合并 | ❌ 报错 | ✅ 自动合并 |
| 映射类型 | ✅ 支持 | ❌ 不支持 |

## 15. 交叉类型

使用 `&` 将多个对象类型合并：

```typescript
type Person = { name: string; age: number };
type Emp = { salary: number; address: string };
type C = Person & Emp & { height: number };

let obj: C = {
  name: "张三", age: 18,
  salary: 3500, address: "beijing",
  height: 180
};
```

> ⚠️ 不要对基本类型使用交叉类型（如 `string & number`），结果会是 `never`。

## 16. 类型断言

### 16.1 基本用法

明确告诉编译器某个值的类型，跳过类型检查：

```typescript
值 as 类型
// 或
<类型>值   // 在 tsx 中不推荐，统一用 as
```

### 16.2 联合类型断言为其中一个类型

```typescript
function fn(m: string | number) {
  (m as string).substring(1);
}
```

### 16.3 非空断言

```typescript
type MyFn = () => number;
function fn(m: MyFn | undefined) {
  let num = m!();  // 断言 m 一定不是 undefined
}
```

> ⚠️ 类型断言只能欺骗编译器，无法避免运行时错误。使用要谨慎。

## 17. enum 枚举类型

枚举用于定义有名字的数字常量，改善代码可读性：

```typescript
enum OrderStatus {
  Start = 1,
  Unpaid,     // 2
  Shipping,   // 3
  Shipped,    // 4
  Complete,   // 5
}
// 取值: OrderStatus.Start

// 实际场景
enum Status {
  Success = 200,
  NotFound = 404,
  Error = 500
}
```

### 字符串枚举

```typescript
enum Message {
  Error = "Sorry, error",
  Success = "Hoho, success"
}
```

### 反向映射（仅数字枚举支持）

```typescript
console.log(Status[200]);  // 'Success'
```

## 18. 泛型

泛型 = 类型参数。定义时不指定具体类型，使用时再传入。

### 18.1 函数中使用泛型

```typescript
function identity<T>(arg: T): T {
  return arg;
}
identity<number>(100);
// 或利用类型推断
identity(100);  // T 被推断为 number

// 多个类型参数
function identity<T, U>(arg: T, arg2: U): T {
  return arg;
}
identity<number, string>(4, "hello");
```

### 18.2 接口中使用泛型

```typescript
interface Person<N> {
  name: string;
  age: N;
}
const xiong: Person<string> = { name: "xiong", age: "18" };
```

### 18.3 类中使用泛型

```typescript
class Person<T> {
  name: T;
  age: number;
  constructor(name: T, age: number) {
    this.name = name;
    this.age = age;
  }
}
const xiong = new Person<string>("xiong", 18);
```

### 18.4 泛型约束

使用 `extends` 约束泛型参数必须满足特定形状：

```typescript
interface Person {
  name: string;
  age: number;
}

function fn<T extends Person>(person: T) { }

fn<{ name: string; age: number; salary: number }>({
  name: "张三", age: 18, salary: 3500
});  // OK，多属性可以，少不行
```

配合 `keyof` 使用：

```typescript
let obj = { a: 1, b: 2, c: 3 };
function fn<T extends keyof U, U>(a: T, b: U) { }
fn("a", obj);  // OK
```

`★ Insight ─────────────────────────────────────`
泛型约束 `<T extends U>` 的含义是「T 必须是 U 的子类型」，这和条件类型中的 `extends` 语义一致。理解这一点是进阶 TypeScript 的关键——它把「约束」和「判断」统一在了同一个关键字下。
`─────────────────────────────────────────────────`

## 19. class 类

TypeScript 中 class 构造函数里用到的所有属性，必须提前定义类型：

```typescript
class Person {
  // 实例属性
  name: string = "张三";
  age: number;
  // 类属性（静态属性）
  static count: number = 100;
  // 只读属性
  readonly sex: string = "boy";

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  eat() {
    console.log("我在吃饭");
  }

  // 静态方法
  static sleep() {
    console.log("我睡觉呢");
  }
}
```

### 19.1 继承

```typescript
class Animal {
  name: string;
  age: number;
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
  eat() { console.log("我在吃饭"); }
}

class Dog extends Animal {
  gender: string;
  constructor(name: string, age: number, gender: string) {
    super(name, age);
    this.gender = gender;
  }
  // 方法重写
  eat() { console.log("我在吃骨头"); }
}
```

### 19.2 访问修饰符

| 修饰符 | 说明 |
|--------|------|
| `public`（默认） | 任何地方可访问 |
| `private` | 只能在当前类中访问 |
| `protected` | 当前类和子类中可以访问 |

### 19.3 抽象类

```typescript
abstract class Animal {
  abstract eat(): void;
  abstract fn(): void;
  sleep() { console.log(1); }
}
// 包含抽象方法的类必须被声明为 abstract，子类必须实现抽象方法
```

### 19.4 implements 关键字

检查类是否遵循特定接口：

```typescript
interface Person {
  id: number;
  name: string;
  play: () => void;
}

class XiaoMing implements Person {
  id: number;
  name: string;
  play: () => void;
  // 必须实现 Person 的所有属性和方法
}
```

## 20. 模块

TypeScript 模块建立在 ES6 模块基础上。包含 `import` 或 `export` 语句的文件就是模块。

### 类型导出/导入

```typescript
// 导出
export interface Person { name: string; age: number; }
export type obj = { gender: boolean; like: string[] };
export { type obj, type Person };  // 推荐：明确标记为类型导出

// 导入
import type { obj, Person } from "./a";
import { type obj, type Person } from "./a";  // 混合导入
```

> 使用 `type` 关键字标记类型导入/导出可以减少编译输出文件大小，避免不必要的运行时开销。

## 21. namespace

命名空间用于组织代码和防止名称冲突（ES6 模块出现前的方案，官方不再推荐）：

```typescript
namespace Utils {
  export function fn() { console.log(1); }
  export var a = 1;
}
Utils.fn();

// 嵌套
namespace My {
  export namespace Girlfriend {
    export function angry() { console.log("爱生气"); }
  }
  export interface Person { name: string; }
}
```

## 22. declare 关键字

`declare` 告诉编译器某个类型是存在的，可以在当前文件中使用。只描述不实现，编译后会被移除。

```typescript
// .d.ts 文件
declare function fn(x: number, y: number): number;
declare let a: number;
declare class Person { name: string; }

// 为外部模块扩展类型（同名 interface 自动合并）
import { A } from "./index";
declare module "./index" {
  interface A { y: number; }
}

// 通配符模块声明
declare module 'my-plugin-*' {
  interface PluginOptions { enabled: boolean; priority: number; }
  function initialize(options: PluginOptions): void;
  export = initialize;
}
```

### .d.ts 文件加载机制

1. **同名引入**：`foo.js` 同级目录下的 `foo.d.ts` 自动被识别
2. **自动引入**：`node_modules/@types/` 下的声明文件自动加载
3. **tsconfig.json 配置**：通过 `include` / `typeRoots` / `types` 控制
4. **三斜线指令**：`/// <reference path="..." />`（不推荐，尽量用 import）

> 第三方库的类型声明通常来自 DefinitelyTyped，安装方式：`npm install @types/jquery --save-dev`

## 23. tsconfig.json 详细配置

### exclude

从编译列表中排除文件：

```json
{
  "include": ["**/*"],
  "exclude": ["**/*.spec.ts"]
}
```

### extends

继承另一个 tsconfig.json：

```json
{
  "extends": "../tsconfig.base.json"
}
```

### files

手动指定编译文件列表（不支持通配符）：

```json
{
  "files": ["a.ts", "b.ts"]
}
```

### include

指定编译文件（支持通配符 `?` `*` `**`）：

```json
{
  "include": ["src/**/*", "tests/**/*"]
}
```

### compilerOptions 常用选项

| 选项 | 说明 |
|------|------|
| `strict` | 总开关，开启所有严格检查 |
| `target` | 编译目标 ES 版本（es2016、es2022 等） |
| `module` | 模块系统（commonjs、esnext、node16 等） |
| `moduleResolution` | 模块解析策略（node、bundler 等） |
| `outDir` | 编译输出目录 |
| `rootDir` | 源码根目录 |
| `baseUrl` / `paths` | 路径别名配置 |
| `sourceMap` | 是否生成 SourceMap |
| `declaration` | 是否生成 `.d.ts` 文件 |
| `noImplicitAny` | 禁止隐式 any |
| `strictNullChecks` | 严格 null/undefined 检查 |
| `esModuleInterop` | 修复 CJS/ESM 兼容性问题 |
| `allowJs` | 允许编译 JS 文件 |
| `jsx` | JSX 处理方式（react、preserve 等） |
| `lib` | 内置类型声明文件列表（dom、es2021 等） |
| `resolveJsonModule` | 允许 import JSON 文件 |
| `incremental` | 增量构建 |

---

## 24. 条件类型与 infer 关键字（进阶）

> 以下为进阶内容。理解了前面的基础类型知识后，条件类型是 TypeScript 类型体操的核心工具。

### 24.1 为什么需要条件类型

日常开发中经常遇到「根据输入类型决定输出类型」的场景。运行时很简单，但**类型层面**如何表达这种分支逻辑？条件类型就是答案。

### 24.2 基础语法

```typescript
type IsString<T> = T extends string ? true : false;

type A = IsString<"hello">; // true
type B = IsString<42>;      // false
```

核心结构：`A extends B ? C : D`——如果类型 `A` 可以赋值给类型 `B`，结果就是 `C`，否则是 `D`。

### 24.3 分配条件类型（Distributive Conditional Types）

当条件类型作用于**裸类型参数**且该参数是联合类型时，会自动分配到每个成员：

```typescript
type ToArray<T> = T extends unknown ? T[] : never;
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

**阻止分配**——用方括号包裹：

```typescript
type ToArrayNoDistribute<T> = [T] extends [unknown] ? T[] : never;
type Result = ToArrayNoDistribute<string | number>;
// 结果: (string | number)[]  ← 不分配
```

### 24.4 infer 关键字：在条件中提取类型

`infer` 只能在 `extends` 子句中使用，用于从类型中提取子类型：

```typescript
// 提取数组元素类型
type ElementType<T> = T extends (infer U)[] ? U : never;
type A = ElementType<string[]>;  // string

// 实现 ReturnType
type MyReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
type GreetReturn = MyReturnType<typeof greet>;  // string

// 提取函数第一个参数
type FirstParam<T> = T extends (first: infer F, ...rest: any[]) => any ? F : never;
```

### 24.5 递归条件类型

```typescript
// 展平嵌套数组
type Flatten<T> = T extends (infer U)[] ? Flatten<U> : T;
type Result = Flatten<number[][][]>;  // number

// 深度 Readonly
type DeepReadonly<T> = {
  readonly [K in keyof T]: T[K] extends object
    ? T[K] extends Function ? T[K] : DeepReadonly<T[K]>
    : T[K];
};
```

### 24.6 模板字面量 + infer

```typescript
// 提取路由参数
type RouteParams<T extends string> =
  T extends `${string}:${infer Param}/${infer Rest}`
    ? Param | RouteParams<`/${Rest}`>
    : T extends `${string}:${infer Param}`
      ? Param
      : never;

type Params = RouteParams<"/user/:id/post/:postId">;
// "id" | "postId"
```

### 24.7 实战工具类型

```typescript
// DeepPartial
type DeepPartial<T> = {
  [K in keyof T]?: T[K] extends object
    ? T[K] extends Function ? T[K] : DeepPartial<T[K]>
    : T[K];
};

// 提取 Promise 最终值
type Awaited<T> = T extends Promise<infer U> ? Awaited<U> : T;

// 提取值类型匹配的键
type KeysOfType<T, V> = {
  [K in keyof T]: T[K] extends V ? K : never;
}[keyof T];
```

### 24.8 常见陷阱

1. **条件类型从不报错**，不匹配时返回 `never`
2. **递归深度限制**约 50 层，复杂递归可能导致性能问题
3. **`never` 在联合类型中自动移除**，在交叉类型中吞掉一切

## 总结

TypeScript 的学习路径可以概括为三层：

1. **基础层**（类型标注、接口、泛型）—— 日常开发必备
2. **进阶层**（映射类型、条件类型、infer）—— 库开发和类型工具
3. **体操层**（递归条件类型、模板字面量类型）—— 极致的类型安全

> 类型是程序的文档，而 TypeScript 的类型系统是编写这份文档最强大的笔。
