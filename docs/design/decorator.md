> 装饰者(decorator)模式能够在不改变对象自身的基础上，动态的给某个对象添加额外的职责

```
Function.prototype.before=function (beforefn) {
    var _this= this;                               //保存旧函数的引用
    return function () {                           //返回包含旧函数和新函数的“代理”函数
        beforefn.apply(this,arguments);            //执行新函数,且保证this不被劫持,新函数接受的参数
        // 也会被原封不动的传入旧函数,新函数在旧函数之前执行
        return _this.apply(this,arguments);
    };
};

var validata=function () {
    if(username.value===''){
        alert('用户名不能为空!')
        return false;
    }
    if(password.value===''){
        alert('密码不能为空!')
        return false;
    }
}

var formSubmit=function () {
    var param={
        username=username.value;
        password=password.value;
    }

    ajax('post','http://www.xxx.com',param);
}

formSubmit= formSubmit.before(validata);


submitBtn.onclick=function () {
    formSubmit();
}
```
