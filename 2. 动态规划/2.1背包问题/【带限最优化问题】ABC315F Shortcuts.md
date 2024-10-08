# [ABC 315 F] Shortcuts

## 题面翻译

对于平面坐标上 $n$ 个点，输出从 $1$ 走到 $n$ 的**代价**。两点直线距离为欧几里得距离。

正常来讲，你的路径应该为 $1 \to 2\to 3 \to \cdots \to (n-1)\to n$，你的代价即为你走过的距离。

但是你**被允许跳过一些点**，使路径变为 $1\to 2\to \cdots \to (x-1) \to (x+1) \to \cdots \to (n-1) \to n$，跳过的点数不限，但 $1$ 号点和 $n$ 号点不能被跳过。

跳过操作需要花费，此时你的**代价**为你走过的距离加跳过产生的花费。

跳过的花费如下定义：

对于整个旅途，如果你跳过 $c$ 个点，设花费为 $S$，

- $c=0$ 时 $S=0$；
- $c>0$ 时 $S=2^{c-1}$。

输出从 $1$ 走到 $n$ 的最小**代价**。 

___

$\text{Translate by Rain.}$

## 题目描述

[problemUrl]: https://atcoder.jp/contests/abc315/tasks/abc315_f

座標平面上でチェックポイント $ 1,2,\dots, N $ をこの順に通るレースが行われます。  
チェックポイント $ i $ の座標は $ (X_i, Y_i) $ であり、すべてのチェックポイントの座標は異なります。

チェックポイント $ 1, N $ 以外のチェックポイントは、通過を省略することもできます。  
ただし、通らなかったチェックポイントの個数を $ C $ として、以下の通りペナルティが課せられます。

- $ C\ >\ 0 $ なら $ \displaystyle\ 2^{C−1} $
- $ C=0 $ なら $ 0 $

チェックポイント $ 1 $ からチェックポイント $ N $ までの総移動距離（ユークリッド距離）とペナルティの和を $ s $ とします。  
このとき、 $ s $ として達成可能な最小の値を求めてください。

## 输入格式

入力は以下の形式で標準入力から与えられる。

> $ N $ $ X_1 $ $ Y_1 $ $ X_2 $ $ Y_2 $ $ \vdots $ $ X_N $ $ Y_N $

## 输出格式

答えを出力せよ。出力は、真の値との絶対誤差または相対誤差が $ 10^{−5} $ 以下のとき正解と判定される。

## 样例 #1

### 样例输入 #1

```
6
0 0
1 1
2 0
0 1
1 0
2 1
```

### 样例输出 #1

```
5.82842712474619009753
```

## 样例 #2

### 样例输入 #2

```
10
1 8
3 7
9 4
4 9
6 1
7 5
0 0
1 3
6 8
6 4
```

### 样例输出 #2

```
24.63441361516795872523
```

## 样例 #3

### 样例输入 #3

```
10
34 24
47 60
30 31
12 97
87 93
64 46
82 50
14 7
17 24
3 78
```

### 样例输出 #3

```
110.61238353245736230207
```

## 提示

### 制約

- 入力は全て整数
- $ 2\ \le\ N\ \le\ 10^4 $
- $ 0\ \le\ X_i, Y_i\ \le\ 10^4 $
- $ i\ \neq\ j $ ならば $ (X_i, Y_i)\ \neq\ (X_j, Y_j) $

### Sample Explanation 1

チェックポイント $ 1,2,5,6 $ を通過し、 $ 3,4 $ の通過を省略することを考えます。 - チェックポイント $ 1\ \rightarrow\ 2 $ に移動する。 $ 2 $ 点間の距離は $ \sqrt{2} $ である。 - チェックポイント $ 2\ \rightarrow\ 5 $ に移動する。 $ 2 $ 点間の距離は $ 1 $ である。 - チェックポイント $ 5\ \rightarrow\ 6 $ に移動する。 $ 2 $ 点間の距離は $ \sqrt{2} $ である。 - 通らなかったチェックポイントは $ 2 $ つであり、このとき科せられるペナルティは $ 2 $ である。以上のようにして、 $ s\ =\ 3\ +\ 2\sqrt{2}\ \approx\ 5.828427 $ を達成できます。 $ s $ をこの値より小さくすることはできません。


## 题解
我们很显然的可以想到我们的状态为 $dp[i][j]$ 表示我们到达 $i$ 点用了 $j$ 个 free 的最小代价是多少，那么我们显然可以用我们的 $dp[i][j]=dp[j][k-(i-j-1)]+dist(i,j)$ 来计算
**注意，如果我们这里没有我们的 $dist(i,j)$，我们就可以转化为相邻来转移，但因为我们这题相邻转移的话，我们的距离不便于计算，所以我们选择用我们的区间转移。**
然后，我们又因为我们的 $dist(i,j)\leq \sqrt{ 2 }\times 1e_{8}$,所以我们的跳跃次数不会超过我们的 30，所以我们就可以用我们的暴力 dp 来转移即可。
```cpp
#include<iostream>
#include<cmath>
#include<cstring>
using namespace std;
int n,x[10005],y[10005];
double f[10005][30];
double dis(int i,int j){
    return sqrt((x[i]-x[j])*(x[i]-x[j])+(y[i]-y[j])*(y[i]-y[j]));
}
int main(){
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>x[i]>>y[i];
        for(int j=0;j<=25;j++)f[i][j]=1e9;
    }
    f[1][0]=0;
    for(int i=1;i<=n;i++){
        for(int j=0;j<min(i,25);j++)f[i][j]=min(f[i][j],f[i-1][j]+dis(i,i-1));
        for(int j=1;j<i-1;j++){
            for(int k=i-j-1;k<=25;k++){
                f[i][k]=min(f[i][k],f[j][k-(i-j-1)]+dis(i,j));
            }
        }
    }
    double ans=f[n][0];
    for(int i=1;i<=25;i++){
        ans=min(ans,f[n][i]+pow(2,i-1));
    }
    printf("%.6lf",ans);
    return 0;
}
```
