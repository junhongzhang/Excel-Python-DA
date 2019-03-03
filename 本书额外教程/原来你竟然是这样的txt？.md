![](https://i.loli.net/2019/03/03/5c7b2421ca838.jpg)
##### 总第134篇/张俊红
# 1.前言
经常有同学问我，老师为啥同样的格式的两个文件我用同样的方法导入到Python里面，一个可以正常导入，一个却会报错，这是为什么呢？你应该也有遇到过这种情况，就是表面相同的文件，文件名完全相同，格式完全相同（至少肉眼看上去是），而且里面的内容也是一样的，但是你用同样的代码却不能打开每一个文件。
> memberinfo.txt
memberinfo.txt
memberinfo.txt
memberinfo.txt

我们本篇就来讲讲这是为什么？

# 2.生成txt文件
要弄懂为什么会出现上面那种看起来完全一样的文件，但实际上却不能用同样的代码打开每一个文件的原因，我们首先看看这些看起来完全一样的文件是如何生成的。

主要是利用Excel中另存为格式，进行txt文件的生成。
![excel文件另存为格式选择](https://ws1.sinaimg.cn/large/006JdmJily1frouxj8h25j30jz0euabi.jpg)

## 2.1生成文本文件
将Excel文件另存为`文本文件(制表符分隔(*.txt))`格式的文件，这样就生成第一个memberinfo.txt文件。

## 2.2生成Unicode文本
将Excel文件另存为`Unicode文本(*.txt)`格式的文件，这样就生成了第二个memberinfo.txt文件。

## 2.3生成CSV文件
先将Excel文件另存为`CSV(逗号分隔)(*csv)`格式的文件`memberinfo.csv`，然后直接将文件名强制更改成`memberinfo.txt`,这样就生成第三个memberinfo.txt文件了。

## 2.4生成CSV UTF-8文件
先将Excel文件另存为`CSV UTF-8(逗号分隔)(*csv)`格式的文件`memberinfo.csv`，然后直接将文件名强制更改成`memberinfo.txt`,这样就生成第四个memberinfo.txt文件了。

这样大家就知道了为什么表面上看起来一样的文件，却不能用同样的代码打开，主要是因为生成的方式(内部存储格式)是不一样的。

# 3.导入文件
我们主要讲述一下如何用Python导入这四种不同格式的txt文件。

## 3.1导入文本文件
因为文本文件是用制表符(\t)进行分隔的，所以我们在`read_table`的时候令`sep = '\t'`即可。
```python
df = pd.read_table(r"C:\Users\Desktop\memberinfo.txt",sep="\t",engine = "python",encoding="gbk")
```
## 3.2导入Unicode文本
因为**Pandas不支持读写`unicode`和`ascii`编码方式**的文件和数据，所以要读写这两类文件时，需要先将文件格式转换成**Pandas支持的`utf-8`或者`gbk`格式**，更改方式如下：
- step1:打开txt文件，选择另存为，我们可以看到红框部分的编码格式是`Unicode`。
![第一步打开txt文件](https://ws1.sinaimg.cn/large/006JdmJily1frova65q7gj30k604f3yl.jpg)
- step2:将文件编码格式修改为`utf-8`。
![第二步修改txt文件编码格式](https://ws1.sinaimg.cn/large/006JdmJily1frovebbjamj30l406e3yx.jpg)

这样就可以进行正常导入了,只需要将上述的encoding从`gbk`改成`utf-8`就可以。

```python
df = pd.read_table(r"C:\Users\Desktop\memberinfo.txt",sep="\t",engine = "python",encoding="gbk")
```
## 3.3导入CSV文件
因为这个txt文件是直接将CSV文件格式进行更改的，文件格式和CVS文件格式一致，逗号分隔(sep=",")，gbk编码(encoding="gbk")，所以，导入txt文件时也需要遵循这样的格式。
```python
df = pd.read_table(r"C:\Users\Desktop\memberinfo.txt",sep=",",engine = "python",encoding="gbk")
```
## 3.4导入CSV UTF-8文件
这个文件和上面的CSV文件唯一不同的就是编码格式不同，这个编码格式是`utf-8`，所以导入的时候只需要在CSV文件的基础上改一下编码格式即可。
```python
df = pd.read_table(r"C:\Users\Desktop\memberinfo.txt",sep=",",engine = "python",encoding="utf-8")
```
现在你应该很清楚txt外表一样，内心不一样的原因了吧。
