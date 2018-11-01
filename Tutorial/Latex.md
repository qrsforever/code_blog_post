---

title: Tutorial for Latex
date: 2018-10-31 17:16:20
tags: [Markdown]
categories: [Tutorial]

---


<!-- vim-markdown-toc GFM -->

* [显示](#显示)
* [常用希腊字母](#常用希腊字母)
* [常用求和符号和积分号](#常用求和符号和积分号)
* [其他常用符号](#其他常用符号)
* [声调](#声调)
* [根号](#根号)
* [上标下标](#上标下标)

<!-- vim-markdown-toc -->

[参考1](https://blog.csdn.net/qq_17528659/article/details/82152530)

[参考2](https://blog.csdn.net/kevinelstri/article/details/62419242)

<!-- more -->

## 显示

```
$...$  (正文)
$$..$$ (单独)
```

## 常用希腊字母
||||
| :-: | :-: | :-: |
\\alpha   | $\alpha$   | α
\\beta    | $\beta$    | β
\\gamma   | $\gamma$   | γ
\\theta   | $\theta$   | θ
\\delta   | $\delta$   | δ
\\epsilon | $\epsilon$ | ϵ
\\zeta    | $\zeta$    | ζ
\\eta     | $\eta$     | η
\\iota    | $\iota$    | ι
\\kappa   | $\kappa$   | κ
\\lambda  | $\lambda$  | λ
\\mu      | $\mu$      | μ
\\nu      | $\nu$      | ν
\\pi      | $\pi$      | π
\\rho     | $\rho$     | ρ
\\sigma   | $\sigma$   | σ
\\tau     | $\tau$     | τ
\\phi     | $\phi$     | ϕ
\\omega   | $\omega$   | ω


## 常用求和符号和积分号

|||
| :-: | :-: |
\\sum                | $\sum$
\\int                | $\int$
\\sum_{i=1}^{N}      | $\sum_{i=1}^{N}$
\\int_{a}^{b}        | $\int_{a}^{b}$
\\prod               | $\prod$
\\iint               | $\iint$
\\prod_{i=1}^{N}     | $\prod_{i=1}^{N}$
\\iint_{a}^{b}       | $\iint_{a}^{b}$
\\bigcup             | $\bigcup$
\\bigcap             | $\bigcap$
\\bigcup_{i=1}^{N}   | $\bigcup_{i=1}^{N}$
\\bigcap_{i=1}^{N}   | $\bigcap_{i=1}^{N}$

## 其他常用符号

|||
| :-: | :-: |
\\sqrt[3]{2}    | $\sqrt[3]{2}$
\\sqrt{2}       | $\sqrt{2}$
x_{3}           | $x_{3}$
\\lim_{x \to 0} | $\lim_{x \to 0}$
\\frac{1}{2}    | $\frac{1}{2}$
\\cdotp         | $\cdotp$
\\infty         | $\infty$
\\cdots         | $\cdots$
\\bot           | $\bot$
\\ddots         | $\ddots$
\\partial       | $\partial$
\\hat{a}        | $\hat{a}$
\\dot{a}        | $\dot{a}$
\\bar{a}        | $\bar{a}$
a^{3}           | $a^{3}$
\\sqrt{a}       | $\sqrt{a}$
\\vec{a}        | $\vec{a}$
\\tilde{a}      | $\tilde{a}$
\\lim_{x \to 0} | $\lim_{x \to 0}$
\\sqrt[3]{2}    | $\sqrt[3]{2}$
\\frac{1}{a}    | $\frac{1}{a}$



## 声调

|  语法 | 效果  |  语法 | 效果  |  语法 | 效果  |
| :---: | :---: | :---: | :---: | :---: | :---: |
\\bar{x}       | $\bar{x}$      | \\acute{\\eta} | $\acute{\eta}$ | \\check{\\alpha} | $\check{\alpha}$
\\grave{\\eta} | $\grave{\eta}$ | \\breve{a}     | $\breve{a}$    | \\ddot{y}        | $\ddot{y}$
\\dot{x}       | $\dot{x}$      | \\hat{\\alpha} | $\hat{\alpha}$ | \\tilde{\\iota}  | $\tilde{\iota}$


## 根号

|  语法 | 效果  |  语法 | 效果  |
| :---: | :---: | :---: | :---: |
\\sqrt{3} | $\sqrt{3}$ | \\sqrt[n]{3} | $\sqrt[n]{3}$


## 上标下标

|  功能 | 语法 |  效果 |
| :---: | :---: | :---: |
上标 | a^2     | $a^2$
下标 | a_2     | $a_2$
组合 | a^{2+2} | $a^{2+2}$
组合 | a_{i,j} | $a_{i,j}$
结合 | x_2^3   | $x_2^3$
前置 | {}_1^2\!X_3^4 | ${}_1^2\!X_3^4$
