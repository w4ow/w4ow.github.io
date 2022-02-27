---
title: Latex常见数学符号整理
date: 2019-09-22 11:00
mathjax: true
meta:
    date: true
categories: 
    - Computer Science
    - Tools
tags:
    - Tex
---

LaTeX常见数学符号整理

---

<!-- more -->

## 1. 数学模式重音符

Show|Code| | | | | | |
-|-|-|-|-|-|-|-
$\hat{a}$ | \hat{a} | $\check{a}$ | \check{a} | $\tilde{a}$ | \tilde{a} | $\acute{a}$ | \acute{a}
$\grave{a}$ | \grave{a} | $\dot{a}$ | \dot{a} | $\ddot{a}$ | \ddot{a} | $\breve{a}$ | \breve{a}
$\bar{a}$ | \bar{a} | $\vec{a}$ | \vec{a} 

## 2. 小写希腊字母

Show|Code| | | | | | |
-|-|-|-|-|-|-|-
$\alpha$ | \alpha | $\theta$ | \theta | $o$ | o | $\upsilon$ | \upsilon
$\beta$ | \beta | $\vartheta$ | \vartheta | $\pi$ | \pi | $\phi$ | \phi
$\gamma$ | \gamma | $\iota$ | \iota | $\varpi$ | \varpi | $\varphi$ | \varphi
$\delta$ | \delta | $\kappa$ | \kappa | $\rho$ | \rho | $\chi$ | \chi
$\epsilon$ | \epsilon | $\lambda$ | \lambda | $\varrho$ | \varrho | $\psi$ | \psi
$\varepsilon$ | \varepsilon | $\mu$ | \mu | $\sigma$ | \sigma | $\omega$ | \omega
$\zeta$ | \zeta | $\nu$ | \nu | $\varsigma$ | \varsigma |
$\eta$ | \eta | $\xi$ | \xi | $\tau$ | \tau |

## 3. 大写希腊字母

Show|Code| | | | | | |
-|-|-|-|-|-|-|-
$\Gamma$ | \Gamma | $\Lambda$ | \Lambda | $\Sigma$ | \Sigma | $\Psi$ | \Psi
$\Delta$ | \Delta | $\Xi$ | \Xi | $\Upsilon$ | \Upsilon | $\Omega$ | \Omega
$\Theta$ | \Theta | $\Pi$ | \Pi | $\Phi$ | \Phi

## 4. 二元关系符

该表中的所有符号都可以通过在代码前加上**\not**来得到其否定形式。

Show|Code| | | | |
-|-|-|-|-|-
$<$ | < | $>$ | > | $=$ | =
$\le$ | \le | $\ge$ | \ge | $\equiv$ | \equiv
$\ll$ | \ll | $\gg$ | \gg | $\doteq$ | \doteq
$\sim$ | \sim | $\simeq$ | \simeq | $\approx$ | \approx
$\subset$ | \subset | $\supset$ | \supset | $\cong$ | \cong
$\subseteq$ | \subseteq | $\supseteq$ | \supseteq | $\Join$ | \Join$^a$
$\in$ | \in | $\ni$ | \ni | $\propto$ | propto
$\mid$ | \mid | $\parallel$ | \parallel | $:$ | :

$^a$使用宏包latexsym来得到这个符号

## 5. 二元运算符

Show|Code| | | | |
-|-|-|-|-|-
$+$ | + | $-$ | - | $\cdot$ | \cdot
$\pm$ | \pm | $\mp$ | \mp | $\div$ | \div
$\times$ | \times | $/$ | / | $\setminus$ | \setminus
$\star$ | \star | $\ast$ | \ast | $\circ$ | \circ
$\cup$ | \cup | $\cap$ | \cap | $\bullet$ | \bullet
$\lor$ | \lor | $\land$ | \land | $\oplus$ | \oplus
$\odot$ | \odot | $\boxplus$ | \boxplus

## 6. 大尺度运算符

Show|Code| | | | |
-|-|-|-|-|-
$\sum_{1}^{2}$ | \sum_{1}^{2} | $\prod_{1}^{2}$ | \prod_{1}^{2} | $\int_{1}^{2}$ | \int_{1}^{2}

## 7. 箭头

Show|Code| | | | |
-|-|-|-|-|-
$\gets$ | \gets | $\to$ | \to | $\leftrightarrow$ | \leftrightarrow
$\Leftarrow$ | \Leftarrow | $\Rightarrow$ | \Rightarrow | $\Leftrightarrow$ | \Leftrightarrow
$\mapsto$ | \mapsto |

## 8. 定界符

Show|Code| | | | | | |
-|-|-|-|-|-|-|-
$($ | ( | $)$ | ) | $\left( \right)$ | \left( \right) | $\lbrace \rbrace$ | \{ \} or \lbrace \rbrace  

## 9. 其他符号

Show|Code| | | | | | |
-|-|-|-|-|-|-|-
$...$ | ... | $\cdots$ | \cdots | $\vdots$ | \vdots | $\ddots$ | \ddots
$'$ | ' | $\prime$ | \prime | $\triangle$ | \triangle | $\infty$ | \infty
$\bot$ | \bot | $\top$ | \top | $\angle$ | \angle | $\surd$ | \surd
$\nabla$ | \nabla | $\heartsuit$ | \heartsuit | $\clubsuit$ | \clubsuit | $\spadesuit$ | \spadesuit
$\neg$ | \neg or \lnot | $\exists$ | \exists | $\emptyset$ | \emptyset | $\partial$ | \partial
$\forall$ | \forall

## 10. 各种帽子

Show|Code| | |
-|-|-|-
$\hat{A}$ | \hat{A} | $\widehat{A}$ | \widehat{A}
$\tilde{A}$ | \tilde{A} | $\widetilde{A}$ | \widetilde{A}
$\overline{A}$ | \overline{A} | $\underline{A}$ | \underline{A}
$\overbrace{A}$ | \overbrace{A} | $\underbrace{A}$ | \underbrace{A}
$\overset{b}{A}$ | \overset{b}{A} | $\underset{b}{A}$ | \underset{b}{A}
$\overleftarrow{A}$ | \overleftarrow{A} | $\overrightarrow{A}$ | \overrightarrow{A}

## 11. 字符样式

字样|命令|宏包
-|-|-
$\mathrm{ABCabc}$ | \mathrm{ABCabc} |
$\mathit{ABCabc}$ | \mathit{ABCabc} |
$\mathcal{ABCabc}$ | \mathcal{ABCabc} |
$\mathfrak{ABCabc}$ | \mathfrak{ABCabc} | eufrak
$\mathbb{ABCabc}$ | \mathbb{ABCabc} | amsfonts or asmsymb
