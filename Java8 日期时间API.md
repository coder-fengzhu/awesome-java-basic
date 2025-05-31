## 旧版日期时间API存在的问题
+ <font style="color:rgb(85, 85, 85);">Java的</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">java.util.Date</font><font style="color:rgb(85, 85, 85);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">java.util.Calendar</font><font style="color:rgb(85, 85, 85);">类易用性差，不支持时区，而且他们都不是线程安全的；</font>



+ <font style="color:rgb(85, 85, 85);">用于格式化日期的类</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">DateFormat</font><font style="color:rgb(85, 85, 85);">被放在</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">java.text</font><font style="color:rgb(85, 85, 85);">包中，它是一个抽象类，所以我们需要实例化一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">SimpleDateFormat</font><font style="color:rgb(85, 85, 85);">对象来处理日期格式化，并且</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">DateFormat</font><font style="color:rgb(85, 85, 85);">也是非线程安全，你得把它用</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">ThreadLocal</font><font style="color:rgb(85, 85, 85);">包起来才能在多线程中使用。</font>



+ <font style="color:rgb(85, 85, 85);">对日期的计算方式繁琐，而且容易出错，因为月份是从0开始的，从</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Calendar</font><font style="color:rgb(85, 85, 85);">中获取的月份需要加一才能表示当前月份。</font>



```java
public static void main(String[] args) {
        //旧版日期时间 API 存在的问题
        //1.设计不合理(JDK8 Date类已经 @Deprecated 注释,不推荐使用)
        Date now = new Date(1985, 9, 23);
        System.out.println(now);

        //2.时间格式化和解析是线程不安全的
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        for (int i = 0; i < 50; i++) {
            new Thread(() -> {
                try {
                    Date date = sdf.parse("2019-09-09");
                    System.out.println("date:" + date);
                } catch (ParseException e) {
                    e.printStackTrace();
                }
            }).start();
        }
}
```





## 新日期时间API介绍
### 关键类介绍
+ **<font style="color:rgb(106, 115, 125) !important;">LocalDate</font>**<font style="color:rgb(106, 115, 125) !important;">：表示日期，包含：年月日。格式为：2020-01-13</font>
+ **<font style="color:rgb(106, 115, 125) !important;">LocalTime</font>**<font style="color:rgb(106, 115, 125) !important;">：表示时间，包含：时分秒。格式为：16:39:09.307</font>
+ **<font style="color:rgb(106, 115, 125) !important;">LocalDateTime</font>**<font style="color:rgb(106, 115, 125) !important;">：表示日期时间，包含：年月日 时分秒。格式为：2020-01-13T16:40:59.138</font>
+ **<font style="color:rgb(106, 115, 125) !important;">DateTimeFormatter</font>**<font style="color:rgb(106, 115, 125) !important;">：日期时间格式化类</font>
+ **<font style="color:rgb(106, 115, 125) !important;">Instant</font>**<font style="color:rgb(106, 115, 125) !important;">：时间戳类</font>
+ **<font style="color:rgb(106, 115, 125) !important;">Duration</font>**<font style="color:rgb(106, 115, 125) !important;">：用于计算 2 个时间(LocalTime，时分秒)之间的差距</font>
+ **<font style="color:rgb(106, 115, 125) !important;">Period</font>**<font style="color:rgb(106, 115, 125) !important;">：用于计算 2 个日期(LocalDate，年月日)之间的差距</font>
+ **<font style="color:rgb(106, 115, 125) !important;">ZonedDateTime</font>**<font style="color:rgb(106, 115, 125) !important;">：包含时区的时间</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);"></font>

### <font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDate</font><font style="color:rgb(85, 85, 85);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalTime</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDate</font><font style="color:rgb(85, 85, 85);">类表示一个具体的日期，但不包含具体时间，也不包含时区信息。可以通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDate</font><font style="color:rgb(85, 85, 85);">的静态方法</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">of()</font><font style="color:rgb(85, 85, 85);">创建一个实例，</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDate</font><font style="color:rgb(85, 85, 85);">也包含一些方法用来获取年份，月份，天，星期几等：</font>

```java
LocalDate localDate = LocalDate.of(2017, 1, 4);     // 初始化一个日期：2017-01-04
int year = localDate.getYear();                     // 年份：2017
Month month = localDate.getMonth();                 // 月份：JANUARY
int dayOfMonth = localDate.getDayOfMonth();         // 月份中的第几天：4
DayOfWeek dayOfWeek = localDate.getDayOfWeek();     // 一周的第几天：WEDNESDAY
int length = localDate.lengthOfMonth();             // 月份的天数：31
boolean leapYear = localDate.isLeapYear();          // 是否为闰年：false
```



<font style="color:rgb(85, 85, 85);">也可以调用静态方法</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">now()</font><font style="color:rgb(85, 85, 85);">来获取当前日期：</font>

```java
LocalDate now = LocalDate.now();
```

<font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalTime</font><font style="color:rgb(85, 85, 85);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDate</font><font style="color:rgb(85, 85, 85);">类似，他们之间的区别在于</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDate</font><font style="color:rgb(85, 85, 85);">不包含具体时间，而</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalTime</font><font style="color:rgb(85, 85, 85);">包含具体时间，例如：</font>

```java
LocalTime localTime = LocalTime.of(17, 23, 52);     // 初始化一个时间：17:23:52
int hour = localTime.getHour();                     // 时：17
int minute = localTime.getMinute();                 // 分：23
int second = localTime.getSecond();                 // 秒：52
```



### <font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDateTime</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDateTime</font><font style="color:rgb(85, 85, 85);">类是</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDate</font><font style="color:rgb(85, 85, 85);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalTime</font><font style="color:rgb(85, 85, 85);">的结合体，可以通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">of()</font><font style="color:rgb(85, 85, 85);">方法直接创建，也可以调用</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDate</font><font style="color:rgb(85, 85, 85);">的</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">atTime()</font><font style="color:rgb(85, 85, 85);">方法或</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalTime</font><font style="color:rgb(85, 85, 85);">的</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">atDate()</font><font style="color:rgb(85, 85, 85);">方法将</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDate</font><font style="color:rgb(85, 85, 85);">或</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalTime</font><font style="color:rgb(85, 85, 85);">合并成一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDateTime</font><font style="color:rgb(85, 85, 85);">：</font>

```java
LocalDateTime ldt1 = LocalDateTime.of(2017, Month.JANUARY, 4, 17, 23, 52);

LocalDate localDate = LocalDate.of(2017, Month.JANUARY, 4);
LocalTime localTime = LocalTime.of(17, 23, 52);
LocalDateTime ldt2 = localDate.atTime(localTime);
```

<font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDateTime</font><font style="color:rgb(85, 85, 85);">也提供用于向</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDate</font><font style="color:rgb(85, 85, 85);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalTime</font><font style="color:rgb(85, 85, 85);">的转化：</font>

```java
LocalDate date = ldt1.toLocalDate();
LocalTime time = ldt1.toLocalTime();
```



### Instant
<font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Instant</font><font style="color:rgb(85, 85, 85);">用于表示一个时间戳，它与我们常使用的</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">System.currentTimeMillis()</font><font style="color:rgb(85, 85, 85);">有些类似，不过</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Instant</font><font style="color:rgb(85, 85, 85);">可以精确到纳秒（Nano-Second），</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">System.currentTimeMillis()</font><font style="color:rgb(85, 85, 85);">方法只精确到毫秒（Milli-Second）。如果查看</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Instant</font><font style="color:rgb(85, 85, 85);">源码，发现它的内部使用了两个常量，</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">seconds</font><font style="color:rgb(85, 85, 85);">表示从1970-01-01 00:00:00开始到现在的秒数，</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">nanos</font><font style="color:rgb(85, 85, 85);">表示纳秒部分（</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">nanos</font><font style="color:rgb(85, 85, 85);">的值不会超过</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">999,999,999</font><font style="color:rgb(85, 85, 85);">）。</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Instant</font><font style="color:rgb(85, 85, 85);">除了使用</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">now()</font><font style="color:rgb(85, 85, 85);">方法创建外，还可以通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">ofEpochSecond</font><font style="color:rgb(85, 85, 85);">方法创建：</font>

```java
Instant instant = Instant.now();
Instant instant = Instant.ofEpochSecond(120, 100000);
```

 

### **<font style="color:rgb(106, 115, 125) !important;">Duration</font>**
<font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Duration</font><font style="color:rgb(85, 85, 85);">的内部实现与</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Instant</font><font style="color:rgb(85, 85, 85);">类似，也是包含两部分：</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">seconds</font><font style="color:rgb(85, 85, 85);">表示秒，</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">nanos</font><font style="color:rgb(85, 85, 85);">表示纳秒。两者的区别是</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Instant</font><font style="color:rgb(85, 85, 85);">用于表示一个时间戳（或者说是一个时间点），而</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Duration</font><font style="color:rgb(85, 85, 85);">表示一个时间段，所以</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Duration</font><font style="color:rgb(85, 85, 85);">类中不包含</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">now()</font><font style="color:rgb(85, 85, 85);">静态方法。可以通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Duration.between()</font><font style="color:rgb(85, 85, 85);">方法创建</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Duration</font><font style="color:rgb(85, 85, 85);">对象：</font>

```java
LocalDateTime from = LocalDateTime.of(2017, Month.JANUARY, 5, 10, 7, 0);    // 2017-01-05 10:07:00
LocalDateTime to = LocalDateTime.of(2017, Month.FEBRUARY, 5, 10, 7, 0);     // 2017-02-05 10:07:00
Duration duration = Duration.between(from, to);     // 表示从 2017-01-05 10:07:00 到 2017-02-05 10:07:00 这段时间

long days = duration.toDays();              // 这段时间的总天数
long hours = duration.toHours();            // 这段时间的小时数
long minutes = duration.toMinutes();        // 这段时间的分钟数
long seconds = duration.getSeconds();       // 这段时间的秒数
long milliSeconds = duration.toMillis();    // 这段时间的毫秒数
long nanoSeconds = duration.toNanos();      // 这段时间的纳秒数

Duration duration1 = Duration.of(5, ChronoUnit.DAYS);       // 5天
Duration duration2 = Duration.of(1000, ChronoUnit.MILLIS);  // 1000毫秒
```



### Period
<font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Period</font><font style="color:rgb(85, 85, 85);">在概念上和</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Duration</font><font style="color:rgb(85, 85, 85);">类似，区别在于</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Period</font><font style="color:rgb(85, 85, 85);">是以年月日来衡量一个时间段，比如2年3个月6天：</font>

```java
Period period = Period.of(2, 3, 6);
```

<font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Period</font><font style="color:rgb(85, 85, 85);">对象也可以通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">between()</font><font style="color:rgb(85, 85, 85);">方法创建，值得注意的是，由于</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">Period</font><font style="color:rgb(85, 85, 85);">是以年月日衡量时间段，所以between()方法只能接收LocalDate类型的参数：</font>

```java
// 2024-01-05 到 2024-02-05 这段时间
Period period = Period.between(
                LocalDate.of(2024, 1, 5),
                LocalDate.of(2024, 2, 5));
```



## 日期的操作和格式化
### <font style="color:rgb(85, 85, 85);">增加和减少日期</font>
<font style="color:rgb(85, 85, 85);">Java 8中的日期/时间类都是不可变的（用</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">final</font><font style="color:rgb(85, 85, 85);">修饰），这是为了保证线程安全。当然，新的日期/时间类也提供了方法用于创建对象的可变版本，比如增加一天或者减少一天：</font>

```java
LocalDate date = LocalDate.of(2024, 1, 5);          // 2024-01-05

LocalDate date1 = date.withYear(2016);              // 修改为 2016-01-05

LocalDate date4 = date.plusYears(1);                // 增加一年 2025-01-05
LocalDate date6 = date.plus(5, ChronoUnit.DAYS);    // 增加5天 2024-01-10
```

利用<font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">TemporalAdjusters</font><font style="color:rgb(85, 85, 85);">对象可以实现一些更复杂的时间操作</font>

```java
LocalDate date7 = date.with(nextOrSame(DayOfWeek.SUNDAY));      // 返回下一个距离当前时间最近的星期日
LocalDate date9 = date.with(lastInMonth(DayOfWeek.SATURDAY));   // 返回本月最后一个星期六
```

| <font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">dayOfWeekInMonth</font> | <font style="color:rgb(85, 85, 85);">返回同一个月中每周的第几天</font> |
| :--- | :--- |
| <font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">firstDayOfMonth</font> | <font style="color:rgb(85, 85, 85);">返回当月的第一天</font> |
| <font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">firstDayOfNextMonth</font> | <font style="color:rgb(85, 85, 85);">返回下月的第一天</font> |
| <font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">firstDayOfNextYear</font> | <font style="color:rgb(85, 85, 85);">返回下一年的第一天</font> |
| <font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">firstDayOfYear</font> | <font style="color:rgb(85, 85, 85);">返回本年的第一天</font> |




也可以实现TemporalAdjuster 接口，实现一些自定义的时间逻辑

### 格式化日期
<font style="color:rgb(85, 85, 85);">新的日期API中提供了一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">DateTimeFormatter</font><font style="color:rgb(85, 85, 85);">类用于处理日期格式化操作，它被包含在</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">java.time.format</font><font style="color:rgb(85, 85, 85);">包中，Java 8的日期类有一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">format()</font><font style="color:rgb(85, 85, 85);">方法用于将日期格式化为字符串，该方法接收一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">DateTimeFormatter</font><font style="color:rgb(85, 85, 85);">类型参数：</font>

```java
LocalDateTime dateTime = LocalDateTime.now();
String strDate1 = dateTime.format(DateTimeFormatter.BASIC_ISO_DATE);    
String strDate2 = dateTime.format(DateTimeFormatter.ISO_LOCAL_DATE);    
String strDate3 = dateTime.format(DateTimeFormatter.ISO_LOCAL_TIME);    
String strDate4 = dateTime.format(DateTimeFormatter.ofPattern("yyyy-MM-dd"));
String strDate5 = dateTime.format(DateTimeFormatter.ofPattern("今天是：YYYY年 MMMM DD日 E", Locale.CHINESE));
```

将字符串转成日期对象

```java
String strDate6 = "2024-03-20";
String strDate7 = "2024-03-20 12:30:05";

LocalDate date = LocalDate.parse(strDate6, DateTimeFormatter.ofPattern("yyyy-MM-dd"));
LocalDateTime dateTime1 = LocalDateTime.parse(strDate7, DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
```



## 时区
<font style="color:rgb(85, 85, 85);">新的时区类</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">java.time.ZoneId</font><font style="color:rgb(85, 85, 85);">是原有的</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">java.util.TimeZone</font><font style="color:rgb(85, 85, 85);">类的替代品。</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">ZoneId</font><font style="color:rgb(85, 85, 85);">对象可以通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">ZoneId.of()</font><font style="color:rgb(85, 85, 85);">方法创建，也可以通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">ZoneId.systemDefault()</font><font style="color:rgb(85, 85, 85);">获取系统默认时区：</font>

```java
ZoneId shanghaiZoneId = ZoneId.of("Asia/Shanghai");
ZoneId systemZoneId = ZoneId.systemDefault();
```

<font style="color:rgb(85, 85, 85);"></font>

<font style="color:rgb(85, 85, 85);">有了</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">ZoneId</font><font style="color:rgb(85, 85, 85);">，我们就可以将一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDate</font><font style="color:rgb(85, 85, 85);">、</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalTime</font><font style="color:rgb(85, 85, 85);">或</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDateTime</font><font style="color:rgb(85, 85, 85);">对象转化为</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">ZonedDateTime</font><font style="color:rgb(85, 85, 85);">对象：</font>

```java
LocalDateTime localDateTime = LocalDateTime.now();
ZonedDateTime zonedDateTime = ZonedDateTime.of(localDateTime, shanghaiZoneId);
```



<font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">ZonedDateTime</font><font style="color:rgb(85, 85, 85);">对象由两部分构成，</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDateTime</font><font style="color:rgb(85, 85, 85);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">ZoneId</font><font style="color:rgb(85, 85, 85);">，其中</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">2017-01-05T15:26:56.147</font><font style="color:rgb(85, 85, 85);">部分为</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">LocalDateTime</font><font style="color:rgb(85, 85, 85);">，</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">+08:00[Asia/Shanghai]</font><font style="color:rgb(85, 85, 85);">部分为</font><font style="color:rgb(85, 85, 85);background-color:rgb(238, 238, 238);">ZoneId</font><font style="color:rgb(85, 85, 85);">。</font>

