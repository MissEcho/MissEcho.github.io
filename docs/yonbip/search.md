## 查询前增加默认值

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
//禁用查询字段
viewModel.getCache('FilterViewModel').get('code').get('fromModel').setDisabled(true)
```



## 设置参照的查询条件

~~~javascript
viewModel.get('pk_supplier_name').setFilter(condition);
~~~



## 设置查询界面的默认值

~~~javascript

viewModel.on('afterMount', function () {
        const filtervm = viewModel.getCache('FilterViewModel');
        if(accountId){
          filtervm.on('afterInit', function () {
            viewModel
              .getCache('FilterViewModel')
              .get('pk_account')
              .get('fromModel')
              .setData(accountId);
          });
        }
      });

//还可以设置对应属性
filtervm.on('afterInit', function () {
          viewModel
            .getCache('FilterViewModel')
            .get(field)
            .get('fromModel')
            .setDisabled(true);
        });
~~~


