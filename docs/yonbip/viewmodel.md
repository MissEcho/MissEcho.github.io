## .excute() 事件

```javascript
//viewModel.excute(name,args) 通过cb.models.js里面的_get_data('events')即可获得所有excute事件
viewModel.excute('back',args)
viewModel.excute('columnSetting',args)
viewModel.excute('customInit',args)
viewModel.excute('extendReady',args)
viewModel.excute('filterClick',args)
viewModel.excute('refresh',args)
viewModel.excute('return',args)
viewModel.excute('toggle',args)
```



## .communication事件

```javascript
// containerModel.prototype.communication
viewModel.communication({ type: 'return' });
//或者说通过该方法，打开model，html，以及其他自定义组件，比如
viewmodel.communication({
    type: 'modal',
    payload: {
        mode: 'inner',
        groupCode: 'form221ih', // 设计器左边容器编号
        viewmodel: viewmodel,
    },
});

viewModel.communication({
    type: 'modal',
    payload: {
        key: 'singleupload',
        data: {
            viewModel: viewModel,
            myParam: {
                title: '弹窗示例之扩展弹窗',
                data: data,
                billnum: billNo,
            },
        },
    },
});
viewmodel.communication({
    type: 'modal',
    payload: {
        mode: 'html',
        html: html,
    },
});

```

