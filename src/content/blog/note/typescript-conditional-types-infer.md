---
title: TypeScript 条件类型与 infer 关键字详解
link: typescript-conditional-types-infer
catalog: true
date: 2026-06-12 15:30:00
description: 深入理解 TypeScript 条件类型和 infer 关键字，从基础语法到高级实战，掌握类型体操的核心利器。
tags:
  - 前端
  - TypeScript
  - 笔记
categories:
  - 笔记
  - 前端
  - TypeScript
---

## 为什么需要条件类型

日常开发中，我们经常遇到「根据输入类型决定输出类型」的场景。比如：

```typescript
// 一个简单的函数：如果是字符串返回长度，如果是数字返回本身
function process(value: string | number): number {
  if (typeof value === "string") {
    return value.length;
  }
  return value;
}
```

运行时很简单，但**类型层面**如何表达这种分支逻辑？这就是条件类型的用武之地。

`★ Insight ─────────────────────────────────────`
条件类型本质上是 TypeScript 类型系统的 **if/else** —— 它让类型可以根据条件分支，是构建复杂工具类型的基础设施。理解条件类型之后，`Pick`、`Exclude`、`ReturnType` 等内置工具类型的实现原理就一目了然了。
`─────────────────────────────────────────────────`

## 条件类型基础语法

条件类型的语法和 JavaScript 三元表达式非常相似：

```typescript
type IsString<T> = T extends string ? true : false;

// 使用
type A = IsString<"hello">; // true
type B = IsString<42>;      // false
```

核心结构：`A extends B ? C : D` — 如果类型 `A` 可以赋值给类型 `B`，结果就是 `C`，否则是 `D`。

### extends 的两种含义

`extends` 在 TypeScript 中有两种不同的角色：

| 场景 | 含义 | 示例 |
|------|------|------|
| 接口继承 | 「继承/扩展」 | `interface Dog extends Animal` |
| 条件类型 | 「可赋值给 / 是...的子类型」 | `T extends string ? A : B` |

在条件类型中，`extends` 做的是**类型兼容性检查**——判断左边的类型是否是右边类型的子类型。

## 分配条件类型（Distributive Conditional Types）

这是条件类型最容易被忽视但又最重要的特性。当条件类型作用于**裸类型参数**（即泛型参数直接放在 `extends` 前）且该参数是联合类型时，条件类型会**自动分配**到联合类型的每个成员上：

```typescript
type ToArray<T> = T extends unknown ? T[] : never;

// 传入联合类型时
type Result = ToArray<string | number>;
// 等价于: ToArray<string> | ToArray<number>
// 结果: string[] | number[]
```

这就是为什么 `Exclude` 可以工作的原理：

```typescript
type MyExclude<T, U> = T extends U ? never : T;

type Result = MyExclude<"a" | "b" | "c", "a">;
// 分配: ("a" extends "a" ? never : "a") | ("b" extends "a" ? never : "b") | ("c" extends "a" ? never : "c")
// 结果: "b" | "c"
```

### 阻止分配

有时你不希望分配行为发生。只需要用方括号包裹泛型参数：

```typescript
type ToArrayNoDistribute<T> = [T] extends [unknown] ? T[] : never;

type Result = ToArrayNoDistribute<string | number>;
// 结果: (string | number)[]  ← 整个联合类型被当作一个整体
```

`★ Insight ─────────────────────────────────────`
分配条件类型是 TypeScript 中最容易被误解的特性之一。记住一个简单的规则：**裸类型参数会分配，包裹起来就不会**。`T extends U` 会分配，`[T] extends [U]` 不会。这也是为什么很多工具类型用元组包裹来做非分配判断。
`─────────────────────────────────────────────────`

## infer 关键字：在条件中提取类型

`infer` 只能在条件类型的 `extends` 子句中使用，用于**从类型中提取（推断）子类型**。它就像一个类型层面的「解构」：

```typescript
// 提取数组元素类型
type ElementType<T> = T extends (infer U)[] ? U : never;

type A = ElementType<string[]>;    // string
type B = ElementType<number[]>;    // number
type C = ElementType<boolean>;     // never（不匹配）
```

### 经典案例：实现 ReturnType

```typescript
type MyReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

function greet() { return "hello"; }
type GreetReturn = MyReturnType<typeof greet>; // string

const add = (a: number, b: number) => a + b;
type AddReturn = MyReturnType<typeof add>;     // number
```

### 提取函数参数类型

```typescript
type FirstParam<T> = T extends (first: infer F, ...rest: any[]) => any ? F : never;

function sendMessage(to: string, content: string) {}
type To = FirstParam<typeof sendMessage>; // string
```

## 递归条件类型

TypeScript 4.1+ 支持递归条件类型，这让类型层面的「遍历」成为可能：

```typescript
// 将嵌套数组完全展平
type Flatten<T> = T extends (infer U)[] ? Flatten<U> : T;

type Result = Flatten<number[][][]>; // number
```

```typescript
// 深度 Readonly
type DeepReadonly<T> = {
  readonly [K in keyof T]: T[K] extends object
    ? T[K] extends Function
      ? T[K]
      : DeepReadonly<T[K]>
    : T[K];
};
```

### 模板字面量 + infer 的组合技

TypeScript 4.1 引入模板字面量类型后，结合 `infer` 可以玩出很多花样：

```typescript
// 提取路径中的参数（类似 Express 路由）
type RouteParams<T extends string> =
  T extends `${string}:${infer Param}/${infer Rest}`
    ? Param | RouteParams<`/${Rest}`>
    : T extends `${string}:${infer Param}`
      ? Param
      : never;

type Params = RouteParams<"/user/:id/post/:postId">;
// "id" | "postId"
```

```typescript
// 将联合类型转换为交叉类型（高级技巧）
type UnionToIntersection<U> =
  (U extends unknown ? (k: U) => void : never) extends (k: infer I) => void
    ? I
    : never;

type Result = UnionToIntersection<{ a: 1 } | { b: 2 }>;
// { a: 1 } & { b: 2 }
```

`★ Insight ─────────────────────────────────────`
`UnionToIntersection` 是最精妙的类型体操之一。它利用了**函数参数位置的逆变**（contravariance）：当联合类型的每个成员都作为函数参数类型时，它们的交集变成了新的参数类型。这个技巧在构建 `DeepPick`、`Merge` 等复杂工具类型时经常出现。
`─────────────────────────────────────────────────`

## 实战：从零构建实用工具类型

### 1. DeepPartial — 递归可选

```typescript
type DeepPartial<T> = {
  [K in keyof T]?: T[K] extends object
    ? T[K] extends Function
      ? T[K]
      : DeepPartial<T[K]>
    : T[K];
};

interface Config {
  server: {
    host: string;
    port: number;
  };
  plugins: string[];
}

type PartialConfig = DeepPartial<Config>;
// { server?: { host?: string; port?: number }; plugins?: string[] }
```

### 2. NonNullable 的实现

```typescript
type MyNonNullable<T> = T extends null | undefined ? never : T;

type A = MyNonNullable<string | null | undefined>; // string
```

### 3. 提取 Promise 的最终值类型（递归展开）

```typescript
type Awaited<T> = T extends Promise<infer U> ? Awaited<U> : T;

type R1 = Awaited<Promise<string>>;          // string
type R2 = Awaited<Promise<Promise<number>>>; // number
```

事实上 TypeScript 4.5+ 已经内置了 `Awaited` 工具类型，以上正是它的实现思路。

### 4. 条件类型 + 映射类型组合

```typescript
// 只提取值类型为特定类型的键
type KeysOfType<T, V> = {
  [K in keyof T]: T[K] extends V ? K : never;
}[keyof T];

interface User {
  name: string;
  age: number;
  isAdmin: boolean;
  email: string;
}

type StringKeys = KeysOfType<User, string>; // "name" | "email"
```

## 条件类型推断的优先级规则

当多个 `infer` 出现在同一个条件类型中，TypeScript 按以下规则推断：

```typescript
// 同一个类型变量可以被推断多次（取交集）
type Example1<T> = T extends { a: infer U; b: infer U } ? U : never;
type R1 = Example1<{ a: string; b: string }>;  // string
type R2 = Example1<{ a: string; b: number }>;  // string | number

// 协变位置取联合，逆变位置取交叉
type Covariant<T> = T extends { a: infer U; b: infer U } ? U : never;
type Contravariant<T> = T extends { a: (x: infer U) => void; b: (x: infer U) => void } ? U : never;
```

## 常见陷阱与最佳实践

### 陷阱 1：条件类型永远不会「失败」

条件类型总是返回一个类型，不会报错。这可能导致意外的 `never`：

```typescript
type GetLength<T> = T extends { length: number } ? T["length"] : never;

type A = GetLength<"hello">;  // number（字面量类型 "hello" 有 length 属性）
type B = GetLength<42>;        // never ← 可能不是你想要的结果
```

**建议**：在使用结果前做类型收窄，或者在函数签名中使用重载来提供更好的错误提示。

### 陷阱 2：复杂条件类型的性能

递归条件类型 + 大量联合类型会导致编译性能问题。TypeScript 的递归深度限制为大约 50 层：

```typescript
// ⚠️ 可能触发递归深度限制
type DeepPath<T, Path extends string[]> = /* ... */;
```

**建议**：使用尾递归优化（TypeScript 4.5+ 支持），避免不必要的深层嵌套。

### 陷阱 3：never 的特殊行为

`never` 在联合类型中会被自动移除，在交叉类型中会吞掉一切：

```typescript
type Test1 = string | never;   // string
type Test2 = string & never;   // never
```

这在条件类型中很有用——用 `never` 来「过滤」不需要的分支。

## 总结

条件类型和 `infer` 是 TypeScript 类型系统的精华部分。掌握它们之后：

- **内置工具类型**不再是黑盒——你理解 `Exclude`、`Extract`、`ReturnType`、`Parameters` 的实现
- **库的类型定义**变得可读写——不再被复杂的泛型声明吓到
- **写出更安全的代码**——用类型约束代替运行时检查

记住三个核心要点：

1. **`T extends U ? X : Y`** — 类型的 if/else
2. **`infer`** — 在条件中捕获（提取）子类型
3. **分配行为** — 裸类型参数会自动分配，需要时用 `[]` 包裹阻止

> 类型是程序员的文档，而条件类型是编写这份文档的笔。
