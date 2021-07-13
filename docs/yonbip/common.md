## 默认规则

默认全局对象为 viewModel（驼峰）

在扩展工程mdf-comp-app中，使用脚本自动生成页面扩展脚本。使用过程如下：



## 参照框确定按钮监听事件

~~~javascript
var ref = viewModel.get('ref_name');
ref.on('afterReferOkClick',function(data){
   //参照选择的数据
   console.log(data);
})
~~~



## 前端请求第三方接口

```javascript
function  get (data,callback) {
    window.URL = window.URL || window.webkitURL;
    var xhr = new XMLHttpRequest();
    xhr.open("get", data, true);
    xhr.responseType = "blob";
    xhr.onload = function () {
      if (this.status == 200) {
        var blob = this.response;
        callback();
      }
    };
    xhr.send();
  };
```

## 设置显示隐藏，更改属性，必输，

```javascript
viewModel.get('button').setVisible(true); //按钮
viewModel.get('field').setVisible(true);//字段
viewModel.get('field').setState('bCanModify'，false);//字段不可编辑
viewModel.get('field').setState('bIsNull',true)
```



## 控制枚举值的显示

```javascript
viewModel.setState('rowStates',{'1':{visible:true}})
//实际案例见 签章模块 印章选择
```



## 请求后台接口统一使用

```javascript
cb.mrjt.ajax(url,{
    method:'post',
    viewModel:viewModel,
    params:{},
    callback:(err,res)=>{
        
    }
})
```



## 页面刷新

```javascript
viewModel.execute('refresh');
```



## 按钮事件

```javascript
viewModel.get('button').on('beforeClick',(args)=>{
    
})
viewModel.get('button').on('beforeClick',(args)=>{
    
})
```

## 打开简易弹窗

```javascript
let html = `<div style="text-align:center;padding-bottom:40px">
<img style="max-width:460px" src="${sealImgUrl}"/>
</div>`;
viewmodel.communication({
    type: 'modal',
    payload: {
        mode: 'html',
        html: html,
    },
});

//关闭模态框
viewModel.communication({type:'modal',payload:{data:false}});
```

## 模仿新增按钮进行新增(多个新增页面,保存变更的时候也有该需求)

```javascript
const execCmdAdd = (viewmodel, card) => {
  let args = {
    cCommand: 'cmdAdd',
    cAction: 'add',
    cSvcUrl: '/bill/add',
    cHttpMethod: 'GET',
    cItemName: 'btnAdd',
    cExtProps: '{"cSubId":"GT857AT42","type":"primary"}',
    cSubId: 'GT857AT42',
    domainKey: 'mrjtsignservice',
  };
  args.cShowCaption = viewmodel._get_data(card.text);
  args.cCaption = viewmodel._get_data(card.text);

  args.disabledCallback = function () {
    // self.setDisabled(true);
  };
  args.enabledCallback = function () {
    // self.setDisabled(false);
  };

  viewmodel.getParams().cardKey = card.key;
  viewmodel.biz.do('add', viewmodel, args);
};
```



## 引用第三方js or css

```javascript
function (event) {
    var viewModel = this;
    extscripturls.push('http://resources.yonyoucloud.com/packages/TestExternal.js');

    //加载css文件
    function loadCssFile(params){
        var fileref=document.createElement("link")
        fileref.rel="stylesheet";
        fileref.type="text/css";
        fileref.href=params;
        fileref.media="screen";
        var headobj=document.getElementsByTagName('head')[0];
        headobj.appendChild(fileref);
    }
    //加载自定义样式
    function loadStyle(params){
        var headobj=document.getElementsByTagName('head')[0];
        var style = document.createElement('style');
        style.type = 'text/css';
        headobj.appendChild(style);
        style.sheet.insertRule(params, 0);
    }
    //加载js
    function loadjs(params){
        var headobj=document.getElementsByTagName('head')[0];
        var script = document.createElement("script");
        script.type = "text/javascript";
        script.src = params;
        headobj.appendChild(script);
    }
    //加载css样式
    loadCssFile("https://a.amap.com/jsapi_demos/static/demo-center/css/demo-center.css");
    //加载js
    loadjs("https://webapi.amap.com/maps?v=1.4.15&key=9b2f6b9f4b37bfe98e4e48cb2a70c376&plugin=AMap.Autocomplete");
    //加载样式
    loadStyle(".mycircle {border-radius: 50%;width: 60px;height: 60px;background: #5A5AAD; text-align:center}");
}
```

## 打开新页签

~~~javascript
cb.mrjt.jDiwork.openService(
    '1a595ad9-518d-4d8e-9ef9-0483567a8528',
    {},
    {
        data: { selectRows },
        type: 1,
        title: '生成收款单',
        code: '',
        url: '',
    }
);
~~~

 ## 在当前页面打开新页面

~~~javascript
let changeData = {
    billtype: 'Voucher',
    billno: 'c23cc890',
    params: {
        billFlag: 'reCreatFlag',
        id: res.id,
        mode: 'edit',
        carryData: res,
        readOnly: true,
        showSearch: false,
        title: '订单详情',
    },
};
cb.loader.runCommandLine('bill', changeData, viewModel);
~~~

## 使用弹窗生成form表单

先在页面上加入一个form表单；

右键打开编辑器，更改 cCtype 为 modal

保存后即可。

```javascript
//打开模态框表单
viewmodel.communication({
    type: 'modal',
    payload: {
        mode: 'inner',
        groupCode: 'form312tg', // 设计器表单容器编号
        viewmodel: viewmodel,
    },
});
```



