# judge
除了上面的一些用法之外，这里还支持系统内部自己进行判断，其中表达式的格式为

```text
class全路径#函数名，比如：com.xxx.AEntity#isValid，其中isValid的入参是当前属性的类型
```

#### 用例：

```java
@Data
@Accessors(chain = true)
public class JudgeEntity {

    @FieldWhiteMatcher(judge = "com.simonalong.mikilin.judge.JudgeCheck#ageValid")
    private Integer age;

    @FieldWhiteMatcher(judge = "com.simonalong.mikilin.judge.JudgeCheck#nameValid")
    private String name;

    @FieldBlackMatcher(judge = "com.simonalong.mikilin.judge.JudgeCheck#addressInvalid")
    private String address;

    @FieldWhiteMatcher(judge = "com.simonalong.mikilin.judge.JudgeCheck#ratioJudge")
    private Float mRatio;

    private Float nRatio;
}
```
对于用户自定义的匹配类，这里分为两种，一种就是普通的类非Spring，如下，这种在处理上面的判决的时候，会进行单例化，对于spring的方式，则可以采用Bean的方式，下面进行介绍
### 1.用户自定义回调非spring

其中系统的匹配判决函数
```java
public class JudgeCls {

    /**
     * 年龄是否合法
     */
    public boolean ageValid(Integer age) {
        if(null == age){
            return false;
        }
        if (age >= 0 && age < 200) {
            return true;
        }

        return false;
    }

    /**
     * 名称是否合法
     */
    private boolean nameValid(String name) {
        if(null == name){
            return false;
        }
        List<String> blackList = Arrays.asList("women", "haode");
        if (blackList.contains(name)) {
            return false;
        }
        return true;
    }

    /**
     * 地址匹配
     */
    private boolean addressInvalid(String address){
        if(null == address){
            return true;
        }
        List<String> blackList = Arrays.asList("beijing", "hangzhou");
        if (blackList.contains(address)) {
            return true;
        }
        return false;
    }
}
```

### 2.用户自定义匹配器spring方式
对于业务系统中，对于常见的spring方式加载的Bean，我们这里也做了适配，可以将用户的匹配器作为一个Bean存在，不过需要多做一次处理，需要在对应的位置配置下扫描指定的路径：
```java
@ComponentScan(value = "com.simonalong.mikilin.util")
```
然后用户的匹配器就可以这样写，这样就可以使用spring中的其他bean来进行更多的处理了
```java
@Service
public class JudgeCls {

    /**
     * 年龄是否合法
     */
    public boolean ageValid(Integer age) {
        if(null == age){
            return false;
        }
        if (age >= 0 && age < 200) {
            return true;
        }

        return false;
    }

    /**
     * 名称是否合法
     */
    private boolean nameValid(String name) {
        if(null == name){
            return false;
        }
        List<String> blackList = Arrays.asList("women", "haode");
        if (blackList.contains(name)) {
            return false;
        }
        return true;
    }

    /**
     * 地址匹配
     */
    private boolean addressInvalid(String address){
        if(null == address){
            return true;
        }
        List<String> blackList = Arrays.asList("beijing", "hangzhou");
        if (blackList.contains(address)) {
            return true;
        }
        return false;
    }
}
```


