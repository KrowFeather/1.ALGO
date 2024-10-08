# 
## 题目描述
电视节目WORLD OF WONDER非常有名. 为了满足猛男的需要, 王♂举办了这个节目. 这个节目在健身房里举办, 所以它吸引了很多的猛男. 现在有n个猛男报名参加. 一开始, n个猛男站成一排，一个接一个地走进更衣室。然而，王突然知道每个猛男都有一个蕉♂急因数D, 如果一名猛男是第k个走进更衣室, 他的蕉急程度就是(k-1) * D, 因为他必须要等待(k-1)个人. 幸运的是，健身房里有一个妙妙屋, 所以王可以先暂时让一个猛男走进妙妙屋， 并让他身后的猛男走到他的前面. 因为妙妙屋深邃又黑暗, 所以先进去的猛男必须最后离开. 王想要通过妙妙屋来改变猛男排队的顺序, 使得总的蕉急度最小. 你能帮♂帮他吗?

## 输入格式
第一行有一个数字T, 表示输入的组数. 对于每组数据，第一行都是n (0 < n <= 100)  
接下来n行的整数D1-Dn表示每个每个猛男的蕉急因数 (0 <= Di <= 100)


## 输出格式
对于每组数据，输出最小的蕉急度


## 样例 #1

### 样例输入 #1

```
2
　　
5
1
2
3
4
5

5
5
4
3
2
2
```

### 样例输出 #1

```
Case #1: 20
Case #2: 24
```

## 题解
我们依然按照区间dp的思路来设计我们的状态，但不同的是，我们这一次要设置的中间k，$dp[i][j]$表示从i到j所能获得的最小交集度。而我们的状态转移就是比较有技巧的。

引理：**如果我们的第一个元素是第k个出栈的，并且每个元素只进出一次，那么我们2-k-1个元素一定在第一个元素之前出栈。k+1,j间的元素一定在第一个元素之后出栈**

根据我们的引理，我们就能设置k表示第i个元素是第k个出栈，我们的计算就可以分成三类，第一类（i+1到i+k-1),即$dp[i+1][i+k-1]$，相当于我们i后面的k-1个人现在排到了i的前面。第二类i自己,我们可以直接带公式计算$D[i]*(k-1)$第三类是我们的第k个位置后面的,我们可以用$dp[i+k][j]+k*sum[j]-sum[i+k-1]$,相当于后面的人都延后了k，

更新思路：我们确定每一个元素在序列中的位置，然后计算这个位置的代价，和后序位置的代价，前面的代价我们可以通过
## 代码
```cpp
for(int len=2;len<=n;len++){
	for(int i=1;i<=len+1;i++){
		int j=i+len-1;
		dp[i][j]=INF;
		for(int k=1;k<=j-i+1;k++){
			dp[i][j]=min(dp[i][j],dp[i+1][i+k-1]+D[i]*(k-1)+dp[i+k][j]+k*sum[j]-sum[i+k-1])
		}
	}
}
```