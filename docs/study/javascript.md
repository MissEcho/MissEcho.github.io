## 数据类型

**string** 

**number**

**boolean**

**object**

**undefined**

**null**

**Symbol**

**BigData**

typeof只会返回以下六种类型：

undefined    值未定义

boolean      布尔值

string        字符串

number      数值（整形和浮点）

object        对象或null

function      函数

---



PS：python的6种标准类型：string；number；Tuple；List；Dictionary；Set

大部分的高级语言种，只有javascript是动态执行语言，其他都是静态语言类型，也就是说先编译后执行。而js虽然也是通过v8来进行编译，但是是边执行，边编译。



## Array数组

### 一、基础api

1.push，会改变原数组，但是返回的是新增加的项；

2.splice，会改变数组，参数是截取替换的，第一个是index，第二个是要替换的项目，



不会改变原数组的：

1.concat：



### 二、es6

1.Array.from();转为数组新方式；





## set集合

早上因为python的一道算法题，学会了es6的Set用法，摘录如下：
1.Set(arr)返回的是一个对象，其实通过Set去重，应该是利用了对象属性唯一的特性；所以Set()也是返回的一个对象

2.值得注意的是：

```javascript
let arr=new Set([{},{}])arr={{},{}} 
let arr2=new Set([NaN,NaN])arr2={NaN}
//由此可见，set中，空对象不是相同值；而NaN却是相同的；
//而且set当中，2，’2’会看作不同的值，所以在去重的时候，最好先遍历一遍
```

3.set的参数不一定是数组，所有可遍历的对象都是；
4.Set()的返回值是一个对象；所以要转为数组，有两种方式；

//解构
[…new Set(arr)]
//数组的from方法Array.from(new Set(arr))

5.set实例的继承Set.prototype的属性：//size，获取set实例的成员总数let set=new Set();let size=set.size;//hasset.has(2)  //false
6.set实例的方法//set的add()方法let set=new Set();//可以不传参数set.add(‘1’);set.add(1);set.size; //2
//set的delete()方法set.delete(2);//返回值为布尔类型；如果值不存在或者其他原因为false；如果删除成功则true；
//set的clear()方法

7.set的遍历器//其实set的返回的值就是一个对象；所以set拥有对象所有的遍历属性；eg：forEach；keys；values；entries；//set里面的键值对可能是同一个值；

8.Set的高级用法：数组之间的交集；并集；差集等；以及最重要的数组去重；
9.不是很重要的Weakset；听名字就不重要，阉割大部分属性；所以不提也罢



## 函数

对于重复循环的函数总是一直找不到很好的办法解决；

加上对回调函数的恐惧，promise也是一直不得其解；这次干脆一次性解决好了；；；；
递归函数其实也用到了回调函数，这两个并不是不可区分的；

递归的经典题目有 https://blog.csdn.net/PrisonersDilemma/article/details/89454382练习题目在/wujurong/program/code/python/jsTest/digui.js
递归函数的意义在于临界值，回调函数只是一种函数而已，这个取决于函数是作为参数的引入而产生的。。。
而嵌套循环则是while的使用多练习即可；

1.第一年的工资是10000，每年增幅5%，那么50年后薪资多少钱？？50年后总共领了多少钱？？



## 正则

#### 使用正则截取字符串中两字符中的内容

```
var text = '活动名称：{{keyword1.DATA}}'    var regex=/\{\{(.+?)\./g;    var result;    while((result=regex.exec(text))!=null) {      console.log(result[1]);      console.log("!!!!!!!!!!!!!!!!!!!!!!!!!")    }


    var text = '亲爱的#customer#，您已成功购买了#brand#的商品，订单编号为#order#，请凭订单编号或⼿机号⾄商家处消费！#url# 请保留此短信，及时查看消费情况哦！'
    var regex=/\#(.+?)\#/g;
    var result;
    while((result=regex.exec(text))!=null) {
      console.log(result[1]);
      console.log("!!!!!!!!!!!!!!!!!!!!!!!!!")
    }

输出：
customer
brand
order
url
```

#### 获取url上的参数

```


function GetQueryString(name){
    let reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
    let r =window.location.search.substr(1).match(reg); //获取url中"?"符后的字符串并正则匹配
    let context = "";
    if (r != null)
        context = r[2];
    return context == null || context == "" || context == "undefined" ? "" : context;
}

var updateId=GetQueryString('updateObj');




获取仅有一位参数的


var _id=window.location.search.substring(1).split("=")[1];



```



## 原型

new运算的具体执行过程：  

1)创建一个空对象  

2)把这个空对象的__proto__指向构造函数的prototype  

3)把这个空对象赋值给this





## 基本面试题

如何理解this关键字

由于this关键字混乱，如何解决这个问题？

什么是闭包?解释下变量提升？

js如何处理异步或者同步的情况？

如何理解事件委托？

如何理解高阶函数如何区分声明函数和表达式函数?

解释下原型继承是如何工作的？

解释下严格模式？