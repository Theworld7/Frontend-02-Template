# 学习笔记

## Javascript 表达式

### Expressions

#### 运算符优先级

 * **Member** 运算，优先级最高
   * a.b
   * a[b]
   * foo\`string`
   * super.b
   * super['b']
   * new.target
   * new Foo()
* **New** 优先级比带括号的 new 优先级更低。
  * new Foo

 * **Call** ，优先级低于 Member 和 New 。`.` 运算的优先级视前面的语法结构来决定。
   * foo()
   * super()
   * foo()['b']
   * foo().b
   * foo()\`abc`

 * **Left Handside & Right Handside - 左手和右手运算**
   * `a.b = c` ，左手运算，`a + b = c` ，右手运算。

 * **Update** ，是 Right Hand Expression 。
   * a ++
   * a --
   * -- a
   * ++ a

 * **Unary - 单目运算符**
   * delete a.b
   * void foo()
   * typeof a
   * \+ a
   * \- a
   * ~ a
   * ! a
   * await a

 * Exponental
   * `**` - 乘方，右结合。

 * Multiplicative
   * `*` `/` `%`

 * Additive
   * `+` `-`
* Shift
  * `<<` `>>` `>>>`
* Relationship
  * `<` `>` `<=` `>=` `instanceof` `in`
* Equality
  * ==
  * ! =
  * ===
  * ! ==
* Bitwise
  * `&` `^` `|`
* Logical
  * &&
  * ||
* Conditional
  * `?` `:`



## 类型转换

|         | Null      | Undefined   | Boolean(true) | Boolean(false) | Number         | String         | Symbol    | Object   |
| ------- | --------- | ----------- | ------------- | -------------- | -------------- | -------------- | --------- | -------- |
| Boolean | FALSE     | FALSE       | -             | -              | 0 / NaN-false  | ""-false       | TRUE      | TRUE     |
| Number  | 0         | NaN         | 1             | 0              | -              | StringToNumber | TypeError | 拆箱转换 |
| String  | "null"    | "undefined" | TRUE          | FALSE          | NumberToString | -              | TypeError | 拆箱转换 |
| Object  | TypeError | TypeError   | 装箱转换      | 装箱转换       | 装箱转换       | 装箱转换       | 装箱转换  | -        |



### String to Number

​	字符串到数字的类型转换，存在一个语法结构，类型转换支持十进制、二进制、八进制和十六进制。

​	JavaScript 支持的字符串语法还包括**正负号科学计数法**，可以使用大写或小写的 e 来表示。

​	`parseInt` 和 `parseFloat` 并不使用这个转换，在不传入第二个参数的情况下，`parseInt` 只支持 16 进制前缀“0x”，而且会忽略非数字字符，也不支持科学计数法。

​	在一些古老的浏览器环境中，`parseInt` 还支持 0 开头的数字作为 8 进制前缀，这是很多错误的来源。所以在任何环境下，都建议传入 `parseInt` 的第二个参数，而 `parseFloat` 则直接把原字符串作为十进制来解析，它不会引入任何的其他进制。

​	多数情况下，Number 是比 `parseInt` 和 `parseFloat` 更好的选择。 



### NumberToString

​	在较小的范围内，数字到字符串的转换是完全符合你直觉的十进制表示。当 Number 绝对值较大或者较小时，字符串表示则是使用科学计数法表示的。 



### 装箱转换

​	每一种基本类型 Number、String、Boolean、Symbol 在对象中都有对应的类，所谓装箱转换，正是把基本类型转换为对应的对象。

​	全局的 Symbol 函数无法使用 new 来调用，可以利用一个函数的 call 方法来强迫产生装箱。

​	我们定义一个函数，函数里面只有 `return this` ，然后我们调用函数的 `call` 方法到一个 Symbol 类型的值上，这样就会产生一个 symbolObject 。

```javascript
const symbolObject = function () {
  return this
}.call(Symbol('a'))
console.log(typeof symbolObject) // object
console.log(symbolObject instanceof Symbol) // true
console.log(symbolObject.constructor == Symbol) // true
```

​	装箱机制会频繁产生临时对象，在一些对性能要求较高的场景下，我们应该尽量避免对基本类型做装箱转换。

​	使用内置的 Object 函数，我们可以在 JavaScript 代码中显式调用装箱能力。

```javascript
const symbolObject = Object(Symbol('a'))
console.log(typeof symbolObject) // object
console.log(symbolObject instanceof Symbol) // true
console.log(symbolObject.constructor == Symbol) // true
```

​	每一类装箱对象皆有私有的 Class 属性，这些属性可以用 `Object.prototype.toString` 获取。

​	在 JavaScript 中，没有任何方法可以更改私有的 Class 属性，因此 `Object.prototype.toString` 是可以准确识别对象对应的基本类型的方法，它比 `instanceof` 更加准确。

​	但需要注意的是，`call` 本身会产生装箱操作，所以需要配合 `typeof` 来区分基本类型还是对象类型。 



### 拆箱转换

​	`ToPrimitive` 函数，对象类型到基本类型的转换（即，拆箱转换）。

​	对象到 String 和 Number 的转换都遵循“先拆箱再转换”的规则。通过拆箱转换，把对象变成基本类型，再从基本类型转换为对应的 String 或者 Number。

​	拆箱转换会尝试调用 `valueOf` 和 `toString` 来获得拆箱后的基本类型。如果 `valueOf` 和 `toString` 都不存在，或者没有返回基本类型，则会产生类型错误 `TypeError`。

​	 到 String 的拆箱转换会优先调用 toString。

​	 ES6 之后，还允许对象通过显式指定 `toPrimitive Symbol` 来**覆盖原有的行为**。

```javascript
const o = {
  valueOf: () => {
    console.log('valueOf')
    return {}
  },
  toString: () => {
    console.log('toString')
    return {}
  }
}

o[Symbol.toPrimary] = () => {
  console.log('toPrimary')
  return 'hello'
}

console.log(o + '')
// toPrimary
// hello
```



### typeof 运算

​	`typeof` 的运算结果，与运行时类型的规定有很多不一致的地方。

| 实例表达式   | typeof 结果 | 运行时类型行为 |
| ------------ | ----------- | -------------- |
| null         | object      | Null           |
| {}           | object      | Object         |
| (function{}) | function    | Object         |
| 3            | number      | Number         |
| "ok"         | string      | String         |
| true         | boolean     | Boolean        |
| void 0       | undefined   | Undefined      |
| Symbol('a')  | symbol      | Symbol         |



## 运行时 Statement

### Completion Record

​	存储语句完成结果的数据结构，是否返回？返回结果？

#### [[type]]

* normal
* break
* continue
* return
* throw

#### [[value]]

​	基本类型

#### [[target]]

​	label，break 和 continue 会带一个 [[target]] 。



## 语句

### 简单语句

* ExpressionStatement：表达式语句，完全由表达式组成后面加一个分号
* EmptyStatement：只有一个分号

- DebuggerStatement：debugger关键字接一个分号

- ThrowStatement：抛出一个异常

- ContinueStatement：结束当次循环

- BreakStatement：结束整个循环

- ReturnStatement：返回函数值



### 组合语句

* BlockStatement：花括号中间语句列表，把所有单条语句的地方变成多一条语句

* IfStatement：条件语句，分支结构

  * while ( 表达式 ) 语句

  - do 语句 while ( 表达式 )

  - for ( 变量声明 ; 表达式 ; 表达式 ) 语句

  - for ( 变量声明 in 表达式 ) 语句

  - for ( 变量声明 of 表达式 ) 语句

  - for await ( 变量声明 of 表达式 ) 语句

* SwitchStatement：多分支结构，在Javascript中跟IfStatement性能相同

* IterationStatement：循环语句

* WidthStatement：通过width打开一个对象，把对象的所有属性直接放进作用域

* LabelledStatement：在语句前加一个label

* TryStatement：即使在 try 里 return ，catch 依旧会执行，try 不能省略花括号



### 声明

* FunctionDeclaration：`function`
* GeneratorDeclaration：`function *`
* AsyncFunctionDeclaration：异步函数声明 `async function`
* AsyncGeneratorDeclaration：异步产生器 `async function *`
* VariableStatement：`var`
* ClassDeclaration：`class`
* LexicalDeclaration：`const` 、`let`



### 预处理机制

​	无论 var 在哪声明都会被预处理机制找出来先执行，const 同样。

​	区别在于 const 在声明前使用会抛错。



### 作用域

​	var 和 function 声明作用域在它所在函数体内。

​	const 作用域在花括号里。



## 结构化

### 宏任务

​	传给 Javascript 引擎的任务。



### 微任务

​	Javascript 引擎内部的任务。`Promise.then()` 会产生一个微任务。



### 事件循环

​	独立线程执行。

* 获取代码
* 执行代码
* 等待，然后继续获取代码



### 函数调用

​	栈（Stack）里保存着执行上下文（Execution Context），保存执行语句时所需要的所有信息。保存执行上下文的数据结构称作执行上下文栈（Execution Context Stack）。

​	执行到当前语句时栈里有个栈顶元素，栈顶元素是当前能访问到的所有变量，这些变量叫 Running Execution Context ，代码所需的一切信息都从这里取。



#### 执行上下文

* code evaluation state ：用于 async 和 generator 函数，代码执行进度的保存信息。
* Function ：由 Function 初始化的执行上下文会有此字段。
* Script or Module
* Generator ：Generator 函数每次执行所生成的隐藏在背后的 Generator 。
* Realm ：保存所有使用的内置对象。
* LexicalEnvironment ：执行代码中所需要访问的环境。
* VariableEnvironment ：var 声明变量到哪的环境。



​	`Generator Execution Contexts` 比 `ECMAScript Code Execution Context` 多了一个 Generator 字段。



##### LexicalEnvironment 

* this
* new.target
* super
* 变量



##### VariableEnvironment 

​	历史遗留的包袱，仅仅用于处理 var 声明。



#### Environment Record

* Declarative Environment Records
  * Function Environment Records
  * module Environment Records
* Global Environment Records
* Object Environment Records



#### 函数闭包

##### 代码

​	由一个 object 和一个变量的序列组成。

##### 环境

​	每个函数都会带一个定义时所在的 Environment Records ，它会把 Environment Records 保存到自己的函数身上变成一个属性。



​	不论 foo2 被传到哪，都会带上`y = 2` 的 Environment Record 。

```javascript
var y = 2;
function foo2 () {
	console.log(y);
}
export foo2;
```

​	

​	foo2 执行会在内部产生一个闭包，产生一个`z = 3` 。foo2 所在环境 `y = 2` 被作为 `z = 3` 所在的环境的上级被保留下来，这就是 Environment Record 形成的链式结构。由于使用箭头函数，`z = 3` 执行时候所用的 this 也被保存下来 this 是 global ，所以这个 Environment Record 会有三条记录。

```javascript
var y = 2;
function foo2 () {
    var z = 3;
    return () => {
        console.log(y, z);
    }
}
var foo3 = foo2()
export foo3
```



#### Realm

  规定了在一个 Javascript 引擎的实例里面，所有的内置对象都会被放进一个 Realm 里面。不同的 Realm 实例之间是完全相互独立的。

​	在 JS 中，函数表达式和对象直接量均会创建对象。使用 . 做隐式转换也会创建对象。这些对象也有原型，如果没有 Realm，就不知道它们的原型是什么。