---
title: LaTex常用字母和数学公式
description: >-
  各位研究僧在写论文的时候不可避免地使用LaTex来搞定排版和美化的问题。
  下面整理了一份常用的希腊字母和数学公式常用的语法。
  
author: Retr0
date: 2023-02-15 16:00:00 +0900
categories: [Math, Writing]
tags: [LaTex]
pin: true
math: true
media_subpath: '/posts/20230215'
---

## **1. 希腊字母**

### 1.1 小写

| 字母 | LaTeX 写法      |
| -- | ------------- |
| α  | `\alpha`      |
| β  | `\beta`       |
| γ  | `\gamma`      |
| δ  | `\delta`      |
| ε  | `\epsilon`    |
| ϵ  | `\varepsilon` |
| ζ  | `\zeta`       |
| η  | `\eta`        |
| θ  | `\theta`      |
| ϑ  | `\vartheta`   |
| ι  | `\iota`       |
| κ  | `\kappa`      |
| λ  | `\lambda`     |
| μ  | `\mu`         |
| ν  | `\nu`         |
| ξ  | `\xi`         |
| π  | `\pi`         |
| ρ  | `\rho`        |
| ϱ  | `\varrho`     |
| σ  | `\sigma`      |
| ς  | `\varsigma`   |
| τ  | `\tau`        |
| υ  | `\upsilon`    |
| φ  | `\phi`        |
| ϕ  | `\varphi`     |
| χ  | `\chi`        |
| ψ  | `\psi`        |
| ω  | `\omega`      |

### 1.2 大写

`Note：并不是所有大写字母都有专门命令，只有长得和拉丁字母不同的才有`

| 字母 | LaTeX 写法   |
| -- | ---------- |
| Γ  | `\Gamma`   |
| Δ  | `\Delta`   |
| Θ  | `\Theta`   |
| Λ  | `\Lambda`  |
| Ξ  | `\Xi`      |
| Π  | `\Pi`      |
| Σ  | `\Sigma`   |
| Υ  | `\Upsilon` |
| Φ  | `\Phi`     |
| Ψ  | `\Psi`     |
| Ω  | `\Omega`   |

## **2. 数学公式**
`Note: 在写公式的时候需要在前后两端加上 $$ `

### 2.1 二次项
$$ x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a} $$

`x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}`

---

### 2.2 完全平方
$$ (a+b)^2=a^2+2ab+b^2 $$

`(a+b)^2=a^2+2ab+b^`

---

### 2.3 平方差
$$ a^2-b^2=(a-b)(a+b) $$

`a^2-b^2=(a-b)(a+b)`

---

### 2.4 等比数列求和
$$ S_n=a\frac{1-r^n}{1-r} $$

`S_n=a\frac{1-r^n}{1-r}`

---

### 2.5 指数、对数

$$ a^m a^n = a^{m+n} $$

`a^m a^n = a^{m+n}`

$$ \log_a b=\frac{\ln b}{\ln a} $$

`\log_a b=\frac{\ln b}{\ln a}`

---

### 2.6 欧拉公式
$$ e^{i\theta}=\cos\theta+i\sin\theta $$

`e^{i\theta}=\cos\theta+i\sin\theta`

---

### 2.7 三角函数

$$ \sin^2 x+\cos^2 x=1 $$

`\sin^2 x+\cos^2 x=1` 

$$ \sin(a+b)=\sin a\cos b+\cos a\sin b $$

`\sin(a+b)=\sin a\cos b+\cos a\sin b`

---

### 2.8 微积分

$$ f'(x)=\lim_{h\to 0}\frac{f(x+h)-f(x)}{h} $$

`f'(x)=\lim_{h\to 0}\frac{f(x+h)-f(x)}{h}`


$$ 
\frac{d}{dx}x^n=nx^{n-1},\quad
\frac{d}{dx}e^x=e^x,\quad
\frac{d}{dx}\ln x=\frac{1}{x}
$$

`\frac{d}{dx}x^n=nx^{n-1},\quad
\frac{d}{dx}e^x=e^x,\quad
\frac{d}{dx}\ln x=\frac{1}{x}`


$$ \int_a^b f(x)\,dx $$

`\int_a^b f(x)\,dx`


$$ \int u\,dv=uv-\int v\,du $$

`\int u\,dv=uv-\int v\,du`

---

### 2.9 线性代数

$$
A=
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
$$

`A=
\begin{bmatrix}
a & b 
c & d
\end{bmatrix}`


$$
\det(A-\lambda I)=0
$$

`\det(A-\lambda I)=0`

---

### 2.10 概率统计

$$
E[X],\quad
\mathrm{Var}(X)=E[(X-\mu)^2]
$$

`E[X],\quad
\mathrm{Var}(X)=E[(X-\mu)^2]`


$$
f(x)=\frac{1}{\sqrt{2\pi\sigma^2}}
\exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
$$

`f(x)=\frac{1}{\sqrt{2\pi\sigma^2}}
\exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)`

---

### 2.11 级数与展开

$$
\sum_{n=0}^{\infty} r^n=\frac{1}{1-r}\quad(|r|<1)
$$

`\sum_{n=0}^{\infty} r^n=\frac{1}{1-r}\quad(|r|<1)`


$$
f(x)=\sum_{n=0}^{\infty}\frac{f^{(n)}(a)}{n!}(x-a)^n
$$

`f(x)=\sum_{n=0}^{\infty}\frac{f^{(n)}(a)}{n!}(x-a)^n`




