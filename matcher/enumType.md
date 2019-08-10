# enumType
表示枚举类型，修饰String类型的属性，用于表示该String类型的属性是多个枚举的名字

```java
@FieldWhiteMatcher(enumType = AEnum.class)
private String name;

@FieldWhiteMatcher(enumType = {AEnum.class, BEnum.class})
private String tag;
```
比如某个枚举类

```java
/**
 * @author zhouzhenyong
 * @since 2019/4/13 下午9:37
 */
@Getter
public enum AEnum {
    A1("a1"),
    A2("a2"),
    A3("a3");

    private String name;

    AEnum(String name) {
        this.name = name;
    }
}
```

```java
/**
 * @author zhouzhenyong
 * @since 2019/4/13 下午9:42
 */
@Getter
public enum  BEnum {

    B1("b1"),
    B2("b2"),
    B3("b3");

    private String name;

    BEnum(String name) {
        this.name = name;
    }
}
```

