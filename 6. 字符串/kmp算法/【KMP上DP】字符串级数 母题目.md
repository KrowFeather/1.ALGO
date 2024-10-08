## 题目描述
字符集中有一些字符（最多 26 个），再给出一个字串 B，长为 M。
求：任给一个长度为 N 的字符串 A（只包含字符集中的字符），使 B 是 A 的子串的方案数。
N<=100;

## 样例 #1

### 样例输入 #1

```

```

### 样例输出 #1

```

```

## 题解
想象一边生成字符串 A，一边用 KMP 匹配到字符串 B 的过程。
$f[i][j]$ 表示生成到第 i 位，此时 B 串匹配到第 j 位的方案。
枚举下一位生成字符 c，计算出 KMP 匹配指针应该移动到 $j_{1}$,我们进行转移**如果匹配成功**，那么我们由
$f[i+1][j_{1}]+=f[i][j]$,**否则**
记 $g[j][j_{1}]$ 为从 $f[i][j]$ 转移到 $f[i+1][j_{1}]$ 的方案数：
$f[i+1][j_{1}]=\sum f[i][j]*g[j][j_{1}]$
注意: 如果我们的 j=M，我们是不进行转换的，因为他已经是 $f[i+1][0]$ 的一部分，我们再转移就是重复计算了.
而我们的 g 数组可以通过我们的 kmp 算法来进行预处理。


我们的答案就是 $\sum dp[N][i]$
## 代码
```cpp

```