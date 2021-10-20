## js的技巧 



### 1.复制到粘贴板



### 2.将RGB换成16进制

```javascript
//my code
const cover16 = (r, g, b) => {
  //判断入参正确（1.大于或等于0 小于或等于255的正整数）
  if (testNum(r) && testNum(g) && testNum(b)) {
    let r16 = r.toString(16);
    let g16 = g.toString(16);
    let b16 = b.toString(16);
    let res = '#' + fixTwo(r16) + '' + fixTwo(g16) + '' + fixTwo(b16);
    console.log(res)
    return res;
  }
  console.log('请输入正确的入参')
  return false;
}

function testNum(num) {
  if (Number.isInteger(num) && num <= 255 && num >= 0) {
    return true;
  }
  return false;
}
function fixTwo(num) {
  let res = num;
  if (num.length === 1) {
    res = '0' + num
  }
  return res;
}

// right code
const rgbToHex = (r, g, b) =>   "#" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1);
rgbToHex(0, 51, 255); // Result: #0033ff


```

PS：遇到了一个toString的问题，即数字后面不能直接接.toString。因为js把这个.当做小数位处理了。如果实在要通过数字来.toString，则可以通过 12..toString(16)的方式。

PS：toString(16)这个api还是比较少见的。可以记下。但是toString的入参只接受2~36的整数。其他的会报错。

点评：ennn，这个技巧还是很好的。。。特别是后面的  slice(1)，这个惊呆了。

还可以使用padEnd  or padStart属性进行拼接0。



### 3.打乱数组

~~~javascript
[1,2,3,4,5,6].sort(()=>{return 0.5-Math.random()});
//涉及到两个知识，sort和random，可以用于打乱的算法。
~~~

