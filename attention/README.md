# 四、注意点

1.如果是集合类型，那么该工具只支持泛型中的直接指明的类型，比如

```java
@Check
List<AEntity> entityList;

@Check
List<List<AEntity>> entityList;
```
而下面的这些暂时是不支持的（后面可以考虑支持）

```java
@Check
List<?> dataList;

@Check
List dataList;

@Check
List<? extend AEntity> dataList;

@Check
List<T> dataList;
```
