# JavaScript历史

#### ECMAScript，即使所说的ES

European Computer Manufactures Association组织

ECMAScript在不断的发展，讲到JavaScript的版本，实际上就是说他实现了ECMAScript标准的哪个版本

---

JavaScript和Java类似，每个语句以 **;** 结束，语句块用 **{...}** 。但是JavaScript并不强制要求在每个语句的结尾加;，浏览器中负责执行JavaScript代码的引擎会自动在每个语句的结尾补上;。

#### JavaScript严格区分大小写

#### 数据类型和变量

Number：JavaScript不区分整数和浮点数，统一用Number表示
字符串：
布尔值：
NaN：
null：
undefined：建议仅仅在判断函数参数是否传递的情况下使用
数组：一组按顺序排列的集合，集合每个值称为元素，JavaScript的数组可以包括任意数据类型
对象：
变量：

#### 比较运算符

+ ==：会自动转换数据类型再比较，很多时候，会得到非常诡异的结果
+ ===：不会自动转换数据类型，如果数据类型不一致，返回false，如果一致，再进行比较

不建议使用==

#### NaN

是一个特殊的Number，与所有其他值不相等，包括自己。唯一能判断NaN的方法是通过`isNoN()`函数

浮点数的比较，浮点数在运算过程中会产生误差，计算机无法精确表示无限循环小数，要比较两个浮点数是否相等，只能计算它们之差的绝对值，看是否小于某个阈值

#### 数组

```javascript
[1, 2, 3.14, 'Hello', null, true];
new Array(1, 2, 3);
```

超过索引的数组元素返回的是undefined

#### 对象

对象是一组由键-值组成的无序集合

```javascript
var person = {
    name: 'Bob',
    age: 20,
    tags: ['js', 'web', 'mobile'],
    city: 'Beijing',
    hasCar: true,
    zipcode: null
};
person.name; //'Bob'
person.zipcode; //null
```

键都是字符串类型，值可以是任意数据类型，每个键又称为对象的属性

---

#### Strict模式

JavaScript设计之初，为了方便初学者学习，不强制要求用var声明变量，如果一个变量没有通过var声明就被使用，那么该变量就自动被声明为全局变量

一个页面的不同JavaScript文件的全局变量会互相冲突

使用`'use strict';`在第一行写上

---

#### 字符串

ASCII字符可以使用`\x##`形式的十六进制表示
Unicode字符可以使用`\u####`表示

多行字符串用反引号表示`

**模板字符串**

```javascript
var name = '小明';
var age = 20;
var message = `你好，${name}, 您今年${age}岁了！`;
alert(message);
```

**操作字符串**

```javascript
var s = 'Hello, world!';
s.length; //13
s[0]; //'H'
s[12]; //'!'
s[13]; //undefined超出范围的索引不会报错，但一律返回undefined
```

字符串是不可变的，如果对某个索引赋值，不会又任何错误，也不会又任何效果

JavaScript为字符串提供的一些常用方法不会改变原有字符串的内容，**而是返回一个新字符串**

```javascript
var s = 'Hello, world!';
s.toUpperCase();
var lower = s.toLowerCase();
s.indexOf('world'); // 7
s.indexOf('World'); // -1
s.substring(0, 5); // 'hello'
s.substring(7); // 'world!'
```

#### 数组

Array可以包含任意数据类型，并通过索引来访问每个元素

```javascript
var arr = [1, 2, 3.14, 'Hello', null, true];
arr.length; //6
var a = [1, 2, 3];
a.length = 6;
a; // a变为[1, 2, 3, undefined, undefined, undefined]
a.length = 2;
a; // a变为[1, 2]
var b = ['A', 'B', 'C'];
b[1] = 99;
b; // ['A', 99, 'C']
b.indexOf('C'); // 2
b.indexOf('D'); // -1
var c = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
c.slice(0, 3); // 从索引0开始，到3结束，['A', 'B', 'C']
//不传参数，表示从头到尾复制一个
```

直接给Array的length赋值会导致Array大小的变化；
通过索引可以把对应的元素修改为新的值，对Array的索引进行赋值会直接修改这个Array

`slice()`类似于string的`substring()`

**push和pop**：`push()`向数组的末尾添加若干元素，`pop()`把Array的最后一个元素删除掉，数组会改变

**unshift和shift**：`unshift()`向数组的头部添加若干元素，`shift()`把数组的第一个元素删除掉，数组会改变

**sort()**对数组进行排序，直接修改当前数组的元素位置
**reverse()**把数组整个掉个个

```javascript
var arr = ['B', 'C', 'A'];
arr.sort();
arr; // ['A', 'B', 'C']
arr.reverse();
arr; // ['C', 'B', 'A']
```

`splice()`方法修改数组的万能方法，可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素

```javascript
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
arr.splice(2, 3, 'Google', 'Facebook'); //返回被删除的元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[]
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```

**concat**把当前数组和另一个数组连接起来，返回一个新的数组，`concat()`可以接收任意个元素和数组，然后自动把数组拆开，然后全部添加到数组里，返回一个新的数组

```javascript
var arr = ['A', 'B', 'C'];
var added = arr.concat([1, 2, 3]);
added; // ['A', 'B', 'C', 1, 2, 3]
arr; // ['A', 'B', 'C']
```

**join**把当前数组的每个元素用指定的字符串连接起来，然后返回连接后的字符串，如果是非字符串则自动转换成字符串后再连接

```javascript
var arr = ['A', 'B', 'C', 1, 2, 3];
arr.join('-'); // 'A-B-C-1-2-3'
```

---

#### 对象

JavaScript的对象是一种无序的集合数据类型，由若干键值对组成

JavaScript用一个`{...}`表示一个对象，键值对以`xxx: xxx`形式声明，用`,`逗号隔开，最后一个键值对不需要再末尾加逗号，否则一些低版本的IE将会报错

属性访问是通过`.`操作符完成的，但这要求属性名必须是一个有效的变量名，如果属性名包含特殊字符，就必须使用`''`括起来，且无法使用`.`操作符，必须使用`['xxx']`来访问：

```javascript
var xiaohong = {
    name: '小红'，
    'middle-school': 'No.1 Middle School'
};
xiaohong['middle-school']; // 'No.1 Middle School'
xiaohong['name']; // '小红'
xiaohong.name; // '小红'
xiaohong.age = 18; // 新增一个age属性
delete xiaohong.age; // 删除age属性
delete xiaohong.school; // 删除一个不存在的school属性也不会报错
```

JavaScript对象的所有属性都是字符串，不过属性对应的值可以是任意数据类型，访问一个不存在的属性会返回undefined而不是报错

检测一个对象是否拥有一个属性，可以用in操作符。这个属性也可能是对象通过继承得到的

```javascript
var xiaoming = {
    name: '小明'，
    birth: 1990
}
'name' in xiaoming; // true
'grade' in xiaoming; // false
'toString' in xiaoming; // true 通过继承得到的
xiaoming.hasOwnProperty('name'); // true 判断一个属性是否是自身拥有的，而不是继承得到的，可以使用hasOwnProperty()方法
xiaoming.hasOwnProperty('toString'); // false
```

#### 条件判断

如果语句块只包含一条语句可以省略`{}`

```javascript
var age = 20;
if (age >= 18)
    alert('adult');
else
    alert('teenager');
```

JavaScript把**null、undefined、0、NaN和空字符串''**视为false，其他值一概视为true

#### 循环

```javascript
var arr = ['Apple', 'Google', 'Microsoft'];
var i, x;
for (i=0; i<arr.length; i++) {
    x = arr[i];
    console.log(x);
}
```

`for ... in `循环可以把一个对象的所有属性依次循环出来

```javascript
var o = {
    name: 'Jack';
    age: 20;
    city: 'Beijing'
};
for (var key in o) {
    console.log(key); // 'name', 'age', 'city'
}
//数组也是对象，不过按上面这个for ... in ...只会列出索引'1'，'2'，'3'，...是字符串而不是数字
```

`for ... in array`得到的是string而不是number

---

#### Map和Set

JavaScript的默认对象表示方式`{}`可以视为其他语言中的Map或dict的数据结构，即一组键值对

# JavaScript的对象的键必须是字符串

Number或其他数据类型作为键也是非常合理的，ES6规范引入了新的数据类型Map

```javascript
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95
```

初始化一个Map需要一个二维数组，或者直接初始化一个空的Map

Set和Map类似，需要一个数组作为输入，或者直接创建一个空Set

```javascript
var s1 = new Set();
var s2 = new Set([1, 2, 3]);
```

重复元素再Set中自动被过滤

#### iterable

ES6标准引入了新的iterable类型。Array/Map/Set都属于iterable类型

```javascript
'use strict';
var a = [1, 2, 3];
for (var x of a) {
    console.log(x);
}
```

**`for ... in`和`for ... of`的区别**

for...in循环由于历史遗留问题，遍历的实际上是对象的属性名称。一个Array数组实际上是一个对象，它的每个元素的索引被视为一个属性

手动给数组添加属性后，for...in会把添加的属性也列出来

for...of循环则完全修复了这些问题，只循环集合本身的元素

更好的方式是直接使用iterable内置的**forEach**方法，他接收一个函数，每次迭代就自动回调该函数

```javascript
'use strict'
var a = ['A', 'B', 'C'];
a.forEach(function (element, index, array) {
    console.log(element + ', index = ' + index);
});
```

Set和Array类似，但Set没有索引，因此回调函数的前两个参数都是元素本身

Map的回调参数依次为value/key/map本身

---

#### 函数

定义函数

```javascript
function abs(x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

没有return的语句，函数执行完毕后也会返回结果，只是结果为undefined

JavaScript的函数也是一个对象，上述定义的ads()函数实际上是一个函数对象，函数名abs可以视为指向该函数的变量

```javascript
var abs = function (x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
};
```

这种情况下，function(x) {...}是一个匿名函数，没有函数名，这个匿名函数赋值给了变量abs，通过abs可以调用该函数。

两种定义完全等价

#### 函数调用

按顺序传入参数即可，由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函数内部并不需要这些参数

传入参数比定义时的少也没有问题，少传入的参数会收到undefined

```javascript
//避免收到undefined，可以对参数进行检查
function abs(x) {
    if (typeof x !== 'number') {
        throw 'Not a number';
    }
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

#### arguments

JavaScript还有一个免费赠送的关键字arguments，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。类似Array但不是一个Array

```javascript
'use strict';
function foo(x) {
    console.log('x= ' + x); //10
    for (var i=0; i<arguments.length; i++) {
        console.log('arg ' + i + ' = ' + arguments[i]); // 10, 20, 30
    }
}
foo(10, 20, 30);
```

即使函数不定义任何参数，还是可以拿到参数的值。arguments最常用于判断传入参数的个数

#### rest参数

由于JavaScript函数允许接收任意个参数，于是我们就不得不用arguments来获取所有参数。为获得额外的参数，使用rest参数

```javascript
function foo(a, b, ...rest) {
    console.log('a=' + a);
    console.log('b=' + b);
    console.log(rest);
}
```

rest参数只能写在最后，前面用...标识，如果传入参数连正常定义的参数都没有填满，rest参数就会接收一个空数组，而不是undefined

---

#### 变量作用域与解构赋值

如果内部函数和外部函数的变量名重名时，内部函数的变量将屏蔽外部函数的变量

#### 变量提升

JavaScript的函数定义会先扫描整个函数体的语句，把所有声明的变量提升到函数顶部，但不会提升变量的赋值

在函数内部定义变量时，严格遵守“在函数内部首先声明所有变量”这一规则。最常见的做法是在一个var声明函数内部用到的所有变量

```javascript
function foo() {
    var x = 1, y = x+1, z, i;
    for (i=0; i<100; i++) {
        ...
    }
}
```

#### 全局作用域

不在任何函数内定义的变量就具有全局作用域。实际上，JavaScript默认有一个全局对象window，全局作用域的变量实际上被绑定到window的一个属性

直接访问全局变量的某个**属性**和访问**window.属性**是完全一样的

定义的函数实际上也是一个全局变量，并绑定到window对象

```javascript
'use strict';
function foo() {
    alert('foo');
}
foo();
window.foo();
```

JavaScript实际上只有一个全局作用域。任何变量(函数也视为变量)，如果没有在当前函数作用域中找到，就会继续往上查找，最后如果在全局作用域中也没有找到，则报ReferenceError错误

#### 名字空间

全局变量会绑定到window上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难发现

减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中

```javascript
var MYAPP = {};
MYAPP.name = 'myapp';
MYAPP.version = 1.0;
MYAPP.foo = function () {
    return 'foo';
};
```

很多著名的JavaScript库都是这么干的jQuery，YUI，underscore等

#### 局部作用域

JavaScript的变量作用域是函数内部，在for循环等语句块中是无法定义具有局部作用域的变量的，为了解决块级作用域，ES6引入了新的关键字let，用let代替var可以声明一个块级作用域的变量

```javascript
'use strict';
function foo() {
    var sum = 0;
    for (let i=0; i<100; i++) {
        sum += i;
    }
    // SyntaxError
    i += 1;
}
```

ES6标准引入了新的关键字const来定义常量，const与let都具有块级作用域。可以给常量const修饰的变量赋值，不报错但不会有效果

#### 解构赋值

ES6开始，JavaScript引入了解构赋值，可以同时对一组变量进行赋值

```javascript
'use strict';
var [x, y, z] = ['hello', 'JavaScript', 'ES6'];
```

对数组元素进行解构赋值时，多个变量要用`[...]`括起来，如果数组本身还有嵌套，也可以通过下面的形式进行解构赋值，注意嵌套层次和位置要保持一致

```javascript
let [x, [y, z]] = ['hello', ['JavaScript', 'ES6']];
let [ , , z] = ['hello', ['JavaScript', 'ES6']];
//忽略前两个元素，只对z赋值
```

如果需要从一个对象中取出若干属性，也可以使用解构赋值，便于快速获取对象的指定属性：

```javascript
'use strict';
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678'，
    address: {
    	city: 'Beijing',
    	street: 'No.1 Road'
	}
};
var {name, age=19, passport:id, address: {city, zip}} = person;
```

对一个对象进行解构赋值时，同样可以直接对嵌套的对象属性进行赋值，只要保证对应的层次是一致的

有些时候，如果变量已经被声明了，再次赋值的时候，JavaScript引擎会把`{`开头的语句当作了块处理，于是=不再合法，这时需要使用小括号括起来

```javascript
var x, y;
{x, y} = {name:'xiaoming', x:100, y:200}; //报错
({x, y} = {name:'xiaoming', x:100, y:200}); //正常
```

#### 使用场景

快速获取当前页面的域名和路径

```javascript
var {hostname: domain, pathname: path} = location;
```

如果一个函数接收一个对象作为参数，那么可以使用解构直接把对象的属性绑定到变量中

```javascript
function buildDate({year, month, day, hour=0, minute=0, second=0}) {
    return new Date(year + '-' + month + '-' + day + ' ' + hour + ':' + minute + ':' + second);
}
buildDate({year:2017, month:1, day:1});
```

---

#### 方法

在一个方法内部，this是一个特殊的变量，它始终指向当前对象

```javascript
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};
xiaoming.age(); //25
getAge(); //NaN
```

如果以对象的方法形式调用，函数的this指向被调用的对象。如果单独调用函数，this指向全局对象，即window

ECMA决定，在strict模式下让函数的this指向undefined

修复的方法

```javascript
'use strict';
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var that = this;
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - that.birth;
        }
        return ageAgeFromBirth();
    }
};
xiaoming.age();
```

#### apply

指定函数的this指向哪个对象，可以用函数本身的apply方法，它接收两个参数，第一个参数就是需要绑定的this变量，第二个参数是array，表示函数本身的参数

```javascript
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}
var xiaoming = {
    name: 'xiaoming',
    birth: 1990,
    age: getAge
};
xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25
```

另一个与`apply()`类似的方法是`call()`，唯一的区别是

+ `apply()`把参数打包成Array再传入
+ `call()`把参数按顺序传入

对普通函数调用，通常把this绑定为null

#### 装饰器

利用`apply()`，我们还可以动态改变函数的行为

```javascript
'use strict';
var count = 0;
var oldParseInt = parseInt; //保存原函数

window.parseInt = function () {
    count += 1;
    return oldParseInt.apply(null, arguments); // 调用原函数
};
parseInt('10');
parseInt('20');
parseInt('30');
console.log('count = ' + count); // 3
```

---

#### 高阶函数

Higher-order function，一个函数可以接收另一个函数作为参数，这种函数就称之为高阶函数

```javascript
function add(x, y, f) {
    return f(x) + f(y);
}
```

---

#### map/reduce Array的方法

```javascript
'use strict';
function pow(x) {
    return x*x;
}
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(pow);
console.log(results);
arr.map(String);
```

`Array`的`map()`方法和`reduce()`方法

`reduce()`把一个函数作用在这个Array的`[x1, x2, x3 ... ]`上，这个函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算，其效果就是

`[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)`

```javascript
var arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x + y;
});
```

**JavaScript的parseInt(string, radix)接收两个参数**
**map接收的回调函数可以有3个参数callback(currentValue, index, array)，通常我们仅需要第一个参数，而忽略后面的两个参数**

**因此parseInt会接收到两个参数，实际执行的函数分别为：parseInt('0', 0); paeseInt('1', 1); parseInt('2', 2);**

---

#### filter

把Array的某些元素过滤掉，然后返回剩下的元素

接收一个参数，把传入的函数依次作用于每个元素，然后根据返回值是true还是false决定保留还是丢弃该元素

```javascript
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
var r = arr.filter(function (x) {
    return x%2 !== 0;
})；
r; // [1, 3, 5, 7, 9]
```

#### 回调函数

接收的回调函数，其实可以有多个参数，通常我们仅使用第一个参数，表示Array的某个元素。回调函数还可以接收另外两个参数，表示元素的位置和数组本身

```javascript
'use strict';
var
	r,
    arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'strawberry'];
r = arr.filter(function (element, index, self) {
    return self.indexOf(element) === index;
});
```

---

#### sort排序算法

sort()方法默认把所有元素先转换为String再排序，根据字符串的ASCII码进行排序

sort会改变原来的元素位置，还是原来的array，而filter/map会返回一个新的array，reduce应该是返回一个数值

sort()还可以接收一个比较函数来实现自定义的排序

```javascript
'use strict';
var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
});
```

#### Array

**every**：可以判断数组的所有元素是否满足测试条件

```javascript
'use strict';
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.every(function (s) {
    return s.length > 0;
})); // true，因为每个元素都满足s.length > 0
```

**find**：用于查找符合条件的第一个元素，如果找到了返回这个元素，否则返回undefined

```javascript
'use strict';
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.find(function (s) {
    return s.toLowerCase() === s;
})); // 'pear', 因为pear全部是小写
```

**findIndex**：和find()类似，也是查找符合条件的第一个元素，不同之处是findIndex会返回这个元素的索引，如果没有找到，会返回-1

**forEach**：和map()类似，也把每个元素依次作用于传入的函数，但不会返回新的数组，forEach()常用于变量数组，因此，传入的函数不需要返回值

```javascript
'use strict';
var arr = ['Apple', 'pear', 'orange'];
arr.forEach(console.log);
```

---

#### 闭包Closure

函数作为返回值，注意到返回的函数在其定义内部应用了局部变量arr，所以，当一个函数返回了一个函数后，其内部的局部变量还被新函数引用，所以，闭包用起来简单，实现起来不容易。而且函数并没有立即执行

```javascript
function lazy_sum(arr) {
    var sum = function () {
        return arr.reduce(function (x, y) {
            return x + y;
        });
    }
    return sum;
}
```

即使传入相同的参数，每次调用lazy_sum(arr)都返回一个新函数。当一个函数返回了一个函数后，其内部的局部变量还被新函数引用。

## 返回闭包时牢记的一点就是：返回函数不要引用任何循环变量，或者后续会发生变化的变量

如需要引用循环变量，需要再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，但是已绑定到函数参数的值不变

```javascript
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push((function (n) {
            return function () {
                return n*n;
            };
        })(i));
    }
    return arr;
}
var results = count();
var f1 = results[0]; 
var f2 = results[1];
var f3 = results[2];
f1(); // 1
f2(); // 4
f3(); // 9
```

在面向对象的程序设计语言里，比如Java和C++，要在对象内部封装一个私有变量，可以用private修饰一个成员变量

在没有class机制只有函数的语言里，借助闭包，同样可以封装一个私有变量

使用JavaScript创建一个计数器：

```javascript
'use strict';
function create_counter(initial) {
    var x = initial || 0;
    return {
        inc: function () {
            x += 1;
            return x;
        }
    }
}
```

# 脑洞大开，不太明白

```javascript
'use strict';

//定义数字0：
var zero = function (f) {
    return function (x) {
        return x;
    }
};
//zero(func) ===> function(x) {return x;};
//zero(func)() ===> []
//定义数字1：
var one = function (f) {
    return function (x) {
        return f(x);
    }
};
//one(func) ===> function(x) {return func(x);};
//one(func)() ===> func([])
//定义加法
function add(n, m) {
    return function (f) {
        return function (x) {
            return m(f)(n(f)(x));
        }
    }
}
//add(one, one) ===> function (f) {return function (x) { return one(f)(one(f)(x))}}
//add(one, two)(func) ===> function (x) { return one(func)(two(func)(x))}
//add(one, x)(func)() ===> one(func)(two(func)([]))
```

---

#### 箭头函数Arrow Function

```javascript
x => x * x
```

相当于匿名函数，并且简化了函数定义，箭头函数有两种格式，一种如上，一种如下

```javascript
x => {
    if (x>0) {
        return x * x;
    } else {
        return - x * x;
    }
}

(x, y) => x*x + y*y;
() => 3.14
```

#### this

箭头函数内部的this是词法作用域，由上下文确定，this总是指向词法作用域，也就是外层调用者obj

```javascript
var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth; //1990
        var fn = () => new Date().getFullYear() - this.birth; // this指向obj对象
        return fn();
    }
};
obj.getAge(); //25
```

由于this在箭头函数中已经按照词法作用域绑定了，所以，用call()或者apply()调用箭头函数时，无法对this进行绑定，即传入的第一个参数被忽略

```javascript
var obj = {
    birth: 1990,
    getAge: function (year) {
        var b = this.birth;
        var fn = (y) => y-this.birth;
        return fn.call({birth:2000}, year); //此处的{birth:2000}被忽略，不起作用
    }
};
obj.getAge(2015); //25
```

---

#### generator生成器

看上去像一个函数，但可以返回很多次

```javascript
function* foo(x) {
    yield x+1;
    yield x+2;
    return x+3;
}
```

直接调用一个generator和调用函数不一样，调用generator仅仅是创建了一个generator对象，还没有去执行它

调用一个generator对象有两个方法，一是不断地调用generator对象的next()方法，第二个方法是直接用for ... of 循环迭代generator对象，这种方式不需要我们自己判断done

```javascript
'use strict';
function* fib(max) {
    var
    	t,
        a = 0,
        b = 1,
        n = 0;
    while (n < max) {
        yield a;
        [a, b] = [b, a+b];
        n ++;
    }
    return;
}
var f = fib(3);
f.next();  // {value: 0, done: false}
f.next();  // {value: 1, done: false}
f.next();  // {value: 1, done: false}
f.next();  // {value: undefined, done: true}
for (var x of fib(10)) {
    console.log(x);
}
```

---

#### 标准对象

# 一切皆是对象

typeof操作符获取对象的类型，返回一个字符串

```javascript
typeof 123; // 'number'
typeof NaN; // 'number'
typeof []; // 'object'
```

`number/string/boolean/function/undefined`有别于其他类型，`null/array`的类型是object，无法用typeof区分出`null/array`和通常意义上的object，即`{}`

#### 包装对象

包装对象用new创建，类型为object，包装对象和原始值用===比较会返回false

```javascript
var n = new Number(123); // 123生产了新的包装类型
var b = new Boolean(true);
var s = new String('str');
```

不建议使用`new Number()/new Boolean()/new String()`创建包装对象

用`parseInt()`或`parseFloat()`来转换任意类型到`number`；
用`String()`来转换任意类型到`string`，或者直接调用某个对象的`toString()`方法；
通常不必把任意类型转换为`boolean`再判断，因为可以直接写`if (myVar) {...}`；
`typeof`操作符可以判断出`number/ boolean/ string/ function/ undefined`；
判断`Array`要使用`Array.isArray(arr)`；
判断`null`请使用`myVar === null`；
判断某个全局变量是否存在`typeof window.myVar === 'undefined'`；
函数内部判断某个变量是否存在用`typeof myVar === 'undefined'`；

**`null`和`undefined`**没有`toString()`方法

数字调用toString：

```javascript
123.toString(); // SyntaxError
123..toString(); // '123'
(123).toString(); // '123'
```

---

#### Date

Date对象用来表示日期和时间

```javascript
var now = new Date();
now;  // Wed Jun 16 2019 10:20:21 GMT+0800 (CST)
now.getFullYear();
now.getMonth();
now.getDate();
now.getDay();
...
//获取系统当前时间，当前时间是浏览器从本机操作系统获取的时间，不一定准确，因为用户可能把时间当前时间设定位任何值
//创建一个指定日期和时间的Date对象
//JavaScript的月份从0开始
var d = new Date(2015, 5, 19, 20, 14, 30, 123);
var d = Date.parse('2015-06-24T19:49:22.875+08:00');
d;  // 返回一个时间戳
var d = new Date(14231456756644);
d;
```

使用Date.parse()时传入的字符串使用实际月份01-12，转换后Date对象后getMonth()获取的月份值为0-11

Date对象表示的时间总是按浏览器所在时区显示的，不过既可以显示本地时间，也可以显示调整后的UTC时间

```javascript
var d = new Date(1435146562875);
d.toLocaleString();
d.toUTCString();
```

时间戳是一个自增的整数，表示从1970年1月1日零时整的GMT时区开始的那一刻，到现在的毫秒数

```javascript
'use strict';
if (Date.now) {
    console.log(Date.now());
}
```

---

#### RegExp

正则表达式是一种用来匹配字符串的强有力的武器

+ `\d`可以匹配一个数字；`\w`可以匹配一个字母或者数字：`00\d`可以匹配`007`；`\s`表示匹配一个空格，也包括Tab等空白符
+ `.`可以匹配任意字符
+ `*`表示任意个字符，包括0个
+ `+`表示至少一个字符
+ `?`表示0或1给字符
+ `{n}`表示n个字符
+ `{n,m}`表示n-m个字符

#### 进阶

+ `[0-9a-zA-Z\_]`可以匹配一个数字、字母或者下划线
+ `[0-9a-zA-Z\_]+`可以匹配至少由一个数字、字母或者下划线组成的字符串
+ `[a-zA-Z\_\$][0-9a-zA-Z\_\$]*`可以匹配由字母或下划线、$开头，后接任意一个数字、字母或者下划线、$组成的字符串
+ `[a-zA-Z\_\$][0-9a-zA-Z\_\$]{0, 19}`更精确地限制了变量的长度是1-20个字符

`[]`表示范围；`A|B`可以匹配A或B；`^`表示行的开头；`$`表示行的结束；`^\d`表示必须以数字开头；`\d$`表示必须以数字结尾

#### RegExp

```javascript
var re1 = /ABC\-001/;  // /正则表达式/
var re2 = new RegExp('ABC\\-001');
re1.test('010-12345'); //false
re2.test('ABC-001'); //true
```

#### 切分字符串

```javascript
'a b  c'.split(/\s+/); // ['a', 'b', 'c']
'a,b;; c  d'.split(/[\s\,\;]+/); // ['a', 'b', 'c', 'd']
```

#### 分组

正则表达式还有提取子串的强大功能，用`()`表示的就是要提取的分组

```javascript
var re = /^(\d{3})\-(\d{3,8})$/;
re.exec('010-12345'); // ['010-12345', '010', '12345']
re.exec('010 12345'); // null
// 匹配成功后会返回一个Array，第一个元素是正则表达式匹配到的整个字符串，后面的字符串表示匹配成功的字串，失败时返回null
```

#### 贪婪匹配

正则匹配默认是贪婪匹配，也就是匹配尽可能多的字符

```javascript
var re = /^(\d+)(0*)$/;
re.exec('102300');  // ['102300', '102300', '']
//给\d+加个?就可以让\d+采用非贪婪匹配
var re = /^(\d+?)(0*)$/;
re.exec('102300');  // ['102300', '1023', '00']
//貌似只返回匹配的第一个
```

#### 全局搜索

g表示全局搜索，i表示忽略大小写，m表示执行多行匹配

```javascript
var r1 = /test/g;
var r2 = new RegExp('test', 'g');
```

全局匹配可以多次执行`exec()`方法来搜索一个匹配的字符串。当我们指定g标志后，每次运行`exec()`，正则表达式本身会更新`lastIndex`属性，表示上次匹配到的最后索引

```javascript
var s = 'JavaScript, VBScript, JScript and ECMAScript';
var re = /[a-zA-Z]+Script/g;
re.exec(s); // ['JavaScript']
re.lastIndex; // 10
re.exec(s); // ['VBScript']
re.lastIndex; // 20
re.exec(s); // ['JScript']
re.lastIndex; // 29
re.exec(s); // ['ECMAScript']
re.lastIndex; // 44
re.exec(s); // null,直到结束仍没有匹配到
```

---

#### JSON

JavaScript Object Notation的缩写，是一种数据交换格式

把任何JavaScript对象变成JSON，就是把这个对象序列化成一个JSON格式的字符串，这样才能够通过网络传递给其他计算机

收到一个JSON格式的字符串，只需要把它反序列化成一个JavaScript对象，就可以在JavaScript中直接使用这个对象了

JSON数据类型：
number/boolean/string/null/array/object(即{}表示方式)

字符集必须是UTF-8，字符串必须使用双引号，Object的键也必须使用双引号

#### 序列化

**将对象的状态信息转换为可以存储或传输的形式的过程**？

```javascript
'use strict';
var xiaoming = {
    name: 'xiaoming',
    age: 14,
    gender: true,
    height: 1.65
};
var s = JSON.stringify(xiaoming);
console.log(s);
var s = JSON.stringify(xiaoming, null, '  ');
// 按缩进输出
// 第二个参数用于控制如何筛选对象的键值，如果只想输出指定的属性，可以传入Array
var s = JSON.stringify(xiaoming, ['name', 'skills'], '  '); // 输入没有的属性也可运行，不报错，返回结果没有没有的属性
//还可以传入一个参数，这样对象的每个键值对都会被函数先处理
function convert(key, value) {
    if (typeof value === 'string') {
        return value.toUpperCase();
    }
    return value;
}
var s = JSON.stringify(xiaoming, convert, '  ');
```

如果想要精确控制如何序列化小明，可以给xiaoming定义一个`toJSON()`的方法，直接返回JSON应该序列化的数据

```javascript
var xiaoming = {
    name: '小明'，
    age: 14,
    gender: true,
    height: 1.65,
    grade: null,
    'middle-school': '\"W3C\" Middle School',
    skills: ['JavaScript', 'Java', 'Python', 'Lisp'],
    toJSON: function () {
        return {
            'Name': this.name,
            'Age': this.age
        };
    }
}；
JSON.stringify(xiaoming); // '{"Name":"小明","Age":14}'
```

#### 反序列化

拿到一个JSON格式的字符串，直接用`JSON.parse()`把它变成一个JavaScript对象

```javascript
JSON.parse('[1,2,3,true]'); // [1,2,3,true]
JSON.parse('{"name":"小明","age":14}'); // Object {name:'小明', age:14}
JSON.parse('true'); // true
```

`JSON.parse()`还可以接收一个函数，用来转换解析出的属性

```javascript
'use strict';
var obj = JSON.parse('{"name":"xiaoming","age":14}', function (key, value) {
    if (key === 'name') {
        return value + '同学';
    }
    return value;
});
console.log(JSON.stringify(obj)); // {name:'xiaoming同学'， age:14}
```

