# 知识点
  ## [[差分数组]] [[求最值]] [[符合条件的最值]]
# 题目
 你可能听过关于金发姑娘和三只熊的经典故事。

然而，鲜为人知的是，金发姑娘最终成了一个农民。

在她的农场中，她的牛棚里有 $N$ 头奶牛。

不幸的是，她的奶牛对温度相当敏感。

对于奶牛 $i$，使其感到舒适的温度为 $Ai…Bi$。

如果金发姑娘将牛棚的恒温器的温度 $T$ 设置为 $T<Ai$ ，奶牛就会觉得冷，并会产出 $x$ 单位的牛奶。

如果她将恒温器的温度 $T$  设置在 $Ai≤T≤Bi$ ，奶牛就会感到舒适，并会产出 $Y$  单位的牛奶。

如果她将恒温器的温度 $T$  设置为 $T>Bi$ ，奶牛就会觉得热，并会产出 $Z$ 单位的牛奶。

正如所期望的那样，$Y$  的值始终大于 $X$  和 $Z$ 。

给定 $X,Y,Z$  以及每头奶牛感到舒适的温度范围，请计算通过合理设定恒温器温度，金发姑娘可获得的最大产奶量。

恒温器温度可设置为任何整数。

#### 输入格式

第一行包含四个整数 $N,X,Y,Z$ 。

接下来 $N$  行，每行包含两个整数 $Ai$  和 $Bi$。

#### 输出格式

输出可获得的最大产奶量。

#### 数据范围

$1≤N≤2000$   
$0≤X,Y,Z≤1000$  
$0≤Ai≤Bi≤10^9$ 

#### 输入样例：

```
4 7 9 6
5 8
3 4
13 20
7 10
```

#### 输出样例：

```
31
```

#### 样例解释

金发姑娘可以将恒温器温度设置为 7 或 8，这样会让奶牛 1 和 4 感到舒适，奶牛 2 感到热，奶牛 3 感到冷。

共可获得 31 单位牛奶。

# 思路
·暴力思路下我们当然可以直接列举每一个温度，然后我们再计算对应温度下所产生的牛奶的量然后再比较，温度的上界就是输入温度的最大值。
·其次，我们还可以在前面的基础上优化一下，比如我们可以把问题转化成如下
根据题目，把每头奶牛在不同温度下的产奶量列出来，可以构成一个 
$n*T$ 的矩阵，其中 n 表示奶牛数量，T 表示最大温度。
就像下面的矩阵一样。
XXXYYYZZZZ//表示一头牛的状态
XXYYYZZZZZ//表示第二头牛的状态
XYYYYZZZZZ//表示第三头牛的状态

我们的目标就是要找到和最大的一列的和。
·因为 T 比较大，所以直接暴力枚举 T 肯定是不可行的。观察矩阵可以发现，每一行的值都是三个区间，区间 $[0,A_{i}-1],[A_{i},B_{i}],\left[ B_{i+1},\infty \ \right]$，显然可以用差分来表示。

然而传统差分数组不能够表示这么大的温度，所以需要用 map 来表示差分的值。
# AC 代码
```cpp
#include<cstdio>
#include<map>
using namespace std;
map<int,int> mp;
int main(){
    int n,x,y,z;
    scanf("%d%d%d%d",&n,&x,&y,&z);
    int l,r;
    for(int i=0;i<n;i++){
        scanf("%d%d",&l,&r);
        mp[0]+=x;
        mp[l]=mp[l]-x+y;
        mp[r+1]=mp[r+1]-y+z;
    }
    int mx=0,cur=0;
    for(auto iter:mp){
        cur+=iter.second;
        if(cur>mx){
            mx=cur;
        }
    }
    printf("%d\n",mx);
    return 0;
}

```
# 备注
