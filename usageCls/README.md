# 一、类介绍

使用的时候，其实只要使用类`Checks`的两中函数即可，用法极其简单，先check，如果失败，则调用getErrMsg即可获取到失败信息
```groovy
boolean actResult = Checks.check(entity)
if (!actResult) {
    println Checks.getErrMsg();
}
```

关于函数`check`有多个重载，可以看详情