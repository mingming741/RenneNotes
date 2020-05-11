# Latex
Latex (LAY-tek or LAH-tek) 目前通用的文档排版工具，对于各种公式可视化非常方便。其操作原理类似于HTML的自动排版。

Overleaf为在线的latex的编辑工具，这个文档来自于https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes#What_is_LaTeX.3F 。我是用google账号在overleaf上面注册的。


### Latex tutorial
这里介绍latex的基本用法，latex的文件通常是.tex结尾，可以认为latex是tex文件的一种extension，在保留了tex本身的特性之上，还加入了了新的features。
```latex
\documentclass{article}

\begin{document}
First document. This is a simple example, with no extra parameters or packages included.

\end{document}
```
`documentclass`说明了这个文件的overall view，"article"是最通用的文件类型，其他的还有"book","report","beamer"等等。在`\begin{document}`和`\end{document}`之间的为document的主体，类似html的boty。

```latex
\documentclass[12pt, letterpaper]{article}
\usepackage[utf8]{inputenc}
```
写在`\begin{document}`前面的内容用于调节document的overview，被称作preamble。中括号[]里面的为设置当前preamble的argument，而大括号{}中的为这个preamble的key对应的value。例如`\documentclass[12pt, letterpaper]{article}`，等于将font size设置为12pt，paper size为letterpaper。如果中括号中没有值的话，就会使用默认。`\usepackage[utf8]{inputenc}`表示使用utf8，推荐无脑使用。

```latex
\documentclass[12pt, letterpaper, twoside]{article}
\usepackage[utf8]{inputenc}

\title{First document}
\author{Hubert Farnsworth \thanks{funded by the Overleaf team}}
\date{February 2017}

\begin{document}
\maketitle
% This line here is a comment. It will not be printed in the document.
\end{document}
```
latex的title的information是写在preamble里面的，我们可以在body中调用`\maketitle`将title的信息打印出来。之类的title包括`title, author, date`。用`%` 标注出来的为latex的comment，不会显示在主体中。

```latex
Some of the \textbf{greatest}
discoveries in \underline{science} 
were made by \textbf{\textit{accident}}.

I am \emph{doki} 2333
```
上面的三句话写在body里面，`\textbf` `\underline` `\textit`分别对应加粗，下划线，斜体三个字体。可以看出来字体的语法为`\语法{内容}`。同时不同的语法之间可以嵌套。而`\emph{}`关键字表示强调，在不同的主体中有不同的behavior，可以无脑通用。latex的换行也为空格，单一空格不会换行，extra的空格才会形成换行，例如上面的例子中，一共为两行。

```latex
\documentclass{article}
\usepackage{graphicx}
\graphicspath{ {images/} }

\begin{document}
The universe is immense and it seems to be homogeneous, 
in a large scale, everywhere we look at.

\includegraphics[width=5cm]{doki}
\end{document}
```
图片，需要在preamble中include packet，使用`\usepackage{graphicx}`。即添加兼容图片的扩展包。这里`\includegraphics`为外部packet `graphicx`的函数，`doki`对应文件夹`image/doki.png`









