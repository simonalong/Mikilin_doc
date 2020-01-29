# type
借鉴java validator中的对属性类型的指定，这里也增加一个这样的属性，用于对修饰的类型进行指定，举例如下
```java
@Data
@Accessors(chain = true)
public class TypeEntity {

    @WhiteMatcher(type = Integer.class)
    private Integer data;

    @WhiteMatcher(type = CharSequence.class)
    private String name;

    @WhiteMatcher(type = {Integer.class, Float.class})
    private Object obj;

    @WhiteMatcher(type = Number.class)
    private Object num;
}
```

对于其中属性obj，该属性可以填写Integer.class，也可以填写Float.class，但是如果填写其他的则就会被拒绝掉，就像Long.class也是会被拒绝，而对于num，则Long.class是会接受的，因为该属性接受一切数字类型。对应的测试如下
```groovy
def "测试不明写类继承关系1"() {
    given:
    TypeEntity entity = new TypeEntity().setObj(obj)

    expect:
    boolean actResult = Checks.check(entity, "obj")
    if (!result) {
        println Checks.getErrMsg()
    }
    Assert.assertEquals(result, actResult)

    where:
    obj    | result
    'c'    | false
    "abad" | false
    1232   | true
    1232l  | false
    1232f  | true
    12.0f  | true
    -12    | true
}

/**
 * 可以让一切数字类型通过
 * @return
 */
def "测试不明写类继承关系2"() {
    given:
    TypeEntity entity = new TypeEntity().setNum(obj)

    expect:
    boolean actResult = Checks.check(entity, "num")
    if (!result) {
        println Checks.getErrMsg()
    }
    Assert.assertEquals(result, actResult)

    where:
    obj    | result
    'c'    | false
    "abad" | false
    1232   | true
    1232l  | true   // 这里跟上面的例子的区别
    1232f  | true
    12.0f  | true
    -12    | true
}
```

