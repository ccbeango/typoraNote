# Type入门概念

## 介绍

[TypeScript](http://www.typescriptlang.org/) 是 JavaScript 的一个超集，主要提供了**类型系统**和**对 ES6 的支持**，它由 Microsoft 开发，代码[开源于 GitHub](https://github.com/Microsoft/TypeScript) 上。

安装TypeScript的方法很简单：

```shell
npm install -g typescript
```

一个小测试，创建`hello.ts`，如下

```typescript
function sayHello(person: string) {
    return 'Hello, ' + person;
}

let user = 'Tom';
console.log(sayHello(user));
```

然后只需要运行`tsc hello.ts`进行编译，这时候会生成一个编译好的`hello.js`

```javascript
function sayHello(person) {
    return 'Hello, ' + person;
}
var user = 'Tom';
console.log(sayHello(user));
```

假如现在将`hello.ts`稍作修改。

```typescript
function sayHello(person: string) {
    return 'Hello, ' + person;
}

let user = ['Tom'];
console.log(sayHello(user));
```

再运行`tsc hello.ts`进行编译，会报错如下：

```powershell
hello.ts:6:22 - error TS2345: Argument of type 'string[]' is not assignable to parameter of type 'string'.

6 console.log(sayHello(person));
                       ~~~~~~
Found 1 error.
```

但是仍然会编译成功

```javascript
function sayHello(person) {
    return "Hello, " + person;
}
var person = ['Tom'];
console.log(sayHello(person));
```

**TypeScript 编译的时候即使报错了，还是会生成编译结果**，我们仍然可以使用这个编译之后的文件。

编译报错可以终止JS文件的生成，可以在 `tsconfig.json` 中配置 `noEmitOnError` 即可。

## 基础

### 原始数据类型

TypeScript的原始数据类型和JavaScript的原始数据类型相同，包含：布尔值、数值、字符串、`null`、`undefined`。

