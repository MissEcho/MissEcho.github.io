## 自定义按钮跳转到编辑页面

使用场景：一张列表单据对应多个详情界面。比如：单据变更（和详情界面相差太多，使用新的编辑页面会更清爽一点）

实现过程：

在列表页面上新增一个自定义按钮，配置点击事件即可

```javascript
function (event) {
    var viewModel = this;
    let data = {
        billtype: 'Voucher', //单据类型
        billno: '9029244a', //单据号
        params: { 
            "billFlag": "orderChangeBill", //给编辑页面一个新的标识
            "mode": 'edit',
            "id": selectRows[0].id,
            "rowData":selectRows[0],
            "title": "订单变更",
        }
    };
    //重点是这行  
    cb.loader.runCommandLine('bill', data, viewModel);
} 
```

变更界面，可以实现其他逻辑

```javascript
function (event) {
    var viewModel = this;
    //当时点击变更过来的 则隐藏客户编码字段，其他状态的单据页面依然显示客户编码字段
    viewModel.on('modeChange', (mode) => {
        if(mode.toLocaleLowerCase() == "edit" && viewModel.getParams.billFlag == "orderChangeBill"){
            viewModel.get("code").setVisible(false);
        }
    })
} 
```

## 表格事件监听

```
//挂载在卡片界面 viewmodel
beforeBatchdelete //批量删除
beforeDelete  //单行删除
beforeDeleteRow  //删除行
beforeSearch       //列表查询前事件，列表界面查询过滤 过滤条件通过调用api函数获取到
afterDeleteRow //删除行之后
afterLoadData       //数据加载完成后事件



//挂载在gridModel上
beforeSetDataSource   //设置数据源前的事件
afterSetDataSource    //设置数据源后的事件
beforedbclick //双击事件
```

## 挂载在viewmodel上的函数

```javascript
function (event) {
    var viewModel = this;
    viewModel.on('afterLoadData', function () {
        var id = viewModel.get("id").getValue();
    });
    //批量删除
    viewModel.on('beforeBatchdelete',function(params){
        var check = true;
        var selected = JSON.parse(params.data.data);
        selected.forEach((row)=>{
            //row循环的行数据
            if('2' ==row.new1){
                check = false;
                return;
            }
        });
        return check;
    });
    //单行删除
    viewModel.on('beforeDelete',function(params){
        var data = JSON.parse(params.data.data);
        if('2' ==data.new1){
            return false;
        }
    });
    //删除行之前
    viewModel.on("beforeDeleteRow",function (args) {
        var returnPromise = new cb.promise();
        cb.utils.confirm('确定要停用吗？', function(){
            //获取选中行
            var data = viewModel.getGridModel().getRows()[args.data[0]];
            return returnPromise.resolve();
        },function (args) {
            cb.utils.alert("点击了取消");
            returnPromise.reject();
        });
        return returnPromise;
    });
    //删除行之后
    viewModel.on('afterDeleteRow', function (args) {
        var rows = viewModel.getGridModel().getRows();
        if(rows.length>0){
            var totaldata = 0;
            rows.forEach(data => {
                if(data.jine==""||data.jine==undefined)data.jine=0;
                totaldata+=data.jine;
            });
            viewModel.get("zongjine").setValue(totaldata);
        }
    });
}
viewModel.on('afterLoadData', function () {
    var id = viewModel.get("id").getValue();
});
```

## 挂载在gridModel上的函数

```javascript
//绑定在gridModel上的事件
var gridModel = viewModel.getGridModel();
//表格选中后事件
gridModel.on('afterSelect', function (data) {
    cb.utils.alert(data);
});  
//编辑单元格
gridModel.on("afterCellValueChange",function(data){
    var cellName = data.cellName;
    if(cellName=='jine'){
        var rows = viewModel.getGridModel().getRows();
        if(rows.length>0){
            var totaldata = 0;
            rows.forEach(data => {
                if(data.jine==""||data.jine==undefined)data.jine=0;
                totaldata+=data.jine;
            });
            viewModel.get("zongjine").setValue(totaldata);
        }
    }
});
//可编辑表格里面的参照过滤
gridModel.getEditRowModel().get('ref1022_id').on('beforeBrowse', function (data) {
    var condition = {
        "isExtend": true,
        simpleVOs: []
    };
    condition.simpleVOs.push({
        field: 'new1',
        op: 'eq',
        value1: 11
    });
    this.setFilter(condition);
});
//设置表格整行禁用
gridModel.setRowState(行号,'disabled',true);
//设置值
gridModel.on('beforeSetDataSource', (data) => {
    var change_data = JSON.parse(JSON.stringify(data));
});
//设置值之后
gridModel.on('afterSetDataSource', (data) => {
    var change_data = JSON.parse(JSON.stringify(data));
});
gridModel.on('afterSelect', function (data) {
        cb.utils.alert(data);
    });

```

## 单元格设置点击事件

```javascript
//需要先在该字段上右键编辑器，设置  为true
var gridModel = viewModel.getGridModel();
gridModel.on('cellJointQuery', ({cellName, row, rowIndex}) => {
  debugger
  if(cellName === ''){
    //当前点击的列是需要操作的列
    return false
  }else{
    // 不是需要处理的列则什么都不做
  }
})

```


## 表格指定某些行可编辑

```javascript
//设置表格为只读
gridModel.setReadOnly(false)
//倒序增行
gridModel.insertRow(0,null,true)


//表格合计
viewModel.getGridModel().doPropertyChange('changeGridProps', {
    showAggregates: 'local', 
    showCheckBox: false ,
    isPagination: false
});

//禁用行操作
viewModel.getGridModel().setRowState(0, 'disabled', true)
```

## 表格内部自定义按钮的显示隐藏

```javascript
let gridModel = viewModel.getGridModel();
gridModel.on('afterSetDataSource', function(arg) {
    setTimeout(function() {
        let verifystate = viewModel.get('verifystate').getValue();
        let actionsState = gridModel.getActionsState();
        actionsState.forEach(item => {
            item['button19kb'].visible = verifystate === 2;
        });
        gridModel.setActionsState(actionsState)
    }, 100);
})

//表格行按钮显示隐藏

function (event) {
  var viewModel = this;
  //获取当前的model
  let gridModel = viewModel.getGridModel();
  //afterSetDataSource界面加载完成后，对数据进行修改
  gridModel.on('afterStateRuleRunGridActionStates', () => {
  //获取列表所有数据
  const rows = gridModel.getRows();
  //从缓存区获取按钮
  const actions = gridModel.getCache('actions');
  if (!actions) return;
  const actionsStates = [];
  rows.forEach(data => {
    const actionState = {};
    actions.forEach(action => {
    //设置按钮可用不可用
    actionState[action.cItemName] = { visible: true };
    if(action.cItemName == 'btnDelete'){
        if(data.enable==1){
            actionState[action.cItemName] = { visible: false };
        }
    }
    });
    actionsStates.push(actionState);
  });
  gridModel.setActionsState(actionsStates);
  });
}

```

## 表格设置单元格的值

~~~javascript
setCellValue();

setData();
~~~

## 表格设置某个单元格样式

```javascript

let gridModel = viewModel.getGridModel();
gridModel.on('afterSetDataSource', (data) => {
    if(undefined==data||data.length==0)return;
    //获取到表格当前页的数据
    var change_data = JSON.parse(JSON.stringify(data));
    //开启同步块
    var promiseCh = new cb.promise();
    //请求调用后端API函数---【根据页面数据获取当前人的阅读信息】
    cb.rest.invokeFunction("32de814b6db5410fa9d4a197dc7c05bc", {data:change_data},
        function(err, res) {
          if(err!=null){
            cb.utils.alert('获取统计数据异常');
            return false;
          }else{
            var lookResMy = res.res;
            for(j = 0; j < change_data.length; j++) {
              if(lookResMy.indexOf(change_data[j].id)>-1){
                gridModel.setCellValue(j, "look_situation_m", "2");
              }else{
              gridModel.setCellValue(j, "look_situation_m", "1");
              }
            } 
            //设置未查看颜色
            var selected = document.querySelectorAll("div[title='未查看']");
            if(null!=selected){
              selected.forEach((data)=>{
                data.style = data.style.cssText + '; color:red';
              })
            }
          return promiseCh.resolve();
        }
    })

  });
```

