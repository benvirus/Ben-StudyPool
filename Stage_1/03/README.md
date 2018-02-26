# 琐碎的知识点儿

### form 表单在不填写action的情况下，数据会被提交到哪里？

答：当前页面

### 检查一个对象是否是一个dom节点

    if(el.nodeName){
        //.....
    }

### 页面中的内容禁止选择

    <div unselectable="on" onselectstart="return false;" style="-moz-user-select:none;-webkit-user-select:none;">
        123
    </div>

