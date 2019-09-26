# model
该属性一般用于修饰String类型的数据，表示命中系统内置的几种类型（目前系统内置了简单的几种）：

```text
手机号，身份证号，固定电话，邮箱，IP地址
```

使用方式比如：

```java
@WhiteMatcher(model = FieldType.FIXED_PHONE)
private String fixedPhone;

@BlackMatcher(model = FieldType.FIXED_PHONE)
private String fixedPhoneInValid;
```


