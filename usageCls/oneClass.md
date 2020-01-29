# 一个类
该类为`Checks` ，里面只包含如下四个个函数用于核查，两种维度：分组还有属性列表

```java
// 核查复杂对象的所有属性
public boolean check(Object object){}

// 核查复杂对象的部分属性
public boolean check(Object object, String... fieldSet){}

// 分组核查对象
public boolean check(String group, Object object) {}

// 分组核查对象的具体属性
public boolean check(String group, Object object, String... fieldSet) {}

// 核查失败抛异常
public void checkWithException(Object object) throws MkCheckException;
public void checkWithException(Object object, String ...fieldSet) throws MkCheckException;
public void checkWithException(String group, Object object) throws MkCheckException;
public void checkWithException(String group, Object object, String ...fieldSet) throws MkCheckException;
```


