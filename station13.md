[Index](/)

---

## Station #13

[TOC]

### History

- [x] `20250218` [轮换式（加强版）](https://www.luogu.com.cn/problem/P6296) [sol](/DT/station13/20250218.html) ps:弱化版只是数据范围与模数不同。

### Daily training

小奔发现，对于任意的 $n$ 个字母，他们构成的轮换式，都表示成 $n$ 个基本轮换式的线性和。

一元的基本轮换式：$a$；

二元：$a+b$，$ab$；

三元：$a+b+c$，$ab+ac+bc$，$abc$；

四元：$a+b+c+d$，$ab+ac+ad+bc+bd+cd$，$abc+abd+bcd$，$abcd$；

......

已知 $n$ 个数的各个基本轮换式的值，求它们的 $m$ 次方和，答案对 $899678209$（$899678209 = 429 \times 2^{21} + 1$）取模。

- 对于 $20\%$ 的数据，$1\le n \le 1000$，$1\le m \le 10^4$；  
- 对于 $60\%$ 的数据，$1\le n \le 1000$，$1\le m \le 10^9$；  
- 对于 $100\%$ 的数据，$1\le n \le 3 \times 10^4$，$1\le m \le 10^9$，$1\le a_i \le 10^8$。

### Daily paper

`更新时间：20250218`

[20250218比赛相关](/FileLink/public/index.html)
