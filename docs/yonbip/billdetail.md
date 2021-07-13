## 页面初始化的时候，设置默认值

```javascript
function (event) {
    viewModel.get('userid').setValue(newPseudoGuid());
    viewModel.get('startdate').setValue(formatDate(new Date()));
    function formatDate(date){
        var month = date.getMonth()+1;
        return date.getFullYear()+"-"+month+"-"+date.getDate()
    }
}
```

## 初始化自定义按钮绑定点击事件

```javascript
function (event) {
    var viewModel = this;
    viewModel.get('button5ad').on('click', function () {
        var rows = viewModel.getGridModel().getSelectedRows();//获取表格选中行
        console.log(rows);
    })
}
```

## 保存前校验+增加默认值

```javascript
function (event) {
  var viewModel = this;
  //设置保存前校验
  viewModel.on("beforeSave", function(args){
      var jieyongriqi = viewModel.get("jieyongriqi").getValue();
      var guihairiqi = viewModel.get("guihairiqi").getValue();
      const isAfterDate = (dateA, dateB) => dateA > dateB;
      if(!isAfterDate(guihairiqi, jieyongriqi)){
          cb.utils.alert("归还日期要大于借用日期")
          return false;
      }
  })
}
```

## 根据页面状态控制显示按钮，mode的改变

```javascript
function (event) {
    var viewModel = this;
    viewModel.on("modeChange",function (data) {
        if(data == "add"){
            viewModel.get("button24rj").setVisible(true);
        }else{
            viewModel.get("button24rj").setVisible(false);
        }
        //如果是自定义按钮跳转过来的，需要通过下面的方法来获取flag
        if( viewModel.getParams.billFlag == "orderChangeBill"){
            viewModel.get("code").setVisible(false);
        }
    });
    //或者直接获取当前页面的mode：
    var currentState = viewModel.getParams().mode;
}
```

## 枚举字段设置默认值，设置枚举个性化list

```javascript
var sexModel=viewModel.get("sex");  //获取枚举
sexModel.clear();//清空枚举
sexModel.setValue(2); //设值



```

## 字段联动，设置值之后其他字段设置不可编辑

```javascript
function (event) {
    var viewModel = this;
    var sexModel=viewModel.get("sex");  //获取ListModel
    sexModel.on('afterValueChange',function(data){
        if(data.value.value=="1"){
            let phoneModel=viewModel.get("phone"); //获取手机控件
            phoneModel.setVisible(true);
            gridModel.setReadOnly(false); //设置gridModel可编辑
        }
    })
}
```

## 图片更改后的事件

```javascript
function (event) {
  var viewModel = this;
  var picture = viewModel.get("tupian");
  picture.on('onChange',function(date){
     filelist = date.fileList;
     console.log(filelist);
   });
}
```

## 参照框确定按钮监听事件

```javascript
var ref = viewModel.get('ref_name');
ref.on('afterReferOkClick',function(data){
   //参照选择的数据
   console.log(data);
})
```

## 卡片组隐藏

```javascript
viewModel.execute('updateViewMeta',{code:'容器编码',visible:false})
```

## 删除事件

~~~javascript
function (event) {
  var viewModel = this;
  viewModel.on('beforeDelete',function(params){
    var data = JSON.parse(params.data.data);
    if('2' ==data.new1){
        return false;
    }
  });
}

~~~





