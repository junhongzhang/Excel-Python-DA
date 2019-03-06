**Jupyter Notebook**是很好的数据科学创作环境，反正我做数据分析的项目或小练习的时候，基本都是在用jupyter notebook（原先是叫ipython notebook，所以现在文件后缀还是.ipynb），以前不怎么用到导出pdf功能，然后要用的时候就遇到很多坑了。jupyter提供导出的格式有.py、.html、.md、.pdf等。

![jupyter notebook支持的导出格式](https://upload-images.jianshu.io/upload_images/2473543-b37f85b5584364b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从效果来看，网页中notebook的渲染是最好看的，导出的html对代码和超链接失真严重。在网页上点*Download as -> PDF via LaTex*的时候先是说缺少Pandoc库，于是pip install pandoc，之后不再说缺少这个库了，而是
 nbconvert failed: pdflatex not found on PATH 或者 nbconvert failed: PDF creating failed, captured latex output。查了一些资料后改用命令行，要避免*'xelatex' 不是内部或外部命令，也不是可运行的程序或批处理文件*，需要先安装MiKTeX，在其[官网下载](https://miktex.org/download)后，Windows版一路next安装就行，安装包有190MB，安装过程还是耗费些时间的，下载安装完成之后的步骤是：

### 1, ipynb文件编译为tex 
在命令行中定位到要转换的jupyter文件的路径下，输入
 **jupyter nbconvert --to latex yourNotebookName.ipynb**

![编译ipynb文件为LaTeX文件](https://upload-images.jianshu.io/upload_images/2473543-3066970796a6043b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在文件目录下就可以看到一个叫**yourNotebookName.tex**的LaTeX文件了。
### 2, 手动编辑latex文件
为了能支持输出中文，需要改一下tex文件，在编辑器（我用的是Notepad++）打开刚才生成的LaTeX文件，
在**\documentclass{article}**（没有这一句就在\documentclass[11pt]{ctexart} 的后面插入下面的语句）后面插入
```latex
\usepackage{fontspec, xunicode, xltxtra}
\setmainfont{Microsoft YaHei}
```
![修改latex文件](https://upload-images.jianshu.io/upload_images/2473543-898fdf8271689505.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3, 转latex为pdf
随后在命令行下输入：（我演示文件用的是GeoCluster.tex）
```
xelatex yourNotebookName.tex
```
![命令行转latex为pdf](https://upload-images.jianshu.io/upload_images/2473543-6624da52f9d4d9d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
之前没有运行过xelatex，首次运行会安装一些依赖文件，会慢一些，最后运行完毕：
![运行完xelatex命令](https://upload-images.jianshu.io/upload_images/2473543-192ac8f3fe434b96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以在文件夹下看到输出的文件：
![最后文件夹下的结果](https://upload-images.jianshu.io/upload_images/2473543-c7f89da3bad6866f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- .ipynb 是我们的jupyter文件
- .tex 是由jupyter notebook文件生成的
- .pdf 是我们最后的目标文件由.tex文件生成
- .log、.out、.aux是LaTex生成pdf的一些输出和日志

总结一下，从jupyter notebook生成pdf文件需要的依赖项还是比较多的，Windows下安装MiKTeX才能用xelatex命令。生成步骤是先把ipynb文件编译为LaTex，然后为了支持中文修改一下lex文件，最后转换为pdf文件。 

最后效果如下，虽然还是比不上网页端.ipynb的直接渲染效果，但比起导出的html等格式，更好地作为展示格式。
![生成pdf的效果](https://upload-images.jianshu.io/upload_images/2473543-036c476dcddbbca0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ps:
- 现在觉得下载安装部分说得有些简略，之后可以把这部分说得更详细；
- 原文[简书链接](https://www.jianshu.com/p/6b84a9631f8a)
