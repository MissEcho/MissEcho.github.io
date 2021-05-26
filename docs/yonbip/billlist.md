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

事件汇总

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
```

#### 查询前增加默认值

```javascript
viewModel.on('beforeSearch',function(args){
    //需要获取当前人的身份信息，确定默认的查询条件
    var promise = new cb.promise();
    setTimeout(function() {
        cb.rest.invokeFunction("2abae82cef154bd194d7069135011395", {},
                               function(err, res) {
            if(err!=null){
                cb.utils.alert('查询数据异常');
                permissions = [];
                return false;
            }else{
                args.isExtend = true;
                //请求数据的条件，获取统计信息的时候需要用到
                reqCondition= args.params.condition;
                commonVOs = args.params.condition.commonVOs;
                if(undefined==permissions){
                    //设置一个不可能查询出数据的条件
                    cb.utils.alert('查询数据异常--获取人员失败');
                    commonVOs.push({
                        itemName:'id',
                        op:'eq',
                        value1:''
                    });
                }else{
                    var permissions = res.res;
                    var alldata = res.allData;
                    if(undefined==alldata){
                        if(permissions.length>0){
                            var conditions = args.params.condition;
                            conditions.simpleVOs=
                                [{
                                    "logicOp": "and",
                                    "conditions": [{
                                        "logicOp": "or",
                                        "conditions": [
                                            {
                                                "field": "matter_type_m",
                                                "op": "eq",
                                                "value1": '3'
                                            },
                                            {
                                                "field": "StaffNew",
                                                "op": "in",
                                                "value1": permissions
                                            }
                                        ]
                                    }]
                                }];
                        }else{
                            commonVOs.push({
                                itemName:'matter_type_m',
                                op:'eq',
                                value1:'3'
                            });
                        }
                    }
                }
                promise.resolve();
            }
        })
    }, 10);
    return promise;
})
```

#### 挂载在viewmodel上的函数

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
```

#### 挂载在gridModel上的函数

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
```

## 查询区域动态设置默认值并过滤

```javascript
function (event) {
    var viewModel = this;
    var bh = viewModel.get("params").abnormalevent;
    viewModel.on('afterMount', function(){
        // 获取查询区模型
        const filtervm = viewModel.getCache('FilterViewModel'); 
        filtervm.on('afterInit', function () {
            // 进行查询区相关扩展
            filtervm.get('abnormalevent').getFromModel().setValue(bh);
        });
    });
}
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
```

