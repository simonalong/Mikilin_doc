# 一个类
该类为`Checks` ，里面只包含如下四个个函数用于核查，两种维度：分组还有属性列表。还包括一个核查失败后的异常打印函数，只有在返回值为false的时候才需要用到

```java
// 核查复杂对象的所有属性
public boolean check(Object object){}

// 核查复杂对象的部分属性
public boolean check(Object object, String... fieldSet){}

// 分组核查对象
public boolean check(String group, Object object) {}

// 分组核查对象的具体属性
public boolean check(String group, Object object, String... fieldSet) {}

// 异常时候的异常信息获取
public String getErrMsg() {}
```


