## 变量和类型

### 1.js几种基本类型

现在有6种常用类型，1种大数据类型，1种es6新增类型。

- null （typeof null ==Object。这个是历史遗留问题，因为typeof是根据二进制的前三位来判断的，而null和object的前三位都是000，所以误判了。但是后面没有更改回来。）
- undefined （这个表示是无效值。如果对象中属性值是undefined，那在JSON.stringify过程中会被丢弃掉。）
- boolean（会产生隐式转换）
- string（字面量和new装箱拆箱是不一样的）
- number（一样会产生隐式转换）
- object（）
- BigInt（使用场景很少）
- Symbol（基本上很少用到。除非做一个特殊无实际内容的区分。）



### 2.对象的底层数据结构是什么





### 3.Symbol在实际应用开发中的作用，手动实现一个Symbol

其实我个人感觉symbol在实际中没有太大作用。可能是因为我现在的项目级别不够大，所以应用不多的原因。但这并不妨碍我去理解作者新增symbol类型的原因。







### 4.js变量在内存中存储的形式

基本类型比如null，undefined，string，number，boolean都是以值的方式存在内存中的

而Object类型包括array等引用类型，是以内存地址的形式存在变量当中的。所以复制引用对象，并不是复制了值，只是复制了引用地址而已。



### 5.基本类型对应的内置对象，以及他们的装箱拆箱操作

- null—object
- undefined—undefined
- boolean—Boolean
- number—Number
- string—String
- object—Object
- Symbol—Symbol
- bigInt



### 6.理解值类型和引用类型



### 7.null和undefined的区别

null是表示该变量存在，但是值为空。

而undefined则表示该变量为无效值。如果对象的属性值是undefined的话，那在转json.stringify的时候就会被丢弃。但是在for...in或者keys等api里面还是可以循环出来的。

''表示是一个空的字符串。



### 8.至少说出三种判断数据类型的方法，以及他们的优缺点。以及准确的判断数组类型。






### 9.可能发生隐式转换的

