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
documentclass说明了这个文件的overall view，"article"是最通用的文件类型，其他的还有"book","report","beamer"等等。在\begin{document}和\end{document}之间的为document的主体，类似html的boty。

```latex
\documentclass[12pt, letterpaper]{article}
\usepackage[utf8]{inputenc}
```
写在
