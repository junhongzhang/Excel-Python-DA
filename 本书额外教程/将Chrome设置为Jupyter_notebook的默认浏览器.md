# 1.前言
我们知道`jupyter_notebook`是在浏览器中打开的，这里建议大家都使用`Chrome`浏览器打开，因为其他浏览器可能会出现一些不兼容的问题。如果你电脑上有`Chrome`浏览器，而且平常已经习惯了使用`Chrome`浏览器，那么你打开`jupyter_notebook`的时候直接选择`Chrome`打开就行。如果你平常也不怎么使用`Chrome`,电脑上也没有安装，先去安装一个`Chrome`浏览器。
![就是这个](https://i.loli.net/2019/02/24/5c723d6381a41.png)
如果你在安装好`Chrome`浏览器之前已经用别的浏览器打开过`Jupyter_notebook`了，那么你就需要修改一下默认设置，让`Jupyter_notebook`用`Chrome`浏览器打开，具体设置方法如下。

# 2.获取Jupyter_notebook配置文件
我们首先需要找到`Jupyter_notebook`的配置文件`jupyter_notebook_config.py`在哪里，配置文件里面保存着`Jupyter_notebook`的各种设置。打开`Anaconda Promt`，运行以下命令就可以得到配置文件的路径。
```
jupyter notebook --generate-config
```
![配置文件路径](https://i.loli.net/2019/02/24/5c723d637f7f8.png)
来到对应的路径下我们就看到了配置文件，然后右键用记事本打开。利用`Ctrl+F`快捷键调出查找框查找`c.NotebookApp.browser`，找到对应位置。
![配置文件](https://i.loli.net/2019/02/24/5c723d639f57e.png)
![查找](https://i.loli.net/2019/02/24/5c723d63aa199.png)
# 3.获取Chrome安装位置
右键已经安装好的`Chrome`浏览器的桌面图标，然后选择属性，即可获取到`Chrome`的安装位置。下面红框框住的部分就是`Chrome`浏览器的安装位置。
![chrome安装位置](https://i.loli.net/2019/02/24/5c723d638075d.png)

# 4.加入设置语句块
在第2部分查找到的`c.NotebookApp.browser = ''`后面，即第2部分中红框框住的空白位置加入下面语句块：
```
import webbrowser

webbrowser.register("chrome",None,webbrowser.GenericBrowser(u"C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe"))

c.NotebookApp.browser = 'chrome'
```
`GenericBrowser`后面放的是第3部分中获取到的`Chrome`浏览器的安装位置。

重启`Jupyter_notebook`就会默认使用`Chrome`浏览器打开了。