# Python中的编码问题

Python2版本与Python3版本的差异之一，即编码问题。在理解编码之前，先介绍两个概念。其中一个是字面量，另一个是字节码。

在写代码时，我们会定义字符串变量，用来表示一段文本内容。比方说 `s="helloworld"`，这就是一个简单的字面量。我们也知道，代码中的字符串变量，作为操作数可能会用于网络数据的交互，或者我们会将这个内容存储到持久存储中。计算机的底层存储数据本质上是0和1，那么如何将这个字面量转换成字节流的数据，就是对字面量的编码。这也就是为什么目前会有这么多种编码方式，比方说`utf-8`，`gbk`，`ascii`等。

那么在Python2和Python3中，字面量和字节码是以不同的类型来表示的，具体如下：

|  版本   | 字节码type | 字面量type | 默认编码方式 |
| :-----: | :--------: | :--------: | :----------: |
| Python2 |    str     |  unicode   |    ascii     |
| Python3 |    byte    |    Str     |    utf-8     |

这里我们重点关注两个方面，即字节码和字面量的转换，以及默认编码方式带来的影响。

1.字节码与字面量的转换

总的来说，转换关系是：

 `字节码.encode(编码格式)-->字面量`

`字面量.decode(编码格式)-->字节码`

2.默认编码方式的影响

在Python3中使用utf-8作为默认编码，不会经过unicode编码(utf-8是unicode一种变长类型)，因此转换主要在字面量和utf-8之间。

在Python2中相对来说会比较复杂，控制台显示的处理方式与Python3不同。第一点，由于字面量是unicode类型，显示的时候也是一个unicode编码，没有显示字面量本来的样子，但其实他们本质是一样的。

```python
# 这是在Python2中
>>> a = '深圳'
>>> a
u'\u6df1\u5733'

# 对比在Python3中，其实本质是一样的
>>> '\u6df1\u5733'.encode('utf-8').decode('utf-8')
>>> ‘深圳’
```

第二点，Python2中的unicode字面量必须要显示的带u来表示，例如`a=u'深圳'`。若不带u显示表示，则是一个str字节码。

第三点，在Python3不能对字节码byte进行encode操作，而在Python2中可以对str进行encode操作。在Python2对字节码变量执行encode动作时，默认会先decode成字面量，然后再使用给定编码格式encode。这里需要注意的是decode成字面量的过程是系统自动完成的，采用的是默认的编码方式即ASCII。对于`a='深圳'`这样一个含中文的字节码来说，采用ASCII是无法decode操作的(ASCII只能decode前255个字符)，因此需要修改解释器默认的编码方式。

```python
# 含中文字符，错误展示
>>> a = '深圳'
>>> a.encode('gbk')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeDecodeError: 'ascii' codec can't decode byte 0xe6 in position 0: ordinal not in range(128)
  
# 纯ASCII文本，可以encode
>>> a = 'zimsky'
>>> a.encode('gbk')
'zimsky'
```

```python
# 修改解释器默认编码方式
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
```



