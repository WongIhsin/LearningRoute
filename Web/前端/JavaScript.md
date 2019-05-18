# 原型链(需要再看)

在ES6之后可以用class代替

定义一个类

```javascript
function Student(props) {
    this.name = props.name || 'Unnamed';
}
Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
}
```

原型链是
new Student() ---> Student.prototype ---> Object.prototype ---> null

new Child() ---> Child.prototype ---> Object.prototype ---> null

要实现原型继承
new Child() ---> Child.prototype ---> Student.prototype ---> Object.prototype ---> null

```javascript
function Child(props) {
    Student.call(this, props); // 调用Student的构造函数，绑定this变量
    this.grade = props.grade || 1;
}
function F () {
}
F.prototype = Student.prototype;
Child.prototype = new F();
Child.prototype.constructor = Child;
```



---

# 面向对象编程

#### 创建对象

JavaScript对每个创建的对象都会设置一个原型，指向它的原型对象

当我们用`obj.xxx`访问一个对象的属性时，JavaScript引擎先在当前对象上查找该属性，如果没有找到，就在其原型对象上找，如果还没有找到，就一致上溯到`Object.prototype`对象，最后，如果还没有找到，就只能返回`undefined`

```javascript
xiaoming.constructor === Student.prototype.constructor; // true
Student.prototype.constructor === Student; // true
Object.getPrototypeOf(xiaoming) === Student.prototype; // true
xiaoming instanceof Student; // true
```

---

#### 原型继承

```javascript
function inherits(Child, Parent) {
    var F = function () {};
    F.prototype = Parent.prototype;
    Child.prototype = new F();
    Child.prototype.constructor = Child;
}
function Student(props) {
    this.name = props.name || 'Unnamed';
}
Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
}
function PrimaryStudent(props) {
    Student.call(this, props);
    this.grade = props.grade || 1;
}
inherits(PrimaryStudent, Student);
PrimaryStudent.prototype.getGrade = function () {
    return this.grade;
};
```

JavaScript的原型继承实现方式就是

1. 定义新的构造函数，并在内部用`call()`调用希望继承的构造函数，并绑定this
2. 借助中间函数F实现原型链继承，最好通过封装的inherits函数完成
3. 继续在新的构造函数的原型上定义新方法

---

#### class继承

新的关键字class从ES6引入到JavaScript，class目的是让定义类更简单

```javascript
function Student(name) {
    this.name = name;
}
Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
}
```

使用新的class关键字来编写Student，可以这样写

```javascript
class Student {
    constructor(name) {
        this.name = name;
    }
    hello() {
        alert('Hello, ' + this.name + '!');
    }
}
```

**class继承**

```javascript
class PrimaryStudent extends Student {
    constructor(name, grade) {
        super(name);
        this.grade = grade;
    }
    myGrade() {
        alert('I am at grade ' + this.grade);
    }
}
```

#### ES6及以上才支持

# 浏览器

---

# 错误处理

两种错误类型

+ 程序写的逻辑不对，导致的代码执行异常，需要修复程序
+ 执行过程中，程序可能遇到无法预测的异常情况而报错，例如网络连接中断，读取不存在的文件，没有操作权限等，这种错误需要处理并可能需要给用户反馈

```javascript
'use strict';
var r1, r2, s = null;
try {
    r1 = s.length;
    r2 = 100;
} catch (e) {
    if (e instanceof TypeError) {
        alert('Type error!');
    } else if (e instanceof Error) {
        alert(e.message);
    } else {
        alert('Error: ' + e);
    }
    console.log('出错了: ' + e);
} finally {
    console.log('finally');
}
console.log('r1 = ' + r1); //r1应为undefined
console.log('r2 = ' + r2); //r2应为undefined
```

#### 错误类型

JavaScript有一个标准的`Error`对象表示错误，还有从`Error`派生的TypeError、ReferenceError等错误对象

#### 抛出错误

throw

```javascript
'use strict';
var r, n, s;
try {
    s = prompt('请输入一个数字');
    n = parseInt(s);
    if (isNaN(n)) {
        throw new Error('输入错误');
    }
    r = n * n;
    console.log(n + ' * ' + n + '=' + r);
} catch (e) {
    console.log('出错了: ' + e);
}
```

如果一个函数内部发生了错误，它自身没有捕获，错误就会被抛到外层调用函数，如果外层函数也没有捕获，该错误会一直沿着函数调用链向上抛出，直到被JavaScript引擎捕获，代码终止执行

#### 异步错误处理

JavaScript引擎是一个事件驱动的执行引擎，代码总是以单线程执行，而回调函数的执行需要等到下一个满足条件的事件出现后，才会被执行

异步代码，无法在调用时捕获，原因是在捕获的当时，回调函数并未执行

# Underscore

JavaScript是函数式编程语言，支持高阶函数和闭包，函数式编程非常强大，可以写出非常简洁的代码。

`map()`、`filter()`等方法Object没有这些方法，低版本的浏览器IE6-8也没有这些方法。**找一个成熟可靠的第三方开源库，使用统一的函数来实现`map()`、`filter()`这些操作**

第三方库underscore，提供了一套完善的函数式编程的接口，更方便地在JavaScript中实现函数式编程

jQuery统一了不同浏览器之间的DOM操作的差异；jQuery在加载时，会把自身绑定到唯一的全局变量`$`上，underscore会把自身绑定到唯一的全局变量`_`上

```javascript
var upper = _.map(obj, function (value, key) {
    return value.toUpperCase();
});
var upper = _.mapObject(obj, function (value, key) {
    return value.toUpperCase();
});
```

**`every/some`**

当集合的所有元素满足条件时，`_.every()`返回true，当集合的至少一个元素满足条件时，`_.some()`返回true

```javascript
'use strict';
_.every([1,4,7,-3,-9], (x) => x > 0); //false
_.some([1,4,7,-3,-9], (x) => x > 0); //true
```

**`max/min`**

这两个函数直接返回集合中的最大和最小的数

```javascript
'use strict';
var arr = [3,5,7,9];
_.max(arr); // 9
_.min(arr); // 3
_.max([]); // -Infinity
_.min([]); // Infinity
```

对于集合是Object的时候，max()和min()只作用于value，忽略key

**`groupBy`**

把集合的元素按照key归类，key由传入的函数返回

```javascript
'use strict';
var scores = [20,81,75,40,91,59,77,66,72,88,99];
var groups = _.groupBy(scores, function (x) {
    if (x < 60) {
        return 'C';
    } else if (x < 80) {
        return 'B';
    } else {
        return 'A';
    }
});
/*
{
	A: [81, 91, 88, 99],
	B: [75, 77, 66, 72],
	C: [20, 40, 59]
}
*/
```

**`shuffle/sample`**

`shuffle()`用洗牌算法随机打乱一个集合，`sample()`则是随机选择一个或多个元素

```javascript
'use strict';
_.shuffle([1,2,3,4,5,6]); // [3,5,4,6,2,1]
_.sample([1,2,3,4,5,6]); // 2
_.sample([1,2,3,4,5,6], 3); // [6,1,4]
```

---

#### Arrays，为Arrays提供了很多工具类方法

**first/last**：分别取第一个和最后一个元素
**flatten**：接收一个Array，无论这个Array里面嵌套了多少个Array，`flatten()`最后都把它们变成一个一维数组
**zip/unzip**：`zip()`把两个或多个数组的所有元素按索引对齐，然后按索引合并成新数组，`unzip()`则相反
**object**：把两个Array拼成一个Object
**range**：快速生成一个序列，不再需要用for循环实现了

```javascript
'use strict';
var name = ['Adam', 'Lisa', 'Bart'];
var scores = [85, 92, 59];
var nameAndScores = _.zip(name, scores); // [['Adam', 85], ['Lisa', 92], ['Bart', 59]]
_.unzip(nameAndScores); // [['Adam', 'Lisa', 'Bart'], [85, 92, 59]]
_.object(name, scores); // {Adam:85, Lisa:92, Bart:59}
_.range(10); // [0,1,2,3,4,5,6,7,8,9]
_.uniq(arr, (x) => x.toLowerCase()); //大小写去重
```

---

#### Functions

underscore本来就是为了充分发挥JavaScript的函数式编程特性，也提供了大量的JavaScript本身没有的高阶函数

**bind**：帮助把一个对象直接绑定在`fn()`的`this`指针上

```javascript
'use strict';
var s = '  Hello ';
var fn = _.bind(s.trim, s);//当用一个变量指向一个对象的方法时，直接调用fn()是不行的，因为丢失了this对象的引用，用bind可以修复这个问题
fn();
```

**partial**：为一个函数创建偏函数。`Math.pow(x, y)`和`Math.pow(2, y)`的关系。根据某个函数创建出来的偏函数，它固定住了原函数的第一个参数

```javascript
'use strict';
var pow2N = _.partial(Math.pow, 2);
pow2N(3); //8
var cube = _.partial(Math.pow, _, 3);
cube(5); //125
```

**memoize**：如果一个函数调用开销很大，我们就可能希望把结果缓存下来，以便后续调用时直接获得结果。用`memoize()`就可以自动缓存函数计算结果

```javascript
'use strict';
var factorial = _.memoize(function (n) {
    console.log('start calculate ' + n + '!...');
    var s = 1, i = n;
    while (i > 1) {
        s = s * i;
        i --;
    }
    console.log(n + '! = ' + s);
    return s;
});
factorial(10);//控制台有输出
factorial(10);//控制台无输出
//两次调用factorial(10), 第二次调用并没有计算，而是直接返回上次计算后缓存的结果，当计算factorial(9)的时候，仍然会重新计算
```

对`factorial()`进行改进，递归调用

```javascript
'use strict';
var factorial = _.memorize(function (n) {
    console.log('start calculate ' + n + '!...');
    if (n < 2) {
        return 1;
    }
    return n * factorial(n - 1);
});
factorial(10);//控制台有输出
factorial(9);//控制台无输出
```

**once**：保证某个函数执行且仅执行一次。如果有一个方法叫`register()`，用户在页面上点两个按钮的任何一个都可以执行的话，就可以用`once()`保证函数仅调用一次，无论用户点击多少次

```javascript
'use strict';
var register = _.once(function () {
    alert('Register OK!');
});
```

**delay**：可以让一个函数延迟执行，效果和`setTimeout()`是一样的，但是代码明显简单了

```javascript
'use strict';
_.delay(alert, 2000);
var log = _.bind(console.log, console);
_.delay(log, 2000, 'Hello,', 'world!');
```

---

#### Objects, 也有大量针对Object的函数

**Keys/allKeys**：可以方便地返回一个object自身所有key，但不包含从原型链继承下来的/包含从原型链继承下来的key

**values**：返回object自身但不包含原型链继承的所有值，么有`allValues()`

**mapObject**：针对object的map版本

**invert**：把object的每个key-value来个交换，key变value，value变key

**extend/extendOwn**：把多个object的key-value合并到第一个object并返回，如果有相同的key，后面的object的value会覆盖前面的object的value。`extendOwn()`和`extend()`类似，但忽略从原型链继承下来的属性

```javascript
'use strict';
var a = {name:'Bob', age:20};
_.extend(a, {age: 15}, {age:88, city:'Beijing'});
//{name:'Bob', age:88, city:'Beijing'}
//变量a的内容也改变了
a;//{name:'Bob', age:88, city:'Beijing'}
```

**clone**：复制一个object对象，会把原有对象的所有属性都复制到新的对象中。**浅复制，两个对象相同的key所引用的value其实是同一对象**

**isEqual**：对两个object，也可以是Array，进行深度比较，如果内容完全相同，则返回true

---

#### Chaining

underscore提供了把对象包装成能进行链式调用的方法，就是`chain()`函数

```javascript
var r = _.chain([1, 4, 9, 16, 25]).map(Math.sqrt).filter(x => x%2 === 1).value();
console.log(r); // [1, 3, 5]
```

