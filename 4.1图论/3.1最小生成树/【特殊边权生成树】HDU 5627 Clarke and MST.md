Clarke is a patient with multiple personality disorder. One day he turned into a learner of graph theory.  
He learned some algorithms of minimum spanning tree. Then he had a good idea, he wanted to find the maximum spanning tree with bit operation AND.  
A spanning tree is composed by n−1 edges. Each two points of n points can reach each other. The size of a spanning tree is generated by bit operation AND with values of n−1 edges.  
Now he wants to figure out the maximum spanning tree.

克拉克是一名人格分裂患者。某一天克拉克变成了一名图论研究者。  
他学习了最小生成树的几个算法，于是突发奇想，想做一个位运算and的最大生成树。  
一棵生成树是由n−1条边组成的，且n个点两两可达。一棵生成树的大小等于所有在生成树上的边的权值经过位运算and后得到的数。  
现在他想找出最大的生成树。

## Input

The first line contains an integer T(1≤T≤5), the number of test cases.  
For each test case, the first line contains two integers n,m(2≤n≤300000,1≤m≤300000), denoting the number of points and the number of edge respectively.  
Then m lines followed, each line contains three integers x,y,w(1≤x,y≤n,0≤w≤109), denoting an edge between x,y with value w.  
The number of test case with n,m>100000 will not exceed 1.  

Output

For each test case, print a line contained an integer represented the answer. If there is no any spanning tree, print 0.

Sample

|Inputcopy|Outputcopy|
|---|---|
|1<br>4 5<br>1 2 5<br>1 3 3<br>1 4 2<br>2 3 1<br>3 4 7|1|
## 题解
我们本题相当于求一颗最小与运算生成树。
假设通过与运算得到的最大值为 m，那么 m 每个二进制位上的1都应该与这棵树所有边的权值的二进制位对应位置的1相同。又因为进位制的性质，高位的数比所有低位的数之和还要大。所以我们可以通过从高到低来枚举二进制位，如果这个位上为1的边可以构成一棵生成树，那么就让这位为0的边都删掉，继续枚举下一位。   


```cpp
#include <iostream>
#include <cmath>
#include <cstdlib>
#include <time.h>
#include <vector>
#include <cstdio>
#include <cstring>
using namespace std;
#define maxn 311111
#define maxm 611111
 
struct node {
	int u, v;
	long long w;
};
vector <node> edge[2];
int n, m;
 
 
int fa[maxn];
int find (int x) {
	return fa[x] == x ? fa[x] : fa[x] = find (fa[x]);
}
bool ok (int id) { //判断能不能构成一个联通分量
	for (int i = 1; i <= n; i++)
		fa[i] = i;
	int Max = edge[id].size ();
	for (int i = 0; i < Max; i++) {
		int u = edge[id][i].u, v = edge[id][i].v;
		int p1 = find (u), p2 = find (v);
		if (p1 != p2)
			fa[p1] = p2 ;
	}
	int gg = find (1);
	for (int i = 2; i <= n; i++) {
		if (find (i) != gg)
			return 0;
	}
	return 1;
}
 
bool is_1 (long long num, int id) { //判断num的id位是不是1
	id--;
	num >>= id;
	return (num&1);
}
 
bool work (int id) { //判断id位能否为1
	int Max = edge[0].size ();
	for (int i = 0; i < Max; i++) {
		if (is_1 (edge[0][i].w, id)) {
			edge[1].push_back (edge[0][i]);
		}
	}
	if (ok (1)) {
		edge[0].clear ();
		int Max = edge[1].size ();
		for (int i = 0; i < Max; i++) {
			edge[0].push_back (edge[1][i]);
		}
		return 1;
	}
	return 0;
}
 
int main () {
	int t;
	scanf ("%d", &t);
	while (t--) {
		scanf ("%d%d", &n, &m);
		edge[0].clear (); edge[1].clear ();
		for (int i = 1; i <= m; i++) {
			int u, v; long long w;
			scanf ("%d%d%lld", &u, &v, &w);
			edge[0].push_back ((node){u, v, w});
		}
		if (!ok (0)) {
			printf ("0\n");
			continue;
		}
		long long ans = 0; 
		for (int i = 32; i >= 1; i--) { 
			edge[1].clear ();
			if (work (i)) { 
				ans <<= 1;
				ans++;
			}
			else 
				ans <<= 1;
		}
		printf ("%lld\n", ans);
	}
	return 0;
}
```