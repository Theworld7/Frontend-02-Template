## 泛用语言分类

### 形式语言（乔姆斯基谱系）

​	下面四种文法属于上下包含关系。

- 0 型：无限制文法
- 1 型：上下文相关文法
- 2 型：上下文无关文法
- 3 型：正则文法

### 非形式语言

​	没有严格的语法定义，中英文等。

---

## 产生式

​	巴斯克 - 诺尔范式（BNF）

- 用尖括号括起来的名称来表示语法结构名
- 语法结构分成基础结构和需要用其他语法结构定义的复合结构基础结构称终结符
  - 复合结构称非终结符
  - 弓|号和中间的字符表示终结符
- 可以有括号
- `*` 表示重复多次
- `|` 表示或
- `+` 表示至少一次



### 示例 - 四则运算

```
< MultiplicativeExpression> ::= < Number> |
  < MultiplicativeExpression> "*" < Number> |
  < MultiplicativeExpression> "/" < Number> |
< AddtiveExpression> ::= < MultiplicativeExpression> |
	< AddtiveExpression> "+" < MultiplicativeExpression> |
	<AddtiveExpression> "-" <MultiplicativeExpression> |
```



### 通过产生式理解乔姆斯基谱系

​	Javascript 总体属于上下文无关文法，表达式大部分属于正则文法；乘方 `**` 属于右结合，不属于正则文法。

​	Javascript 并非严格属于上下文无关文法，例如 `get` 右边接对象就是关键字，接 `:` 就是属性名，严格按照乔姆斯基谱系定义，Javascript 属于上下文相关文法。

- 0型无限制文法 `?::=?`
- 1型上下文相关文法 `?<A>?::=?<B>?`
- 2型上下文无关文法 `<A>::=?`
- 3型正則文法 `<A>::=<A>?`

---

## 现代语言分类

### 现代语言特例

​	C++ 中，`*` 可能表示乘号或者指针，具体是哪个，取决于星号前面的标识符是否被声明为类型。

​	VB 中，`<` 可能是小于号，也可能是 XML 直接量的开始，取决于当前位置是否可接受 XML 直接量。

​	Python 中，行首的 `tab` 符和空格会根据上一行的行首空白以一定规则被处理成虚拟中介符 indent 或者 dedent。

​	Javascript 中，`/` 可能是除号，也可能是正则表达式开头，处理方式类似于 VB ，字符串模版中也需要特殊处理 `}` ，还有自动插入分号规则。

### 语言的分类

#### 形式语言 - 用途

* 数据描述语言：

  描述或存储数据，不能编程。

  `JSON、HTML、XAML、SQL、CSS、SGML、XML`

* 编程语言：

  `C、C++、Java、C#、Python、Ruby、Perl、Lisp、T-SQL、Clojure、Haskell、Javascript、Swift、PHP、R、Visual Basic、Go、Assembly language、Rust`

#### 形式语言 - 表达方式

* 声明式语言：

  只告诉结果怎样。

  `JSON、HTML、XAML、SQL、CSS、Lisp、Clojure、Haskell`

* 命令式语言：

  达成结果的每个步骤是怎样的。

  `C、C++、Java、C#、Python、Ruby、Perl、Javascript、Fortran、ALGOL、COBOL、Ada、Pascal`

---

## 编程语言的性质

### 图灵完备性

* 命令式 - 图灵机
  * goto
  * if 和 while
* 声明式 - lambda
  * 递归



### 动态与静态

* 动态：
  * 在用户的设备/在线服务器上
  * 产品实际运行时
  * Runtime
* 静态：
  * 在程序员的设备上
  * 产品开发时
  * Compiletime



### 类型系统

* 动态类型系统与静态类型系统

  动态类型，在用户的机器、内存里能找到类型。

  静态类型，只在编写代码时能够保留的内存信息。

* 强类型与弱类型

  指类型转换的形式，Javascript 是弱类型。

  * String + Number ，Javascript 里会把 Number 主动转换成 String 再执行 `+` 操作。
  * String == Boolean

* 复合类型

  * 结构体
  * 函数签名
    * 参数类型
    * 返回值类型

* 子类型

* 泛型

  把类型当作参数传递给某一段代码 。

  * 逆变 / 协变，泛型和子类型结合产生。

---

## Number

​	IEEE 754 双精度浮点数。

​	浮点数，小数点可以来回浮动。

​	10 进制转换到 2进制会最多损失一个 epsilon。0.1 + 0.2 !== 0.3 由三次转换和一次运算的精度损失造成的，损失最大 3 epsilon。

### 表示法

* 1 符号位

* 11 指数位

  浮点数表示范围。

* 52 精度位

  精度以1开头。

### 语法

* 2 进制，以 0b 开头。
* 8 进制，以 0o 开头。
* 10 进制，允许有小数。
* 16 进制，以 0x 开头，0 - 9 用数字，10 开始使用 A - F 表示。

---

## String

​	最大长度为 2^53 - 1 ，该长度为字符串的 UTF-16 编码后的长度。

### Character - 字符

- ASCII

- Unicode

- UCS

- GB
  - GB2312
  - GBK(GB13000)
  - GB18030

- ISO8859

- BIG5

### Code Point - 码点

### Encoding - 编码方式

- UTF-8 ，一个字节兼容一个字

- UTF-16 ，两个字节表示一个字符

## Null

​	表示空值，值为 null ，是关键字。

## Undefined

​	表示未定义，是变量，为了防止无意中被篡改，建议用 void0 获取。

---

## Javascript 对象

### property - 属性

​	既可以描述状态，又可以描述行为。

#### 键

##### 		String

​		只要拿到对象实例，通过对应属性的 `string` 就可以拿到对应的属性。

##### 		Symbol

​		只能通过变量引用。即使名字相同的两个 `Symbol` 也是不相等的。

#### 值

##### 		数据属性

- writable

- enumerable

  ​	影响 `Object.keys()` 等内置函数的行为，也会影响 `forEach` 等语法产生的默认行为。

- configurable

##### 		访问器属性

- get
- set
- enumerable
- configurable

### protptype - 原型

​	当我们访问属性时，如果当前对象没有，则会沿着原型找原型对象是否有此名称的属性，而原型对象还可能有原型，因此，会有 “原型链” 这一说法。

​	这一算法保证了，每个对象只需要描述自己和原型的区别即可。



### 语法和 API

- `{}` `.` `[]` `Object.defineProperty`：基本对象机制。创建对象、访问属性和定义新属性，以及改变属性的特征值。
- `Object.create` `Object.setPrototypeOf` `Object.getPrototypeOf`：基于原型的描述对象方法。创建对象，修改对象，获取对象。
- `qnew` `class` `extends`：基于分类的方式描述对象。运行时会转换成 Javascript 原型相关的访问，但从语法上来说，它就是一个基于类的面向对象组织方式。
- `new` `function` `prototype`：不伦不类（不建议使用）。



### 特殊对象

- Array：Array 的 length 属性根据最大的下标自动发生变化。
- Object.prototype：作为所有正常对象的默认原型，不能再给它设置原型了。
- String：为了支持下标运算，String 的正整数属性访问会去字符串里查找。
- Arguments：arguments 的非负整数型下标属性跟对应的变量联动。
- 模块的 namespace 对象：特殊的地方非常多，跟一般对象完全不一样，尽量只用于 import 吧。
- 类型数组和数组缓冲区：跟内存块相关联，下标运算比较特殊。
- bind 后的 function：跟原来的函数相关联。



### 宿主对象

​	全局对象 window 上的属性，一部分来自 JavaScript 语言，一部分来自浏览器环境。

​	宿主对象也分为固有的和用户可创建的两种，比如 document.createElement 就可以创建一些 DOM 对象。

​	宿主也会提供一些构造器，比如我们可以使用 new Image 来创建 img 元素