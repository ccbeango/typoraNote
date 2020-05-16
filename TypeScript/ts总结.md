##### 静态类型

```typescript
const count: number = 2020;

count.toFixed();

interface Point {
  x: number
  y: number
}
const point: Point = { x: 1, y: 10 };
```

对于静态类型，除了要知道count是静态类型`number`外，还要知道`count`具备`number`类型上具备的所有属性和方法，IDE正是利用这点给我们进行了很好的提示

`point`同理，除了是`Point`类型外，还具备Point类型上的所有属性和方法。

当类型确定以后，不仅意味着类型不能再修改，也意味着属性和方法已经确定了。

如果静态类型的声明和注解在一行，可以省略注解，TS会帮助我们自动推断，但是如果不在同一行，就需要进行类型注解，TS无法进行自行推断。

```typescript
const count = 1; // OK 可推断是number

let count; 
count = 1; // NO count此时的类型是any

// 不在同一行时，类型注解不可省略
let count: number;
count = 1;
```

##### 类型注解和类型推断

```typescript
// 类型注解
let count: number;
count: 123;

// 类型推断
let countInference = 123;
```

类型注解：我们来告诉TS变量是什么类型

类型推断：TS会自动的去尝试分析变量的类型

如果TS能够自动分析变量类型，我们就什么也不需要做了

如果TS无法分析变量类型的话，我们就需要使用类型注解

```typescript
// 不需要注解，TS会自动推断出来
const firstNumber = 1;
const secondNumber = 2;
const total = firstNumber + secondNumber;

// 需要注解，TS并不知道你传入的是什么类型
function getTotal(fitstNumber: number, secondNumber: number) {
  return fitstNumber + secondNumber;
}
getTotal(1, 10);
```

TS做的事情其实就是让代码上的变量或属性都拥有具体的类型。TS看不出来的，就需要我们自己去加类型注解。

##### 函数

在函数中，虽然有时候TS会帮助我们推断出返回值类型，但是我们还是应该加上返回值类型，这样可以避免一些不必要的错误。

```typescript
function add(first: number, second: number): number {
  return first + second;
}

const total = add(1, 2);
```

加入上面的代码不加返回值number，我们希望返回的值是`number`，但由于疏忽写错了函数，就会造成返回值是字符串，而TS无法帮助我们判断出来的情况。

```typescript
function add(first: number, second: number) {
  return first + second + '';
}

// 此时total是字符串类型，而不是我们想要的number类型
const total = add(1, 2);
```

##### 数组

```typescript
const arr: (number | string)[] = [1, '2', 3];
const stringArr: string[] = ['a', 'b', 'c'];
const undefinedArr: undefined[] = [undefined];
```

对象数组类型的注解

```typescript
cosnt objectArr: { name: string, age: number }[] = [
  {
    name: 'Tom',
    age: 28
  }
];
```

这样写可读性会很差，这时候可以使用类型别名`type alias`

```typescript
// type alias 类型别名
type User = { name: string, age: number };

cosnt objectArr: User[] = [
  {
    name: 'Tom',
    age: 28
  }
];
```

##### 元组

一个数组，长度是固定的，每个元素的类型也是固定的，这时候就可以使用元组。

```typescript
// 元组 tuple
const teacherInfo: [string, string, number] = ["Dell", "male", 18];
// csv
const teacherList: [string, string, number][] = [
  ["dell", "male", 19],
  ["sun", "female", 26],
  ["jeny", "female", 38]
];
```

##### 接口

有一些通用类型的集合，我们可以使用interface去把它表示出来。

```typescript
interface Person {
  name: string;
};

type Person1 = string;

const getPersonName = (person: Person):void => {
  console.log(person.name);
}

const setPersonName = (person: Person, name: string):void => {
  person.name = name;
}

const preson = {
  name: 'dell',
  sex: 'male'
};

// 正常
getPersonName(person);
// 报错 会进行强校验
getPersonName({
  name: 'dell',
  sex: 'male'
});
```

接口和类型别名的区别不大，区别如，`type Person1 = string`可以直接使用基础的静态类型，但是接口就没有办法代表。

在TS中，如果能够使用接口去描述一些类型的话，就是用接口去描述，实在不行才用类型别名。

接口在编译之后的文件中其实是不存在的，是帮助我们在写TS时进行语法提示和校验的。

##### 类

子类在继承父类之后，可以对父类中的方法进行重写，重写之后如果还想调用父类中的方法，可以使用`super`进行调用。

```typescript
class Person {
  name = 'dell';
  getName() {
    return this.name;
  }
}

class Teacher extends Person {
  getTeacherName() {
    return 'Teacher';
  }
  getName() {
    return super.getName() + 'lee';
  }
}

```

在TS中访问类型有：

* `public` 允许我在类的内外被调用

* `private` 允许在类内被使用
* `protected` 允许在类内及继承的子类中使用

子类在继承父类时候，要手动调用`super()`来执行父类的构造器。

利用`getter`和`setter`可以对数据的存储和读取进行重写，利用了JS的访问器属性中的方法。比如下面的例子，

```typescript
// getter and setter
class Person {
  constructor(private _name: string) {}
  get name() {
    return this._name + ' lee';
  }
  set name(name: string) {
    const realName = name.split(' ')[0];
    this._name = realName;
  }
}

const person = new Person('dell');
console.log(person.name);
person.name = 'dell lee';
console.log(person.name);
```

单例模式，一个类只允许通过这个类获取一个这个类的实例：

```typescript
class Demo {
  private static instance: Demo;
  private constructor(public name: string) {}

  static getInstance() {
    if (!this.instance) {
      this.instance = new Demo('dell lee');
    }
    return this.instance;
  }
}
const demo0 = new Demo(); // error 
const demo1 = Demo.getInstance();
const demo2 = Demo.getInstance();
console.log(demo1.name);
console.log(demo2.name);
```

此时的`demo1`和`demo2`是同一个实例。使用静态方法，去调用此静态方法的时候，保证了只生成了一次这个实例。



抽象类

```typescript
抽象类
abstract class Geom {
  width: number;
  // 可以定义自己的方法
  getType() {
    return 'Gemo';
  }
  // 抽象方法，继承他的类要实现抽象方法
  abstract getArea(): number;
}

class Circle extends Geom {
  getArea() { // 对抽象方法进行实现
    return 123;
  }
}

class Square {}
class Triangle {}
```

##### 联合类型

一个函数可以传入`number`或`string`类型的参数。这个时候实现函数通常需要进行类型保护。

```typescript
interface Bird {
  fly: boolean
  sing:() => {}
}

interface Dog {
  fly: boolean
  bark: () => {}
}

// 类型保护
// 类型断言 进行类型保护
function trainAnimal(animal: Bird | Dog) {
  if(animal.fly) {
    (animal as Bird).sing();
  } else {
    (animal as Dog).bark();
  }
}

// in 语法方式 进行类型保护
function trainAnimalSecond(animal: Bird | Dog) {
  if('sing' in animal)  {
    animal.sing();
  } else {
    animal.bark();
  }
}

// typeof 语法方式 进行类型保护
function add(first: string | number, second: string | number) {
  if(typeof first === 'string' || typeof second === 'string') {
    return `${first}${second}`;
  }
  return first + second;
}

// instanceof 语法方式 进行类型保护
class NumberObj {
  count!: number;
}

function addSecond(first: object | NumberObj, second: object | NumberObj) {
  if(first instanceof NumberObj && second instanceof NumberObj) {
    return first.count + second.count;
  }
  return 0;
}
```

##### 枚举

`Direction.Up`的值为`1`，`Down`为`2`，`Left`为`3`，`Right`为`4`。

```typescript
enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}
```

##### 泛型

泛型和普通的类型使用基本是一样的，不过是在调用的时候才确认具体的类型。

在函数中使用泛型：

 ```typescript
function join<T>(first: T, second: T) {
  return `${first}${second}`
}
join<number>(1,1);

function joinSecond<T, P>(first: T, second: P) {
  return `${first}${second}`
}
joinSecond(123, '123');

function joinThird<T>(first: T, second: T): T {
  return first;
}

function map<T>(params: T[]) {
  return params;
}
map<string>(['123']);
 ```

在类中使用泛型：

```typescript
class DataManager<T> {
  constructor(private data: T[]) {}
  getItem(index: number): T {
    return this.data[index].name;
  }
}

const data = new DataManager<number>([1]);
data.getItem(0);
```

还可以对泛型进行泛型约束：

```typescript
interface Item {
  name: string;
}

class DataManager<T extends Item> {
  constructor(private data: T[]) {}
  getItem(index: number): T {
    return this.data[index];
  }
}

// 泛型约束后，传入的值中必须有Item接口中的字段
const data = new DataManager([
  {
    name: 'tom'
  }
]);


class AnotherDataManager<T extends number | string> {
  constructor(private data: T[]) {}
  getItem(index: number): T {
    return this.data[index];
  }
}

// 泛型约束后，传入的值中必须有Item接口中的字段
const anotherData = new AnotherDataManager([
  1, 2,'3'
]);


// 使用泛型作为一个具体的类型注解
function hello<T>(params: T) {
  return params;
}

const func: <T>(param: T) => T = hello;
```

##### 命名空间

```typescript

// components.ts
namespace Components {
  export class Header {
    constructor() {
      const elem = document.createElement('div');
      elem.innerText = 'This is Header';
      document.body.appendChild(elem);
    }
  }
  
  export class Content {
    constructor() {
      const elem = document.createElement('div');
      elem.innerText = 'This is Content';
      document.body.appendChild(elem);
    }
  }
  
  export class Footer {
    constructor() {
      const elem = document.createElement('div');
      elem.innerText = 'This is Footer';
      document.body.appendChild(elem);
    }
  }
}
```

在另一个文件的命名空间中使用上面的：

```typescript
///<reference path='./component.ts' />

// page.ts
namespace Home {
  export class Page {
    constructor() {
      new Components.Header();
      new Components.Content(); 
      new Components.Footer();
    }
  }
}
```

上面的代码，如果没有三斜杠指令，也是没有问题的，但是相互引用的关系会难以看出来。

使用三斜杠指令可以让命名空间之间的引用关系更明确。上述代码说明`Home`命名空间是依赖于`Components`命名空间的。

##### 代码模块化

使用`import`导入，使用`export`导出

##### 全局类型描述文件

定义Jquery的全局描述文件中的变量和函数：

```typescript
// 定义全局变量
// declare var $: (param:() => void) => void; 

// 定义全局函数 
interface JqueryInstance {
  html: (html: string) => JqueryInstance;
}

// 如果即是函数又是对象，就用这种语法
// 使用函数重载定义 
declare function $(readyFunc: () => void): void;
declare function $(selector: string): JqueryInstance;


// 如果只是函数，使用这种语法是可以的
// 使用interface语法实现函数重载 
interface JQuery {
  (readyFunc: () => void): void;
  (selector:string):JqueryInstance
}
declare var $: JQuery

// 对对象进行类型定义、对类型进行类型定义、命名空间的嵌套
declare namespace $ {
  namespace fn {
    class init {}
  }
}
```

使用

```typescript
$(function() {
  $('body').html('<div>hello world</div>');

  new $.fn.init();
});


```

定义模块代码的类型描述文件：

以es6为例，对上面的描述文件进行模块化

```typescript
// es6模块化
declare module 'jquery' {
  interface JqueryInstance {
    html: (html: string) => JqueryInstance;
  }

  function $(readyFunc: () => void): void;
  function $(selector: string): JqueryInstance;

  namespace $ {
    namespace fn {
      class init { }
    }
  }
}
```

##### keyof的使用

```typescript
interface Person {
  name: string;
  age: number;
  gender: string;
}

class Teacher {
  constructor(private info: Person) {}
  getInfo<T extends keyof Person>(key: T): Person[T] {
    return this.info[key];
  }
}

const teacher = new Teacher({
  name: 'dell',
  age: 18,
  gender: 'male'
});

const test = teacher.getInfo('name');
console.log(test);
```

`T extends keyof Person`实际上是对接口`Person`上的每个属性值进行遍历，三次遍历返回的内容其实就是

```typescript
T extends 'name' // type T = 'name'
T extends 'age'
T extends 'gender'
```

而`T extends 'name'`等价于`type T = 'name'`，其他同理。

`keyof`可以让类型是一个固定的字符串，就像是类型别名的循环遍历。

##### 使用express

express现在还是使用js实现的，库的类型定义文件.d.ts文件类型描述不准确，如接口`Request`中`ReqBody`定义的类型为`any`

```typescript
export interface Request<P extends Params = ParamsDictionary, ResBody = any, ReqBody = any> extends http.IncomingMessage, Express.Request {
    // ....
}

// 我们可以自己在进行接口补充
interface RequestWithBody extends Request {
  body: {
    [key: string]: string | undefined;
  }
}
```

还有问题是，当我们使用中间件的时候，对`req`或者`res`做了修改之后，实际上类型并不能改变。

 比如使用某个中间件之后给req加上了一个字段`req.hello = 'req';`，但是实际上要去使用的时候，是没有这个字段的ts会报错。

```typescript
// 自定义一个中间件
app.use((req: Request, res: Response, next: NextFunction) => {
  // 不存在，会报错
  req.hello = 'hello world';
  next();
});
```

这是`hello`报错，可以使用类型融合进行类型的扩展。

```typescript
// 定义一个.d.ts文件，根据express中定义的规则进行扩展就可以了
declare namespace Express {
  interface Request {
    hello: string
  }
}
```

这样就解决了一些中间件报错的问题，在其他地方就可以正常使用。



##### 装饰器

装饰器本身是一个函数，装饰器通过`@`符号类使用。

###### 类装饰器

类装饰器表达式会在运行时当作函数被调用，类的构造函数作为其唯一的参数。

装饰器的写法：

```typescript
function testDecorator() { // 工厂函数
  return function (constructor: any) { // 装饰器函数
    constructor.prototype.getName = () => {
      console.log('Name Tom from decorator');
    }
  }

}

@testDecorator()
class Test { }

const test = new Test();
(test as any).getName();
```

上面的写法虽然容易理解，但并不是一个良好的类装饰器的写法，引用的时候回出现定义找不到的情况，其实是不推荐的，推荐的是下面的写法。

```typescript
function testDecorator<T extends new(...args:any[]) => {}>(constructor: T) { // 工厂函数
  return class extends constructor  { // 装饰器函数
    
  }
}
```

`new (...args:any[]) => {}`的含义：`(...args:any[]) => {}`这个函数接收任意多个参数，参数是一个数组，数组中的每一项都是`any`，和`new`组合之后表示这是一个构造函数。

这是一个接收任意多个参数的构造函数且返回类型是一个对象的一种写法。

`<T extends new (...args:any[]) => {}>`就表示`T`可以通过后面这种构造函数被实例化出来。所以`T`就可以理解成一个类或者包含构造函数的一个结构。

类型`T`中一定包含一个接收任意多个参数，最终返回一个`{}`的构造函数。

与上面的含义相同，另一种写法是：

```typescript
function testDecorator<T extends {new(...args:any[]):{}}>(constructor: T) { 
  return class extends constructor  { 
    
  }
}
```

所以重载构造函数的例子就可以是这样的：

```typescript
function testDecorator<T extends new (...args:any[]) => {}>(constructor: T) { // 工厂函数
  return class extends constructor  { // 装饰器函数
    hello = 'lee';

    getName(){
      return this.hello;
    }
  }
}

@testDecorator
class Test {
  hello: string;
  constructor(hello: string) {
    this.hello = hello;
  }
}

const test = new Test('world');
console.log(test);
test.getName(); // 会提示错误，装饰器装饰后是找不到这个函数的
```

想要解决这个问题，可以换另外一种工厂函数的写法

```typescript

function testDecorator() {
  return function <T extends new (...args: any[]) => {}>(constructor: T) { // 工厂函数
    return class extends constructor { // 装饰器函数
      hello = 'lee';

      getName() {
        return this.hello;
      }
    }
  }
}



const Test = testDecorator()(
  class {
    hello: string;
    constructor(hello: string) {
      this.hello = hello;
    }
  })

const test = new Test('world');
console.log(test);
test.getName();
```

装饰器装饰过这个类之后再返回，就可以找得到`getName()`方法了。

当需要在装饰器中对类进行方法的扩展时，需要使用上面的工厂函数形式，对类进行一下装饰器的修饰，然后返回一个新的类，给到`Test`，在调用的时候，就可以找到对应的方法了。

###### 类中方法装饰器

方法装饰器表达式会在运行时当作函数被调用，传入下列3个参数：

1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象(prototype)。
2. 成员的名字。
3. 成员的属性描述符。

```typescript

function getNameDecorator(target: any, key: string, descriptor: PropertyDescriptor) {
  console.log(target, key, descriptor);
  // 对属性进行修改
  descriptor.value = function() {
    return 'decorator';
  }
}


class Test {
  hello: string;
  constructor(hello: string) {
    this.hello = hello;
  }

  @getNameDecorator
  getName() {
    return this.hello;
  }
}

const test = new Test('world');
console.log(test);
```

###### 访问器的装饰器

接收的参数与方法装饰器一样。

访问器属性只能作用于`get`或`set`中的一个。这是因为，在装饰器应用于一个属性描述符时，它联合了`get`和`set`访问器，而不是分开声明的

```typescript

function visitDecorator(target: any, key: string, descriptor: PropertyDescriptor) {
  descriptor.writable = false;
}


class Test {
  private hello: string;
  constructor(hello: string) {
    this.hello = hello;
  }

  
  get name() {
    return this.hello;
  }

  @visitDecorator
  set name(hello: string) {
    this.hello = hello;
  }
}

const test = new Test('world');
test.name = 'hello';
console.log(test.name);
```

上述代码运行会报错，因为`test.name`在装饰器中对属性描述符中的`writeable`进行了修改`false`。

###### 属性装饰器

属性装饰器表达式会在运行时当作函数被调用，传入下列2个参数：

1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
2. 成员的名字。

如果访问符装饰器返回一个值，它会被用作方法的*属性描述符*。

```typescript
// 返回值会作为name的属性描述符
function nameDecorator(target: any, key: string): any {
  const descriptor: PropertyDescriptor = {
    writable: false
  };
  return descriptor;
}

class Test {
  @nameDecorator
  name = 'Dell';
}

// 此时name不能修改
const test = new Test();
test.name = 'Tom'; // 报错
```

再看下面的例子，直接修改原型对象上的name，是无法做到修改name的值的：

```typescript
// 修改的并不是实例上的 name， 而是原型上的 name
function nameDecorator(target: any, key: string): any {
  target[key] = 'Tom';
}

// name 放在实例上
class Test {
  @nameDecorator
  name = 'Dell';
}

const test = new Test();
console.log(test.name); // Dell
console.log((test as any).__proto__.name); // Tom
```

首先装饰器会修改原型链上的name的值为`Tom`，而实例Test本身的name会屏蔽掉原型链上的值。

###### 参数装饰器

参数装饰器表达式会在运行时当作函数被调用，传入下列3个参数：

1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
2. 成员的名字。
3. 参数在函数参数列表中的索引。

```typescript
function paramDecorator(target: any, key: string, paramIndex: number) {
  console.log(target, key, paramIndex);
}


class Test {
  getInfo(@paramDecorator name: string, age: number) {
    console.log(name, age);
  }
}

const test = new Test();
test.getInfo('Tom', 18);
```

###### 装饰器使用demo

当我们运行这个代码的时候，会报错，因为userInfo是未定义的。

```typescript
const userInfo: any = undefined;

class Test {
  getName() {
    return userInfo.name;
  }

  getAge() {
    return userInfo.age;
  }
}

const test = new Test();

test.getName();
```

按照之前的解决办法，我们可以使用`try catch`语法进行修改：

```typescript
const userInfo: any = undefined;

class Test {
  getName() {
    try {
      return userInfo.name;
    } catch (error) {
      console.log('userInfo.name 不存在');
    }
  }

  getAge() {
    try {
      return userInfo.age;
    } catch (error) {
      console.log('userInfo.age 不存在');
    }
  }
}

const test = new Test();

test.getName();
```

如果这类方法很多，每一个我们要这样写，这时我们可以使用装饰器解决这个问题：

```typescript
const userInfo: any = undefined;

function catchError(msg: string) {
  return function(target: any, key: string, descriptor: PropertyDescriptor) {
    // 属性描述符中的value就是getName的函数方法
    const fn = descriptor.value;
    try {
      fn();
    } catch (error) {
      console.log(msg);
    }
  }
}

class Test {
  @catchError('userInfo.name')
  getName() {
    return userInfo.name;
  }

  @catchError('userInfo.age')
  getAge() {
    return userInfo.age;
  }
}

const test = new Test();

test.getName();
test.getAge();
```

装饰器工厂函数接收参数，并返回一个装饰器，在装饰器中对本身的函数进行装饰，解决了些一些重复代码的情况。