# Latex
Latex (LAY-tek or LAH-tek) 目前通用的文档排版工具，对于各种公式可视化非常方便。其操作原理类似于HTML的自动排版。

Overleaf为在线的latex的编辑工具，这个文档来自于https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes#What_is_LaTeX.3F 。我是用google账号在overleaf上面注册的。


# Latex tutorial
这里介绍latex的基本用法，latex的文件通常是.tex结尾，可以认为latex是tex文件的一种extension，在保留了tex本身的特性之上，还加入了了新的features。

### 文档 Document class
```latex
\documentclass{article}

\begin{document}
First document. This is a simple example, with no extra parameters or packages included.

\end{document}
```
`documentclass`说明了这个文件的overall view，"article"是最通用的文件类型，其他的还有"book","report","beamer"等等。在`\begin{document}`和`\end{document}`之间的为document的主体，类似html的boty。

### 宏 preamble
```latex
\documentclass[12pt, letterpaper]{article}
\usepackage[utf8]{inputenc}
```
写在`\begin{document}`前面的内容用于调节document的overview，被称作preamble。中括号[]里面的为设置当前preamble的argument，而大括号{}中的为这个preamble的key对应的value。例如`\documentclass[12pt, letterpaper]{article}`，等于将font size设置为12pt，paper size为letterpaper。如果中括号中没有值的话，就会使用默认。`\usepackage[utf8]{inputenc}`表示使用utf8，推荐无脑使用。

### 标题 title
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
latex的title的information是写在preamble里面的，我们可以在body中调用`\maketitle`将title的信息打印出来。之类的title包括`title, author, date`。用`%` 标注出来的为latex的comment，不会显示在主体中。这里`author`和`thanks`是合在一起的。

### 格式
```latex
Some of the \textbf{greatest}
discoveries in \underline{science} 
were made by \textbf{\textit{accident}}.

I am \emph{doki} 2333
```
上面的三句话写在body里面，`\textbf` `\underline` `\textit`分别对应加粗，下划线，斜体三个字体。可以看出来字体的语法为`\语法{内容}`。同时不同的语法之间可以嵌套。而`\emph{}`关键字表示强调，在不同的主体中有不同的behavior，可以无脑通用。latex的换行也为空格，单一空格不会换行，extra的空格才会形成换行，例如上面的例子中，一共为两行。

### 图片
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

```latex
\begin{figure}[h]
    \centering
    \includegraphics[width=0.25\textwidth]{mesh}
    \caption{a nice plot}
    `\label{fig:mesh``
\end{figure}

As you can see in the figure \ref{fig:mesh1}, the 
function grows near 0. Also, in the page \pageref{fig:mesh1} 
is the same example.
```
这里试图产生一个paper中的figure，使用`\begin{figure}[h], \end{figure}`表示image caption。`[h]`表示figure的position，`\caption{a nice plot}`表示对于这个figure的说明。`\label{fig:mesh1}`表示这个figure的变量名，之后可以通过其他方式reference到这个figure，例如下面的`\ref{fig:mesh1}`和`\pageref{fig:mesh1} `，分别输出figure 1和page 1。我猜latex的figure是自动编号12345的。

### list

```latex
\begin{itemize}
  \item The individual entries are indicated with a black dot, a so-called bullet.
  \item The text in the entries may be of any length.
\end{itemize}

\begin{enumerate}
  \item This is the first entry in our list
  \item The list numbers increase with each entry we add
\end{enumerate}
```
上面的第一个例子create了一个list，即黑色小点点。第二个例子也是list，但是数字编号。


### 公式 equations
```latex
In physics, the mass-energy equivalence is stated 
by the equation $E=mc^2$, discovered in 1905 by Albert Einstein.

The mass-energy equivalence is described by the famous equation
\[ E=mc^2 \]
discovered in 1905 by Albert Einstein. 
In natural units ($c = 1$), the formula expresses the identity
\begin{equation}
E=m
\end{equation}
```
使用`$E=mc^2$`，`$`中间的部分会被自动识别为公式。这是公式写在同一行的写法。`\[ E=mc^2 \]`是公式换行且居中的写法。注意第二种写法不会打断当前自然段。第三种`\begin{equation}` `\end{equation}`的写法调用了equation，即latex会自动在equation后面添加括号()，我猜和figure一样也是自动编号的。

```latex
Subscripts in math mode are written as $a_b$ and superscripts are written as $a^b$. These can be combined an nested to write expressions such as

\[ T^{i_1 i_2 \dots i_p}_{j_1 j_2 \dots j_q} = T(x^{i_1},\dots,x^{i_p},e_{j_1},\dots,e_{j_q}) \]
 
We write integrals using $\int$ and fractions using $\frac{a}{b}$. Limits are placed on integrals using superscripts and subscripts:

\[ \int_0^1 \frac{dx}{e^x} =  \frac{e-1}{e} \]

Lower case Greek letters are written as $\omega$ $\delta$ etc. while upper case Greek letters are written as $\Omega$ $\Delta$.

Mathematical operators are prefixed with a backslash as $\sin(\beta)$, $\cos(\alpha)$, $\log(x)$ etc.
```
这里告诉我们如何使用公式，`$a_b$`表示b是a的下标，而` $a^b$`表示b是a的上标，上标同时也可表示乘方。这里`\dots`在equation中编译为省略号。`$\int$`表示积分符号，积分符号同样适用于上下标。`$\frac{a}{b}$`表示a除以b。


