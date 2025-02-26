## ARC111F Do you like query problems?

恶臭推导，不过思路很简单。

---

直接摊一下贡献。

考虑对一个点有效的（即 $l \leq i \leq r$）的若干次操作（指前两个操作）。

直接对于所有操作，写出答案之和（方案乘数字）：$i(n-i+1)(\frac{m(m-1)}{2}+mj)$。

假设 $f_j$ 表示经过前面的操作答案变为 $j$ 的方案数，一次操作即 $\sum f_j j \mapsto \sum f_ji(n-i+1)(\frac{m(m-1)}{2}+mj)$。

假设经过 $j$ 次操作方案数为 $a_{i,j}$，答案为 $b_{i,j}$。则：
$$
a_{i,j}=a_{i,j-1}i(n-i+1)2m=(i(n-i+1)2m)^{j}\\
b_{i,j} = \frac{i(n-i+1)m(m-1)}{2}a_{i,j-1}+i(n-i+1)mb_{i,j-1}\\
=lk_1(i)(2lk_0(i))^{j-1}+lk_0(i)b_{i,j-1}
$$
不妨记 $lk_0(i)=i(n-i+1)m$、$lk_1(i)=\frac{i(n-i+1)m(m-1)}{2}$。

如果我们将 $b_i$ 挂在 GF 上，则 $B=x(\frac{lk_1(i)}{1-2lk_0(i)x}+lk_0(i)B) \Rightarrow B=\frac{xlk_1(i)}{(1-2lk_0(i)x)(1-lk_0(i)x)}=\frac{\frac{lk_1(i)}{lk_0(i)}}{(1-2lk_0(i)x)}-\frac{\frac{lk_1(i)}{lk_0(i)}}{(1-lk_0(i)x)}$。

直接记 $lk_2(i)=\frac{lk_1(i)}{lk_0(i)}$，则 $b_{i,j}=lk_2(i)lk_0^j(i)(2^j-1)$。

我们可以通过枚举贡献位置 $i$，贡献时间 $j$，有关操作 $k$ 来描述答案：
$$
lk_3(i)=n(n+1)m-2i(n-i+1)m+\frac{n(n+1)}{2}\\
lk_4(i)=\frac{n(n+1)(2m+1)}{2}\\
ans=\sum_{1 \leq i \leq n} i(n-i+1)\sum_{0 \leq j < q}\sum_{0 \leq k \leq j}\binom{j}{k}b_{i,k}lk_3^{j-k}(i)lk_4^{q-1-j}(i)\\
ans=\sum_{1 \leq i \leq n} i(n-i+1)lk_2(i)lk_4^{q-1}(i)\sum_{0 \leq j < q}lk_4^{-j}(i)\sum_{0 \leq k \leq j}\binom{j}{k}lk_3^{j-k}(i)lk_0^k(i)(2^k-1)\\
ans=\sum_{1 \leq i \leq n} i(n-i+1)lk_2(i)lk_4^{q-1}(i)\sum_{0 \leq j < q}((\frac{(lk_3(i)+2lk_0(i))}{lk_4(i)})^{j}-(\frac{(lk_3(i)+lk_0(i))}{lk_4(i)})^{j})\\
lk_5(i)=\frac{(lk_3(i)+lk_0(i))}{lk_4(i)}\\
lk_6(i)=\frac{(lk_3(i)+2lk_0(i))}{lk_4(i)}\\
ans=\sum_{1 \leq i \leq n} i(n-i+1)lk_2(i)lk_4^{q-1}(i)(\frac{1-lk_6^q(i)}{1-lk_6(i)}-\frac{1-lk_5^q(i)}{1-lk_5(i)})\\
$$
七个恶臭系数都可以很快的算，于是这个恶臭问题也可以很容易完成。

---

定义汇总。
$$
lk_0(i)=i(n-i+1)m\\
lk_1(i)=\frac{i(n-i+1)m(m-1)}{2}\\
lk_2(i)=\frac{lk_1(i)}{lk_0(i)}\\
lk_3(i)=n(n+1)m-2i(n-i+1)m+\frac{n(n+1)}{2}\\
lk_4(i)=\frac{n(n+1)(2m+1)}{2}\\
lk_5(i)=\frac{(lk_3(i)+lk_0(i))}{lk_4(i)}\\
lk_6(i)=\frac{(lk_3(i)+2lk_0(i))}{lk_4(i)}\\
ans=lk_4^{q-1}\sum_{1 \leq i \leq n} i(n-i+1)lk_2(i)(\frac{1-lk_6^q(i)}{1-lk_6(i)}-\frac{1-lk_5^q(i)}{1-lk_5(i)})\\
$$

---

实现中需要注意的点：

- long long 或者更精细的取模实现。
- 等比数列在 $q=1$ 处的求值。

```cpp
#include<bits/stdc++.h>
#define int long long
const int mod=998244353,inv2=499122177;
int fpow(int a,int b=mod-2){
    int r=1;
    while(b){
        if(b&1)r=1ll*r*a%mod;
        a=1ll*a*a%mod;
        b>>=1;
    }
    return r;
}
int eb(int q,int n){
    if(q==1)return n;
    return 1ll*(1-fpow(q,n)+mod)%mod*fpow((1-q+mod)%mod)%mod;
}
using namespace std;
int n,m,q,ans;
signed main(){
    cin>>n>>m>>q;
    int lk4=1ll*n*(n+1)%mod*(2*m+1)%mod*inv2%mod;
    for(int i=1;i<=n;i++){
        int lk0=1ll*i*(n-i+1)%mod*m%mod;
        int lk1=1ll*lk0*(m-1)%mod*inv2%mod;
        int lk2=1ll*lk1*fpow(lk0)%mod;
        int lk3=(1ll*n*(n+1)%mod*m%mod-2ll*i*(n-i+1)%mod*m%mod+mod)%mod;
        lk3=(lk3+1ll*n*(n+1)%mod*inv2%mod)%mod;
        int lk5=1ll*(lk3+lk0)%mod*fpow(lk4)%mod;
        int lk6=1ll*(lk3+2*lk0)%mod*fpow(lk4)%mod;
        ans=(ans+1ll*i*(n-i+1)%mod*lk2%mod*(eb(lk6,q)-eb(lk5,q)+mod)%mod)%mod;
    }
    ans=1ll*ans*fpow(lk4,q-1)%mod;
    cout<<ans;
}
```

