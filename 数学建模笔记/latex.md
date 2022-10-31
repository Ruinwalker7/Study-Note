# LateX笔记

- 中文

```latex
\usepackage[utf8]{ctex}  %编译器要改
```

- 页边距

```latex
\usepackage[a4paper,left=30mm,right=30mm,top=30mm,bottom=30mm]{geometry}
```

- 图片

```latex
\usepackage{graphicx}
\graphicspath{{image/}}%新建文件夹后放到这个位置
```



```latex
\begin{figure}[h]
    \centering %居中
    \includegraphics[width=0.75\textwidth]{mesh} %大小变成原图片75%
    \caption{A nice plot.} %图片名称
    \label{fig:mesh1} %标签
\end{figure}
```



列表

```latex
\begin{itemize}
\end{itemize}

\begin{enumerate}
\end{enumerate}   %有序列表

\renewcommand{\labelenumii}{\arabic{enumi}.\arabic{enumii}}
\renewcommand{\labelenumiii}{\arabic{enumi}.\arabic{enumii}.\arabic{enumiii}}
\renewcommand{\labelenumiv}
{\arabic{enumi}.\arabic{enumii}.\arabic{enumiii}.\arabic{enumiv}} %重设列表

enumitem package %需要强大功能ne'g
```



段落

两个enter

```latex
\newline
\setlength{\parindent}{20pt} %设置段落距离
\noindent%无距离
```

不要使用多个\newline来模拟较大的段间距

### 数学公式

平方         $x^2$

下标         $y_1$

无穷         $\infty$

除号         $a\div{b}$

加减号     $a\pm{b}$

分数         $\frac{a}{b}$

根号         $\sqrt{a}$

...               $\dots$

sin             $sin{\theta}$

cos            $\cos{\theta}$

tan            $\tan{\theta}$

cot             $\cot{\theta}$



$\sum_{n=1}^{20} n^{2}$









希腊字母表

