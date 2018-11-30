---

title: Tutorial for Latex
date: 2018-10-31 17:16:20
tags: [Markdown]
categories: [Tutorial]

---


<!-- vim-markdown-toc GFM -->

* [行内行间应用方式](#行内行间应用方式)
* [常用希腊字母](#常用希腊字母)
* [常用求和符号和积分号](#常用求和符号和积分号)
* [其他常用符号](#其他常用符号)
* [运算符](#运算符)
* [声调](#声调)
* [根号](#根号)
* [上标下标](#上标下标)
* [空白类型列举](#空白类型列举)
* [矩阵](#矩阵)
    * [简单的矩阵](#简单的矩阵)
    * [带括号的矩阵](#带括号的矩阵)
* [排版](#排版)
    * [多行方程组对齐](#多行方程组对齐)
    * [多行公式等号对齐](#多行公式等号对齐)
    * [大括号右多行赋值](#大括号右多行赋值)
* [字体](#字体)
* [括号](#括号)
* [颜色](#颜色)
* [特殊例子](#特殊例子)
    * [分子分母](#分子分母)
    * [在线使用](#在线使用)

<!-- vim-markdown-toc -->

[参考1](https://blog.csdn.net/qq_17528659/article/details/82152530)

[参考2](https://blog.csdn.net/kevinelstri/article/details/62419242)

[在线编辑](http://latex.codecogs.com/eqneditor/editor.php)

<!-- more -->

## 行内行间应用方式 

```
$ equation $  (正文)
$$ eqauation $$ (单独)
```

## 常用希腊字母
|||||
|:---:|:---:|:---:|:---:|
| \\alpha   | $\alpha$   | \\varTheta  | $\varTheta$  |
| \\beta    | $\beta$    | \\varGamma  | $\varGamma$  |
| \\gamma   | $\gamma$   | \\Gamma     | $\Gamma$     |
| \\theta   | $\theta$   | \\Theta     | $\Theta$     |
| \\delta   | $\delta$   | \\Delta     | $\Delta$     |
| \\epsilon | $\epsilon$ | \\varDelta  | $\varDelta$  |
| \\zeta    | $\zeta$    | \\varLambda | $\varLambda$ |
| \\eta     | $\eta$     |             |              |
| \\iota    | $\iota$    |             |              |
| \\kappa   | $\kappa$   | \\varLambda | $\varLambda$ |
| \\lambda  | $\lambda$  | \\Lambda    | $\Lambda$    |
| \\mu      | $\mu$      | \\varOmega  | $\varOmega$  |
| \\nu      | $\nu$      | \\varPi     | $\varPi$     |
| \\pi      | $\pi$      | \\Pi        | $\Pi$        |
| \\rho     | $\rho$     | \\varSigma  | $\varSigma$  |
| \\sigma   | $\sigma$   | \\Sigma     | $\Sigma$     |
| \\tau     | $\tau$     | \\varPhi    | $\varPhi$    |
| \\phi     | $\phi$     | \\Phi       | $\Phi$       |
| \\omega   | $\omega$   | \\Omega     | $\Omega$     |
| \\tau     | $\tau$     | \\Tau       | $\Tau$       |
| \\chi     | $\chi$     | | |
| \\upsilon | $\upsilon$ | | |
| \\iota    | $\iota$    | | |


## 常用求和符号和积分号

|||
|:---:|:---:|
| \\sum                | $\sum$
| \\int                | $\int$
| \\sum_{i=1}^{N}      | $\sum_{i=1}^{N}$
| \\int_{a}^{b}        | $\int_{a}^{b}$
| \\prod               | $\prod$
| \\iint               | $\iint$
| \\prod_{i=1}^{N}     | $\prod_{i=1}^{N}$
| \\iint_{a}^{b}       | $\iint_{a}^{b}$
| \\bigcup             | $\bigcup$
| \\bigcap             | $\bigcap$
| \\bigcup_{i=1}^{N}   | $\bigcup_{i=1}^{N}$
| \\bigcap_{i=1}^{N}   | $\bigcap_{i=1}^{N}$

## 其他常用符号

|||
|:---:|:---:|
| \\sqrt[3]{2}      | $\sqrt[3]{2}$
| \\sqrt{2}         | $\sqrt{2}$
| \\sqrt{a}         | $\sqrt{a}$
| \\vec{a}          | $\vec{a}$
| \\tilde{a}        | $\tilde{a}$
| \\sqrt[3]{2}      | $\sqrt[3]{2}$
| x_{3}             | $x_{3}$
| \\lim_{x \\to 0}  | $\lim_{x \to 0}$
| \\frac{1}{2}      | $\frac{1}{2}$
| \\infty           | $\infty$
| \\cdotp           | $\cdotp$
| \\cdots           | $\cdots$
| \\vdots           | $\vdots$
| \\ddots           | $\ddots$
| \\bot             | $\bot$
| \\partial         | $\partial$
| \\hat{a}          | $\hat{a}$
| \\dot{a}          | $\dot{a}$
| \\bar{a}          | $\bar{a}$
| a^{3}             | $a^{3}$
| \\frac{1}{a}      | $\frac{1}{a}$

## 运算符
|语法|效果|语法|效果|
|:---:|:---:|:---:|:---:|
| \\pm 	      | $\pm$ 	     | \\propto   | $\propto$    |           
| \\times 	  | $\times$	 | \\leq 	  | $\leq$	     |
| \\circ 	  | $\circ$	     | \\subseteq | $\subseteq$	 |
| \\cdot 	  | $\cdot$	     | \\subset   | $\subset$	 |
| \\cap 	  | $\cap$	     | \\cup 	  | $\cup$	     |
| \\supset    | $\supset$    | \\bullet   | $\bullet$    | 
| \\supseteq  | $\supseteq$  | \\div 	  | $\div$	     |
| \\geq 	  | $\geq$	     | \\mp       | $\mp$        |
| \\in 	      | $\in$	     |||

## 声调

|  语法 | 效果  |  语法 | 效果  |  语法 | 效果  |
| :---: | :---: | :---: | :---: | :---: | :---: |
| \\bar{x}       | $\bar{x}$      | \\acute{\\eta} | $\acute{\eta}$ | \\check{\\alpha} | $\check{\alpha}$
| \\grave{\\eta} | $\grave{\eta}$ | \\breve{a}     | $\breve{a}$    | \\ddot{y}        | $\ddot{y}$
| \\dot{x}       | $\dot{x}$      | \\hat{\\alpha} | $\hat{\alpha}$ | \\tilde{\\iota}  | $\tilde{\iota}$


## 根号

|  语法 | 效果  |  语法 | 效果  |
| :---: | :---: | :---: | :---: |
| \\sqrt{3} | $\sqrt{3}$ | \\sqrt[n]{3} | $\sqrt[n]{3}$


## 上标下标

|  功能 | 语法 |  效果 |
| :---: | :---: | :---: |
| 上标 | a^2     | $a^2$
| 下标 | a\_2    | $a_2$
| 组合 | a^{2+2} | $a^{2+2}$
| 组合 | a_{i,j} | $a_{i,j}$
| 结合 | x\_2^3  | $x_2^3$
| 前置 | {}_1\^2\!X_3\^4 | ${}_1^2\!X_3^4$
| 向量 | \\vec{c} | $\vec{c}$
| 向量 | \\overleftarrow{a b} | $\overleftarrow{a b}$
| 向量 | \\overrightarrow{c d} | $\overrightarrow{c d}$
| 导数 | x' | $x'$
| 导数 | x^\\prime | $x^\prime$
| 导数 | \\dot{x} | $\dot{x}$
| 导数 | \\ddot{y} | $\ddot{y}$
| 上划线 | \\overline{h i j} | $\overline{h i j}$
| 下划线 | \\underline{k l m} | $\underline{k l m}$
| 上面 | \\overset{P}{\\longrightarrow} | $\overset{P}{\longrightarrow}$
| 下面 | \\underset{P}{\\Longrightarrow} | $\underset{P}{\Longrightarrow}$



- 默认行内公式`$\sum_{k=1}^n{x_k}$`上下标: $\sum_{k=1}^n{x_k}$ 
- 默认行间公式`$$\sum_{k=1}^n{x_k}$$`上下标: $$\sum_{k=1}^n{x_k}$$

- 强制行内公式`$\sum\limits_{k=1}^n{x_k}$`上下标: $\sum\limits_{k=1}^n{x_k}$
- 强制行间公式`$$\sum\nolimits_{k=1}^n{x_k}$$`上下标: $$\sum\nolimits_{k=1}^n{x_k}$$

## 空白类型列举

| 功能 | 语法 | 效果 | 描述 |
|:---:|:---:|:---:|:---:|
| 两个空格 |  a \\qquad b | $a \qquad b$ | 两个m的宽度  |
| 一个空格 |  a \\quad b  | $a \quad b$  | 一个m的宽度  |
| 大空格   |  a\\ b       | $a\ b$       | 1/3m宽度     |
| 中等空格 |  a\\;b       | $a\;b$       | 2/7m宽度     |
| 小空格   |  a,b         | $a,b$        | 1/6m宽度     |
| 没有空格 |  ab          | $ab$         | 正常宽度     |
| 紧贴     |  a!b         | $a!b$        | 缩进1/6m宽度 |

## 矩阵

### 简单的矩阵
    $$
    \begin{matrix}
        1 & 2 & 3 \\
        4 & 5 & 6 \\
        7 & 8 & 9
    \end{matrix}  \tag{3}
    $$

$$
\begin{matrix}
	1 & 2 & 3 \\
	4 & 5 & 6 \\
	7 & 8 & 9
\end{matrix}  \tag{3}
$$

### 带括号的矩阵

    $$
    \left \{
        \begin{matrix}
            1 & 2 & 3 \\
            4 & 5 & 6 \\
            7 & 8 & 9
        \end{matrix}
    \right \} \tag{4}
    $$

$$
\left \{
	\begin{matrix}
		1 & 2 & 3 \\
		4 & 5 & 6 \\
		7 & 8 & 9
	\end{matrix}
\right \} \tag{4}
$$


    $$
    \left [
        \begin{matrix}
            1 & 2 & 3 \\
            4 & 5 & 6 \\
            7 & 8 & 9
        \end{matrix}
    \right ] \tag{5}
    $$

$$
\left[
	\begin{matrix}
		1 & 2 & 3 \\
		4 & 5 & 6 \\
		7 & 8 & 9
	\end{matrix}
\right] \tag{5}
$$


    $$
    \begin{bmatrix}
        1 & 2 & 3 \\
        4 & 5 & 6 \\
        7 & 8 & 9
    \end{bmatrix} \tag{6}
    $$

$$
\begin{bmatrix}
    1 & 2 & 3 \\
    4 & 5 & 6 \\
    7 & 8 & 9
\end{bmatrix} \tag{6}
$$


    $$
    \begin{Bmatrix}
        1 & 2 & 3 \\
        4 & 5 & 6 \\
        7 & 8 & 9
    \end{Bmatrix} \tag{7}
    $$

$$
\begin{Bmatrix}
    1 & 2 & 3 \\
    4 & 5 & 6 \\
    7 & 8 & 9
\end{Bmatrix} \tag{7}
$$


    $$
    \begin{vmatrix}
        1 & 2 & 3 \\
        4 & 5 & 6 \\
        7 & 8 & 9
    \end{vmatrix} \tag{8}
    $$

$$
\begin{vmatrix}
    1 & 2 & 3 \\
    4 & 5 & 6 \\
    7 & 8 & 9
\end{vmatrix} \tag{8}
$$


    $$
    \begin{Vmatrix}
        1 & 2 & 3 \\
        4 & 5 & 6 \\
        7 & 8 & 9
    \end{Vmatrix} \tag{9}
    $$

$$
\begin{Vmatrix}
    1 & 2 & 3 \\
    4 & 5 & 6 \\
    7 & 8 & 9
\end{Vmatrix} \tag{9}
$$

## 排版

### 多行方程组对齐

    $$ 
    \begin{cases} 
        a_{11}x_1&+&a_{12}x_2&+&\cdots&+a_{1n}x_n&=&b_1\\
        &&&&\vdots\\
        a_{n1}x_1&+&a_{n2}x_2&+&\cdots&+a_{nn}x_n&=&b_n&
    \end{cases}
    $$

$$ 
\begin{cases} 
    a_{11}x_1&+&a_{12}x_2&+&\cdots&+a_{1n}x_n&=&b_1\\
    &&&&\vdots\\
    a_{n1}x_1&+&a_{n2}x_2&+&\cdots&+a_{nn}x_n&=&b_n&
\end{cases}
$$

    $$ 
    \begin{align*} 
    　　&a_1 x + b_1 y + c_1 z = d_1\tag{$3.11$}  \\ 
    　　&aa_2 x + b_2 y + c_2 z = d_2\tag{$3.12$}  \\
    　　&aaa_3 x + b_3 y + c_3 z = d_3\tag{$3.13$}
    \end{align*}
    $$

$$ 
\begin{align*} 
　　&a_1 x + b_1 y + c_1 z = d_1\tag{$3.11$}  \\ 
　　&aa_2 x + b_2 y + c_2 z = d_2\tag{$3.12$}  \\
　　&aaa_3 x + b_3 y + c_3 z = d_3\tag{$3.13$}
\end{align*}
$$

### 多行公式等号对齐

eqnarray(built-in) is similar to align(amsmath package) below

    $$
    \begin{eqnarray}
        f(x,y) &=& 2xy+(x-y)^2\\
               &=& x^2+y^2
    \end{eqnarray}
    $$

$$
\begin{eqnarray}
    f(x,y) &=& 2xy+(x-y)^2\\
           &=& x^2+y^2
\end{eqnarray}
$$

    $$
    \begin{align*}
        f(x,y) &= 2xy+(x-y)^2 \\
               &= x^2+y^2
    \end{align*}
    $$

$$
\begin{align*}
    f(x,y) &= 2xy+(x-y)^2 \\
           &= x^2+y^2
\end{align*}
$$

指定第几列对齐

    $$
    \begin{alignat}{2} 
        \sigma_1 &= x + y &\qquad \sigma_2 &= \frac{x}{y} \\
        \sigma_1' &= \frac{\partial x + y}{\partial x} & \sigma_2' &= \frac{\partial \frac{x}{y}}{\partial x}
    \end{alignat}
    $$

$$
\begin{alignat}{2} 
    \sigma_1 &= x + y &\quad \sigma_2 &= \frac{x}{y} \\
    \sigma_1' &= \frac{\partial x + y}{\partial x} & \sigma_2' &= \frac{\partial \frac{x}{y}}{\partial x}
\end{alignat}
$$


### 大括号右多行赋值

    $$
    \left\{\begin{array}{cc} 
            1, & x=f(Pa_{x})\\ 
            0, & other\ values 
    \end{array}\right.
    $$

$$
\left\{\begin{array}{cc} 
		1, & x=f(Pa_{x})\\ 
		0, & other\ values 
\end{array}\right.
$$

    $$
    P(x|Pa_x)=\begin{cases} 
            1, & x=f(Pa_{x})\\ 
            0, & other\ values 
    \end{cases}
    $$

$$
P(x|Pa_x)=\begin{cases} 
		1, & x=f(Pa_{x})\\ 
		0, & other\ values 
\end{cases}
$$

    $$
    \begin{equation}
        \left.\begin{aligned}
                B'&=-\partial \times E,\\
                E'&=\partial \times B - 4\pi j,
               \end{aligned}
        \right\}
        \qquad \text{Maxwell's equations}
    \end{equation}
    $$

$$
\begin{equation}
    \left.\begin{aligned}
            B'&=-\partial \times E,\\
            E'&=\partial \times B - 4\pi j,
           \end{aligned}
    \right\}
    \qquad \text{Maxwell's equations}
\end{equation}
$$


## 字体

- 黑板粗体`$\mathbb{ABCDEFGHIJKLMNOPQRSTUVWXYZ}$`: $\mathbb{ABCDEFGHIJKLMNOPQRSTUVWXYZ}$
- 正粗体`$\mathbf{012abcABC}$`: $\mathbf{012abcABC}$
- 罗马体`$\mathrm{012abcABC}$`: $\mathrm{012abcABC}$
- 斜体数字`$\mathit{0123456789}$`: $\mathit{0123456789}$
- 手写体 `$\mathcal{012abcABC}$`: $\mathcal{012abcABC}$

## 括号

`\Bigg ( \bigg [ \Big \{ \big \langle \left | \| \frac{a}{b} \| \right | \big\rangle \Big \} \bigg ] \Bigg )`:

$$
\Bigg ( \bigg [ \Big \{ \big \langle \left | \| \frac{a}{b} \| \right | \big\rangle \Big \} \bigg ] \Bigg )
$$

## 颜色

- 字体颜色︰`$\color{色调}表达式$`
- 背景颜色︰`$\pagecolor{色调}表达式$`


|||||
|---|---|---|---|
| $\color{Apricot}Apricot$	               | $\color{Aquamarine}Aquamarine$          | $\color{Bittersweet}Bittersweet$	 | $\color{Black}Black$
| $\color{Blue}Blue$    	               | $\color{BlueGreen}BlueGreen$   	     | $\color{BlueViolet}BlueViolet$	 | $\color{BrickRed}BrickRed$
| $\color{Brown}Brown$   	               | $\color{BurntOrange}BurntOrange$ 	     | $\color{CadetBlue}CadetBlue$	     | $\color{CarnationPink}CarnationPink$
| $\color{Cerulean}Cerulean$	   	       | $\color{CornflowerBlue}CornflowerBlue$  | $\color{Cyan}Cyan$	             | $\color{Dandelion}Dandelion$
| $\color{DarkOrchid}DarkOrchid$           | $\color{Emerald}Emerald$	             | $\color{ForestGreen}ForestGreen$	 | $\color{Fuchsia}Fuchsia$
| $\color{Goldenrod}Goldenrod$	           | $\color{Gray}Gray$	                     | $\color{Green}Green$              | $\color{GreenYellow}GreenYellow$
| $\color{JungleGreen}JungleGreen$         | $\color{Lavender}Lavender$              | $\color{LimeGreen}LimeGreen$	     | $\color{Magenta}Magenta$
| $\color{Mahogany}Mahogany$	           | $\color{Maroon}Maroon$	                 | $\color{Melon}Melon$	             | $\color{MidnightBlue}MidnightBlue$
| $\color{Mulberry}Mulberry$	           | $\color{NavyBlue}NavyBlue$ 	         | $\color{OliveGreen}OliveGreen$	 | $\color{Orange}Orange$
| $\color{OrangeRed}OrangeRed$	           | $\color{Orchid}Orchid$	                 | $\color{Peach}Peach$	             | $\color{Periwinkle}Periwinkle$
| $\color{PineGreen}PineGreen$	           | $\color{Plum}Plum$	                     | $\color{ProcessBlue}ProcessBlue$	 | $\color{Purple}Purple$
| $\color{RawSienna}RawSienna$	           | $\color{Red}Red$	                     | $\color{RedOrange}RedOrange$	     | $\color{RedViolet}RedViolet$
| $\color{Rhodamine}Rhodamine$	           | $\color{RoyalBlue}RoyalBlue$            | $\color{RoyalPurple}RoyalPurple$	 | $\color{RubineRed}RubineRed$
| $\color{Salmon}Salmon$	               | $\color{SeaGreen}SeaGreen$ 	         | $\color{Sepia}Sepia$	             | $\color{SkyBlue}SkyBlue$
| $\color{SpringGreen}SpringGreen$         | $\color{Tan}Tan$                        | $\color{TealBlue}TealBlue$	     | $\color{Thistle}Thistle$
| $\color{Turquoise}Turquoise$	           | $\color{Violet}Violet$	                 | $\color{VioletRed}VioletRed$      | $\color{White}White$
| $\color{WildStrawberry}WildStrawberry$   | $\color{Yellow}Yellow$                  | $\color{YellowGreen}YellowGreen$  | $\color{YellowOrange}YellowOrange$

## 特殊例子

### 分子分母

    $$
    x_1^*=\dfrac{a_{22}r_1-a_{12}r_2}{a_{11}a_{22}-a_{12}a_{21}}
    $$

$$
x_1^*=\dfrac{a_{22}r_1-a_{12}r_2}{a_{11}a_{22}-a_{12}a_{21}}
$$


|语法|效果|语法|效果|语法|效果|
|:------:|:---:|:------:|:---:|:---:|:---:|
| \\frac{a}{b} | $\frac{a}{b}$ | \\dfrac{c}{d} | $\dfrac{c}{d}$ | e/f | $e/f$ |


### 分布

|分布名|语法|效果|例子|
|:---:|:---:|:---:|:------|
|二项分布| `\binom{n}{k}` | $\binom{n}{k}$ | $f(k) = \binom{n}{k} p^k (1-p)^{n-k}$ |



### 在线使用

只能行间

`![](http://latex.codecogs.com/gif.latex?F%28x%29%20%3D%20%5Cint_%7Ba%7D%5E%7Bb%7Df%28x%29dx "latextest")`

输出:

![](http://latex.codecogs.com/gif.latex?F%28x%29%20%3D%20%5Cint_%7Ba%7D%5E%7Bb%7Df%28x%29dx "latextest")
