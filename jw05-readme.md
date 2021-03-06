# jw05数据

## jw04数据也可用
jw04的数据依旧适合jw05，因为jw05包含了jw04的功能。请别忘了用jw04的数据测试。

## 尚未解决的问题
jw05的测试数据依旧有没有解决的问题（如果我忽略了什么，请issue提出）：

- 少参数、错参数（例如`login -q ...`）的情况，并未包含在jw5的测试数据里面。一个原因是pdf的说明不够清楚，这也是jw04的问题。但是大家一定要处理这种情况，我个人是输出input-illegal处理的。
- jw5 pdf 末尾的**input illegal**只说了“该输入数字的地方输了字母” 。那么，nc和udc的教师列表格式错误（例如`[12345,]`）应该输出什么啊？input illegal？还是输出由nc和udc各自的错误信息？

## jw05.yml
**注意**：教师列表格式错误，采用了`Input illegal.`的输出。后续出了通知会更新测试数据的。

使用之前，应当用jw04的数据进行测试，因为`jw05.yml`里面省略了某些细节，而这些细节包含在jw04的测试数据中。

## 需要重视的问题

### 课程号大小写问题

这是摘自jw3 pdf的：
> 查找时课程编号中字⺟不区分⼤⼩写，即bhxxx，Bhxxx皆有效，除此之外要求完全匹
配。测试数据保证满⾜`^[bB][hH]\d{8}$`。

另外，我隐约记得（是在wx群还是sp论坛看到的），jw3的测试数据只包含大写的BH。

但是，jw05请注意，测试数据不会有保证，并且请仔细考虑储存的方式（应当保持原样储存课程号码，以及按照原样排序（用String.compareTo）。注意`bH`和`Bh`的字典序差距很大，原样的行为和全部用大写存起来的不同）