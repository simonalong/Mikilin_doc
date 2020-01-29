# range
表示修饰类型数据的范围，目前支持两种类型，一个是数字（包括byte, short, int, Long, float, double及包装类以及其他的数字类型），一个是时间类型：java.util.Date以及它的继承类，只需要可以比较大小即可

### 数字类型

| 范围 | 详情 |
| ------ | ------ |
| [a, b] | 表示数字>=a且<=b |
| [a, b) | 表示数字>=a且<b |
| (a, b] | 表示数字>a且<=b |
| (a, b) | 表示数字>a且<b |
| (null, b] | 表示数字<=b |
| (, b] | 表示数字<=b |
| (null, b) | 表示数字<b |
| (, b) | 表示数字<b |
| [a, null) | 表示数字>=a |
| [a,) | 表示数字>=a |
| (a, null) | 表示数字>a |
| (a,) | 表示数字>a |

#### 例子：

```java
@Data
@Accessors(chain = true)
public class RangeEntity4 {

    /**
     * 属性为大于100
     */
    @WhiteMatcher(range = "(100, null)")
    private Integer num1;

    /**
     * 属性为大于等于100
     */
    @WhiteMatcher(range = "[100, null)")
    private Integer num2;

    /**
     * 属性为大于20且小于50
     */
    @WhiteMatcher(range = "(20, 50)")
    private Integer num3;

    /**
     * 属性为小于等于50
     */
    @WhiteMatcher(range = "(null, 50]")
    private Integer num4;
    
    /**
     * 属性为大于等于20且小于等于50
     */
    @WhiteMatcher(range = "[20, 50]")
    private Integer num5;
    
    /**
     * 属性为大于等于100
     * @since 1.4.4
     */
    @WhiteMatcher(range = "(100, )")
    private Integer num6;

    @WhiteMatcher(range = "[100,)")
    private Integer num7;

    @WhiteMatcher(range = "(, 50)")
    private Integer num8;

    @WhiteMatcher(range = "(,50]")
    private Integer num9;
}
```
### 时间类型
修饰的类型可以为Date类型，也可以为Long类型，这里表示的是时间范围，用法跟上面的数字是相同的，但是这里是字符，此外，对于时间的类型，这里也增加了额外的几个函数，用于简化表示

| 函数名 | 表示 |
| ------ | ------ |
| now | 当前时间 |
| past | 过去的时间范围，跟(null, now)相同 |
| future | 表示未来的时间范围，跟(now, null)相同 |

#### 用例

|表示|描述|
| ------ | ------ |
| ['2019-02-12 13:23:50.281', '2019-02-22 13:23:50.281'] | 表示匹配在该时间之内的值，包括边界值 |
| ('2018-02-12 13:23:50.281', '2018-02-22 13:23:50.281'] | 不包括左边界值 |
| ('2018-02-12 13:23', '2018-02-22 13:23:50.281'] | 开始时间是当前下午13点23分00 |
| ('2018-02-12 13', '2018-02-22 13:23:50.281'] | 开始时间是当前下午13点00分00 |
| ('2018-02-12', '2018-02-22 13:23:50.281'] | 开始时间是2月12号00点00分00 |
| ('2018-02-12 13:23:50.281', now] | 从过去的时间到现在 |
| (now, '2020-02-12 13:23:50.281'] | 从现在到以后的某个时间 |
| (null, '2020-02-12 13:23:50.281') | 从过去到以后的某个时间 |
| past | 过去的任何时间 |
| future | 未来的任何时间 |

其中时间的格式支持如下这么几种

| 类型 | 表示 |
| ------ | ------ |
| yyyy | 年 |
| yyyy-MM | 年月 |
| yyyy-MM-dd | 年月日 |
| yyyy-MM-dd HH | 年月日时 |
| yyyy-MM-dd HH:mm | 年月日时分 |
| yyyy-MM-dd HH:mm:ss | 年月日时分秒 |
| yyyy-MM-dd HH:mm:ss.SSS | 年月日时分秒毫秒 |

#### 注意
- 1.对于其中的字符，(null, now)这种添加字符也是可以识别的，比如('null', 'now')，past和future一样
- 2.对于时间的范围，如果起点比终点大则会解析失败

```java
@Data
@Accessors(chain = true)
public class RangeTimeEntity {

    /**
     * 属性为：2019-07-13 12:00:23.321 到 2019-07-23 12:00:23.321的时间
     */
    @WhiteMatcher(range = "['2019-07-13 12:00:23.321', '2019-07-23 12:00:23.321']")
    private Date date1;

    /**
     * 属性为：2019-07-13 12:00:23.000 到 2019-07-23 12:00:00.000 的时间
     */
    @WhiteMatcher(range = "['2019-07-13 12:00:23', '2019-07-23 12:00']")
    private Date date2;
    
    /**
     * 属性为：2019-07-13 00:00:00.000 到 2019-07-01 00:00:00.000 的时间
     */
    @WhiteMatcher(range = "['2019-07-13', '2019-07']")
    private Long dateLong3;
        
    /**
     * 属性为：现在时间 到 2019-07-23 12:00:23.321 的时间
     */
    @WhiteMatcher(range = "(now, '2019-07-23 12:00:23.321']")
    private Date date4;
    
    /**
     * 属性为：2019-07-13 00:00:00.000 到现在的时间
     */
    @WhiteMatcher(range = "['2019-07-13', now)")
    private Date date5;
    
    /**
     * 属性为：过去的时间，同下面的past
     */
    @WhiteMatcher(range = "(null, now)")
    private Date date6;
    
    /**
     * 属性为：过去的时间，同下面的past
     */
    @WhiteMatcher(range = "('null', 'now')")
    private Date date7;
    
    /**
     * 属性为：过去的时间，同上
     */
    @WhiteMatcher(range = "past")
    private Date date8;
    
    /**
     * 属性为：未来的时间，同下面的future
     */
    @WhiteMatcher(range = "(now, null)")
    private Date date9;
    
    /**
     * 属性为：未来的时间，同下面的future
     */
    @WhiteMatcher(range = "future")
    private Date date10;
}
```

### 集合类型
集合这里只核查集合的数据大小
```java
@Data
@Accessors(chain = true)
public class CollectionSizeEntityA {

    private String name;

    /**
    * 对应集合的个数不为空，且个数小于等于2 
    */
    @WhiteMatcher(range = "(0, 2]")
    private List<CollectionSizeEntityB> bList;
}
```