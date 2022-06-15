## 一、开发流程







## 二、组件开发





### sdk的对象

```javascript
 // 获取设备信息
/** {
        language: "zh_CN"
        pixelRatio: 2
        platform: "iOS"
        screenWidth: 414
        windowWidth: 414
    }**/
getSdk().getSystemInfo().then(res => {
    console.log(res);
    
});

_最大的问题，无法获取当前用户的地理位置；


```

