## 对象

### 定义

对象（object）是JavaScript的核心概念，也是最重要的数据类型。JavaScript的所有数据都可以被视为对象。

简单说，所谓对象，就是一种无序的数据集合，由若干个“键值对”（key-value）构成。

```javascript

var o = {
  p: "Hello World"
};

```

上面代码中，大括号就定义了一个对象，它被赋值给变量o。这个对象内部包含一个键值对（又称为“成员”），p是“键名”（成员的名称），字符串“Hello World”是“键值”（成员的值）。键名与键值之间用冒号分隔。如果对象内部包含多个键值对，每个键值对之间用逗号分隔。

### 键名

对象的所有键名都是字符串，所以加不加引号都可以。上面的代码也可以写成下面这样。

```javascript
var o = {
  "p": "Hello World"
};
```

但是，如果键名不符合标识名的条件（比如第一个字符为数字，或者含有空格或运算符），也不是数字，则必须加上引号。

```javascript
var o = {
  "1p": "Hello World",
  "h w": "Hello World",
  "p+q": "Hello World"
};
```

上面对象的三个键名，都不符合标识名的条件，所以必须加上引号。

如果键名是数字，则会默认转为对应的字符串。

```javascript
var obj = {
  1e2: true,
  1e-2: true,
  .234: true,
  0xFF: true,
};

Object.keys(obj)
// [ '100', '255', '0.01', '0.234' ]
```

上面代码表示，如果键名为数值，则会先转为标准形式的数值，然后再转为字符串。

### 属性

对象的每一个“键名”又称为“属性”（property），它的“键值”可以是任何数据类型。如果一个属性的值为函数，通常把这个属性称为“方法”，它可以像函数那样调用。

```javascript

var o = {
  p: function(x) {return 2*x;}
};

o.p(1)
// 2

```

上面的对象就有一个方法p，它就是一个函数。

对象的属性之间用逗号分隔，ECMAScript 5规定最后一个属性后面可以加逗号（trailing comma），也可以不加。

```javascript
var o = {
  p: 123,
  m: function () { ... },
}
```

上面的代码中m属性后面的那个逗号，有或没有都不算错。但是，ECMAScript 3不允许添加逗号，所以如果要兼容老式浏览器（比如IE 8），那就不能加这个逗号。

由于对象的方法就是函数，因此也有`name`属性。

```javascript
var obj = {
  m1: function m1() {},
  m2: function () {}
};

obj.m1.name // m1
obj.m2.name // undefined
```

### 生成方法

对象的生成方法，通常有三种方法。除了像上面那样直接使用大括号生成（{}），还可以用new命令生成一个Object对象的实例，或者使用Object.create方法生成。

```javascript

var o1 = {};
var o2 = new Object();
var o3 = Object.create(null);

```


### 读写属性

**（1）读取属性**

读取对象的属性，有两种方法，一种是使用点运算符，还有一种是使用方括号运算符。

```javascript

var o = {
  p: "Hello World"
};

o.p // "Hello World"
o["p"] // "Hello World"

```

上面代码分别采用点运算符和方括号运算符，读取属性p。

请注意，如果使用方括号运算符，键名必须放在引号里面，否则会被当作变量处理。但是，数字键可以不加引号，因为会被当作字符串处理。

```javascript

var o = {
  0.7: "Hello World"
};

o.["0.7"] // "Hello World"
o[0.7] // "Hello World"

```

方括号运算符内部可以使用表达式。

```javascript

o['hello' + ' world']
o[3+3]

```

数值键名不能使用点运算符（因为会被当成小数点），只能使用方括号运算符。

```javascript
obj.0xFF
// SyntaxError: Unexpected token
obj[0xFF]
// true
```

上面代码的第一个表达式，对数值键名0xFF使用点运算符，结果报错。第二个表达式使用方括号运算符，结果就是正确的。

**（2）检查变量是否声明**

如果读取一个不存在的键，会返回undefined，而不是报错。可以利用这一点，来检查一个变量是否被声明。

```javascript

// 检查a变量是否被声明

if(a) {...} // 报错

if(window.a) {...} // 不报错
if(window['a']) {...} // 不报错

```

上面的后二种写法之所以不报错，是因为在浏览器环境，所有全局变量都是window对象的属性。window.a的含义就是读取window对象的a属性，如果该属性不存在，就返回undefined，并不会报错。

需要注意的是，后二种写法有漏洞，如果a属性是一个空字符串（或其他对应的布尔值为false的情况），则无法起到检查变量是否声明的作用。正确的写法是使用in运算符。

```javascript

if('a' in window) {
  ...
}

```

**（3）写入属性**

点运算符和方括号运算符，不仅可以用来读取值，还可以用来赋值。

```javascript

o.p = "abc";
o["p"] = "abc";

```

上面代码分别使用点运算符和方括号运算符，对属性p赋值。

JavaScript允许属性的“后绑定”，也就是说，你可以在任意时刻新增属性，没必要在定义对象的时候，就定义好属性。

```javascript

var o = { p:1 };

// 等价于

var o = {};
o.p = 1;

```

**（4）查看所有属性**

查看一个对象本身的所有属性，可以使用Object.keys方法。

```javascript

var o = {
  key1: 1,
  key2: 2
};

Object.keys(o);
// ["key1", "key2"]

```

### 属性的删除

删除一个属性，需要使用delete命令。

```javascript

var o = { p:1 };

Object.keys(o) // ["p"]

delete o.p // true

o.p // undefined

Object.keys(o) // []

```

上面代码表示，一旦使用delete命令删除某个属性，再读取该属性就会返回undefined，而且Object.keys方法返回的该对象的所有属性中，也将不再包括该属性。

麻烦的是，如果删除一个不存在的属性，delete不报错，而且返回true。

```javascript

var o = {};

delete o.p // true

```

上面代码表示，delete命令只能用来保证某个属性的值为undefined，而无法保证该属性是否真的存在。

只有一种情况，delete命令会返回false，那就是该属性存在，且不得删除。

```javascript

var o = Object.defineProperty({}, "p", {
        value: 123,
        configurable: false
});

o.p // 123
delete o.p // false

```

上面代码之中，o对象的p属性是不能删除的，所以delete命令返回false（关于Object.defineProperty方法的介绍，请看《标准库》一章的Object对象章节）。

另外，需要注意的是，delete命令只能删除对象本身的属性，不能删除继承的属性（关于继承参见《面向对象编程》一节）。delete命令也不能删除var命令声明的变量，只能用来删除属性。

### 对象的引用

如果不同的变量名指向同一个对象，那么它们都是这个对象的引用，也就是说指向同一个内存地址。修改其中一个变量，会影响到其他所有变量。

```javascript

var o1 = {};
var o2 = o1;

o1.a = 1;
o2.a // 1

o2.b = 2;
o1.b // 2

```

上面代码之中，o1和o2指向同一个对象，因此为其中任何一个变量添加属性，另一个变量都可以读写该属性。

但是，这种引用只局限于对象，对于原始类型的数据则是传值引用，也就是说，都是值的拷贝。

```javascript

var x = 1;
var y = x;

x = 2;
y // 1

```

上面的代码中，当x的值发生变化后，y的值并不变，这就表示y和x并不是指向同一个内存地址。

### in运算符

in运算符用于检查对象是否包含某个属性（注意，检查的是键名，不是键值），如果包含就返回true，否则返回false。

```javascript
var o = { p: 1 };
'p' in o // true
```

该运算符对数组也适用。

```javascript
var a = ["hello", "world"];

0 in a // true
1 in a // true
2 in a // false

'0' in a // true
'1' in a // true
'2' in a // false
```

上面代码表示，数字键0和1都在数组之中。由于数组是一种特殊对象，而对象的键名都是字符串，所以字符串的”0“和”1“，也是数组的键名。

在JavaScript语言中，所有全局变量都是顶层对象（浏览器的顶层对象就是window对象）的属性，因此可以用in运算符判断，一个全局变量是否存在。

```javascript
// 假设变量x未定义

// 写法一：报错
if (x){ return 1; }

// 写法二：不正确
if (window.x){ return 1; }

// 写法三：正确
if ('x' in window) { return 1; }
```

上面三种写法之中，如果x不存在，第一种写法会报错；如果x的值对应布尔值false（比如x等于空字符串），第二种写法无法得到正确结果；只有第三种写法，才能正确判断变量x是否存在。

in运算符的一个问题是，它不能识别对象继承的属性。

```javascript
var o = new Object();
o.hasOwnProperty('toString') // false

'toString' in o // true
```

上面代码中，toString方法不是对象o自身的属性，而是继承的属性，hasOwnProperty方法可以说明这一点。但是，in运算符不能识别，对继承的属性也返回true。

### for...in循环

for...in循环用来遍历一个对象的全部属性。

```javascript

var o = {a:1, b:2, c:3};

for (i in o){
  console.log(o[i]);
}
// 1
// 2
// 3

```

注意，for...in循环遍历的是对象所有可enumberable的属性，其中不仅包括定义在对象本身的属性，还包括对象继承的属性。

```javascript

// name是Person本身的属性
function Person(name) {
  this.name = name;
}

// describe是Person.prototype的属性
Person.prototype.describe = function () {
  return 'Name: '+this.name;
};

var person = new Person('Jane');

// for...in循环会遍历实例自身的属性（name），
// 以及继承的属性（describe）
for (var key in person) {
  console.log(key);
}
// name
// describe

```

上面代码中，name是对象本身的属性，describe是对象继承的属性，for-in循环的遍历会包括这两者。

如果只想遍历对象本身的属性，可以使用hasOwnProperty方法，在循环内部做一个判断。

```javascript

for (var key in person) {
    if (person.hasOwnProperty(key)) {
        console.log(key);
    }
}
// name

```

为了避免这一点，可以新建一个继承null的对象。由于null没有任何属性，所以新对象也就不会有继承的属性了。

## 类似数组的对象

在JavaScript中，有些对象被称为“类似数组的对象”（array-like object）。意思是，它们看上去很像数组，可以使用length属性，但是它们并不是数组，所以无法使用一些数组的方法。

下面就是一个类似数组的对象。

```javascript

var a = {
    0:'a',
    1:'b',
    2:'c',
    length:3
};

a[0] // 'a'
a[2] // 'c'
a.length // 3

```

上面代码的变量a是一个对象，但是看上去跟数组很像。所以只要有数字键和length属性，就是一个类似数组的对象。当然，变量a无法使用数组特有的一些方法，比如pop和push方法。而且，length属性不是动态值，不会随着成员的变化而变化。

```javascript

a[3] = 'd';

a.length // 3

```

上面代码为对象a添加了一个数字键，但是length属性没变。这就说明了a不是数组。

典型的类似数组的对象是函数的arguments对象，以及大多数DOM元素集，还有字符串。

```javascript

// arguments对象
function args() { return arguments }
var arrayLike = args('a', 'b');

arrayLike[0] // 'a'
arrayLike.length // 2
arrayLike instanceof Array // false

// DOM元素集
var elts = document.getElementsByTagName('h3');
elts.length // 3
elts instanceof Array // false

// 字符串
'abc'[1] // 'b'
'abc'.length // 3
'abc' instanceof Array // false

```

通过函数的call方法，可以用slice方法将类似数组的对象，变成真正的数组。

```javascript

var arr = Array.prototype.slice.call(arguments);

```

遍历类似数组的对象，可以采用for循环，也可以采用数组的forEach方法。

```javascript

// for循环
function logArgs() {
    for (var i=0; i<arguments.length; i++) {
        console.log(i+'. '+arguments[i]);
    }
}

// forEach方法
function logArgs() {
    Array.prototype.forEach.call(arguments, function (elem, i) {
        console.log(i+'. '+elem);
    });
}

```
