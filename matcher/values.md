# values
用于表示只要的或者不要的值列表，一般用于`String`，`Integer`（会自动转成`Integer`），该属性用于表示修饰的属性对应的值，比如

```java
@FieldWhiteMatcher({"a", "b", "c", "null"})
private String name;

@FieldWhiteMatcher({"12", "32", "29"})
private Integer age;
```

表示一个类的属性名 name 允许的值（或禁止的值）只能为："a", "b", "c" 和null，属性名 age 允许的值（或禁止的值）只能为：12， 32， 29


