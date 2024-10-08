ACM 集训队的龙哥哥一直想要抽取某个游戏的角色。但是游戏角色的获取需要足够多的原石，而获取原石最快速的方法是氪金。可是龙哥哥现在并没有足够的财力支持他获取想要的角色。

所以，龙哥哥购买了一个存钱罐，将平时剩余的钱投入到存钱罐中，这个过程不可逆，因为只有把存钱罐打碎才能取出硬币。在抽奖池子结束之前，存钱罐中终于有了一定的现金，用于氪金抽奖。

但是，龙哥哥的存钱罐存在一个很大的问题，即无法确定其中有多少钱。因此，我们可能在打碎存钱罐之后，发现里面的钱不够。显然，龙哥哥希望避免这种不愉快的情况。所以，聪明的明明子想到一个办法，我们可以称一下存钱罐的重量，并尝试猜测里面的有多少硬币。假定我们能够精确判断存钱罐的重量，并且我们也知道所有硬币的重量。那么，我们可以保证存罐中最少有多少钱。

输入

输入包含 T 组测试数据。输入文件的第一行，给出了 T 的值。 对于每组测试数据，第一行包含 E 和 F 两个整数，它们表示空的存钱罐的重量，以及装有硬币的存钱罐的重量。两个重量的计量单位都是 g (克)。存钱罐的重量不会超过 10 kg (千克)，即 1 <= E <= F <= 10000 。

每组测试数据的第二行，有一个整数 N (1 <= N <= 500)，提供了给定币种的不同硬币有多少种。

接下来的 N 行，每行指定一种硬币类型，每行包含两个整数 P 和 W (1 <= P <= 50000，1 <= W <=10000)。P 是硬币的金额 (货币计量单位)；W 是它的重量，以 g (克) 为计量单位。

输出

对于每组测试数据，打印一行输出。

每行必须包含句子"The minimum amount of money in the piggy-bank is X."

其中，X 表示对于给定总重量的硬币，所能得到的最少金额。 如果无法恰好得到给定的重量，则打印一行"This is impossible."。

示例

|Inputcopy|Outputcopy|
|---|---|
|3<br>10 110<br>2<br>1 1<br>30 50<br>10 110<br>2<br>1 1<br>50 30<br>1 6<br>2<br>10 3<br>20 4|The minimum amount of money in the piggy-bank is 60.<br>The minimum amount of money in the piggy-bank is 100.<br>This is impossible.|

## 题解
我们这一题是标准的无限背包问题，我们只需要按照无限背包的模板来求我们的最小值即可。
但是与我们的无限背包不同的是，我们的无限背包要求的是我们小于等于，而我们这里要我们等于。这一点，我们的区别在于我们的初始化，在无限背包里，我们选择初始化我们的背包为 0，但在这里，我们只能把我们的第一个初始化为 0，其他都初始化成正无穷。

```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int INF=0x3f3f3f3f;
int main()
{
	//背包问题变式:要求背包恰好装满,只要在初始化时做一些改变；
	//由于初始化的情况为该情况满足条件,在没有要求装满的时候,所有的dp[i]都可以初始化为0， 
	//这是因为在没有要求装满时,对于所有的dp[i]在容量为0时价值均为0满足条件;
	//但是如果条件限制为必须装满,那只有dp[0]才能初始化为0,因为只有这种情况才属于装满;
	//对于除dp[0]之外其他的dp[i],根据目标而设定为INF或-INF,
	//若问背包装满的最小价值,则初始化为INF,若问最大价值,则初始化为-INF;
	int i,j;
	int n;
	int t;
	int w1,w2,m;
	int dp[10005];
	int v[550],w[550];
	scanf("%d",&t);
	while(t--)
	{
		scanf("%d%d",&w1,&w2);
		m=w2-w1;
		scanf("%d",&n);
		for(i=1;i<=n;i++)
		{
			scanf("%d%d",&v[i],&w[i]);
		}
		memset(dp,INF,sizeof(dp));
		dp[0]=0;
		for(i=1;i<=n;i++)
		{
			for(j=w[i];j<=m;j++)//完全背包正序循环,01背包逆序循环;
			{
				dp[j]=min(dp[j],dp[j-w[i]]+v[i]);
			 } 
		}
		if(dp[m]==INF)
		{
			printf("This is impossible.\n");
		}
		else
		{
			printf("The minimum amount of money in the piggy-bank is %d.\n",dp[m]);
		}
	 } 
	 return 0;
}

```