# [ABC 145 E] All-you-can-eat

## 题面翻译

有 $n$ 道菜，每道菜需要 $a[i]$ 分吃完，能获得 $b[i]$ 的美味值给 $T$ 分钟，问怎么样吃，才能在 $T$ 内得到最多的美味值，

一旦开始了吃就一定会吃完这道菜，哪怕时间超过 $T$ 分

## 题目描述

[problemUrl]: https://atcoder.jp/contests/abc145/tasks/abc145_e

高橋君は食べ放題のお店に来ました。

$ N $ 種類の料理があり、$ i $ 番目の料理は、食べるために $ A_i $ 分必要で、美味しさは $ B_i $ です。

この店のルールは以下の通りです。

- $ 1 $ 度に $ 1 $ つの料理のみを注文することができます。注文した料理は即座に提供され、食べ始めることができます。
- 同じ種類の料理を $ 2 $ 度以上注文することはできません。
- 提供済みの料理を食べ終わるまで次の料理を注文することはできません。
- 最初の注文から $ T-0.5 $ 分後以降に注文することはできませんが、提供済みの料理を食べることはできます。

高橋君の満足度を、この来店で高橋君が食べる料理の美味しさの合計とします。

高橋君が適切に行動したとき、満足度は最大でいくらになるでしょうか。

## 输入格式

入力は以下の形式で標準入力から与えられる。

> $ N $ $ T $ $ A_1 $ $ B_1 $ $ : $ $ A_N $ $ B_N $

## 输出格式

高橋君が適切に行動したときの、満足度の最大値を出力せよ。

## 样例 #1

### 样例输入 #1

```
2 60
10 10
100 100
```

### 样例输出 #1

```
110
```

## 样例 #2

### 样例输入 #2

```
3 60
10 10
10 20
10 30
```

### 样例输出 #2

```
60
```

## 样例 #3

### 样例输入 #3

```
3 60
30 10
30 20
30 30
```

### 样例输出 #3

```
50
```

## 样例 #4

### 样例输入 #4

```
10 100
15 23
20 18
13 17
24 12
18 29
19 27
23 21
18 20
27 15
22 25
```

### 样例输出 #4

```
145
```

## 提示

### 制約

- $ 2\ \leq\ N\ \leq\ 3000 $
- $ 1\ \leq\ T\ \leq\ 3000 $
- $ 1\ \leq\ A_i\ \leq\ 3000 $
- $ 1\ \leq\ B_i\ \leq\ 3000 $
- 入力中のすべての値は整数である。

### Sample Explanation 1

$ 1 $ 番目、$ 2 $ 番目の順に料理を注文することで、満足度は $ 110 $ になります。注文が時間内に間に合えば、食べるのにどれだけ時間がかかっても良いことに注意してください。

### Sample Explanation 2

$ 60 $ 分以内に全ての料理を食べることができます。

### Sample Explanation 3

$ 2 $ 番目、$ 3 $ 番目の順に料理に注文することで、満足度を $ 50 $ にできます。 どのような順に料理を注文しても、料理を $ 3 $ つ注文することはできません。


## 题解
我们本题如果没有后面的限制：你点了之后也一定能你吃完，那么我们就是一个简单的背包问题。但是在添加了限制之后，我们的最大边界就可以来到我们的 $T+a_i+1$,但是我们要注意一点，如果我们按照这种方式来计算的时候，我们需要把我们的时间进行一个排序。因为我们的顺序显然会影响我们的答案，而 obviously，我们让先吃完的尽可能在前面吃是一种贪心最优解。

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;
inline int read() {
	int x = 0, m = 1;
	char ch = getchar();
	while(!isdigit(ch)) {
		if(ch == '-') m = -1;
		ch = getchar();
	}
	while(isdigit(ch)) {
		x = (x << 1) + (x << 3) + (ch ^ 48);
		ch = getchar();
	}
	return x * m;
}
inline void write(int x) {
	if(x < 0) {
		putchar('-');
		write(-x);
		return;
	}
	if(x >= 10) write(x / 10);
	putchar(x % 10 + '0');
}
struct info {
	int x, y;
} a[6000];
int f[6000];
inline bool cmp(info x, info y) {
	return x.x < y.x;
}
signed main() {
	int n = read(), t = read();
	for(int i = 1; i <= n; i ++) a[i].x = read(), a[i].y = read();
	sort(a + 1, a + n + 1, cmp);
	for(int i = 1; i <= n; i ++)
		for(int j = t + a[i].x - 1; j >= a[i].x; j --)
			f[j] = max(f[j], f[j - a[i].x] + a[i].y);
	int ans = 0;
	for(int i = 1; i < 6000; i ++) ans = max(ans, f[i]);
	write(ans);
	return 0;
}


```