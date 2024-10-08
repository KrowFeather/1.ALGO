# [ABC 279 F] BOX

## 题面翻译

有 $N$ 个箱子和无数个编号从 $1$ 开始的球，第 $i$ 个箱子开始时只装了编号为 $i$ 的球。

有 $Q$ 次操作，每次操作分别可能为：

- `1 X Y` 将 $Y$ 箱中的球全部放入 $X$ 箱。
- `2 X` 将目前最小的未被放到箱子里的球放到 $X$ 箱。
- `3 X` 查询 $X$ 球在哪个箱子中，输出该箱编号，输出间用换行隔开。

## 题目描述

[problemUrl]: https://atcoder.jp/contests/abc279/tasks/abc279_f

$ N $ 個の箱 $ 1,2,\dots, N $ と、 $ 10^{100} $ 個のボール $ 1,2,\dots, 10^{100} $ があります。最初、箱 $ i $ にはボール $ i $ のみが入っています。

ここに以下の操作が合計 $ Q $ 回行われるので、処理してください。

操作にはタイプ $ 1,2,3 $ の $ 3 $ 種類があります。

タイプ $ 1 $ : 箱 $ X $ に箱 $ Y $ の中身を全て入れる。 この操作では $ X\ \neq\ Y $ が保証される。

> 1 $ X $ $ Y $

タイプ $ 2 $ : 現在いずれかの箱に入っているボールの数の合計を $ k $ とすると、箱 $ X $ にボール $ k+1 $ を入れる。

> 2 $ X $

タイプ $ 3 $ : ボール $ X $ が入っている箱の番号を答える。

> 3 $ X $

## 输入格式

入力は以下の形式で標準入力から与えられる。  
但し、 $ op_i $ は $ i $ 回目の操作を表す。

> $ N $ $ Q $ $ op_1 $ $ op_2 $ $ \vdots $ $ op_Q $

## 输出格式

各タイプ $ 3 $ の操作に対して、答えを $ 1 $ 行に $ 1 $ つ、整数として出力せよ。

## 样例 #1

### 样例输入 #1

```
5 10
3 5
1 1 4
2 1
2 4
3 7
1 3 1
3 4
1 1 4
3 7
3 6
```

### 样例输出 #1

```
5
4
3
1
3
```

## 提示

### 制約

- 入力は全て整数
- $ 2\ \le\ N\ \le\ 3\ \times\ 10^5 $
- $ 1\ \le\ Q\ \le\ 3\ \times\ 10^5 $
- タイプ $ 1 $ の操作について、 $ 1\ \le\ X, Y\ \le\ N $ かつ $ X\ \neq\ Y $

- タイプ $ 2 $ の操作について、 $ 1\ \le\ X\ \le\ N $
- タイプ $ 3 $ の操作について、その時点でボール $ X $ がいずれかの箱に入っている
- タイプ $ 3 $ の操作が少なくとも $ 1 $ つ与えられる

### Sample Explanation 1

この入力は $ 10 $ 個の操作を含みます。 - $ 1 $ 回目の操作はタイプ $ 3 $ です。ボール $ 5 $ は箱 $ 5 $ に入っています。 - $ 2 $ 回目の操作はタイプ $ 1 $ です。箱 $ 1 $ に箱 $ 4 $ の中身を全て入れます。 - 箱 $ 1 $ の中身はボール $ 1,4 $ 、箱 $ 4 $ の中身は空になります。 - $ 3 $ 回目の操作はタイプ $ 2 $ です。箱 $ 1 $ にボール $ 6 $ を入れます。 - $ 4 $ 回目の操作はタイプ $ 2 $ です。箱 $ 4 $ にボール $ 7 $ を入れます。 - $ 5 $ 回目の操作はタイプ $ 3 $ です。ボール $ 7 $ は箱 $ 4 $ に入っています。 - $ 6 $ 回目の操作はタイプ $ 1 $ です。箱 $ 3 $ に箱 $ 1 $ の中身を全て入れます。 - 箱 $ 3 $ の中身はボール $ 1,3,4,6 $ 、箱 $ 1 $ の中身は空になります。 - $ 7 $ 回目の操作はタイプ $ 3 $ です。ボール $ 4 $ は箱 $ 3 $ に入っています。 - $ 8 $ 回目の操作はタイプ $ 1 $ です。箱 $ 1 $ に箱 $ 4 $ の中身を全て入れます。 - 箱 $ 1 $ の中身はボール $ 7 $ 、箱 $ 4 $ の中身は空になります。 - $ 9 $ 回目の操作はタイプ $ 3 $ です。ボール $ 7 $ は箱 $ 1 $ に入っています。 - $ 10 $ 回目の操作はタイプ $ 3 $ です。ボール $ 6 $ は箱 $ 3 $ に入っています。

## 题解
我们本题中不难发现，我们本题中存在一个合并的操作，就是把我们某个箱子中的全部元素都放到我们的另一个箱子当中去。但是，我们这一题还有一个就是查询 X 球在哪里。
由于要支持类似合并集合的操作，首先想到并查集。但是题目中的把 y 倒入 x 之后 y 是空的。按照一般的并查集来做的话，此时原来属于 y 的集合仍属于 y，并没有清空。

所以我们对于题目中的每个把 y 倒入 x 中的操作，在合并 x 和 y 之后新建一个并查集节点代替原来代表 y 的并查集节点，这样就实现了清空 y。其他的按正常的并查集做就好。

```cpp
const int N = 1000005;
int c[N],w[N];
//c是每个箱子对应的是哪个并查集节点
//w是每个球对应的是哪个并查集节点
int ans[N];//每个并查集节点对应的是哪个箱子
int fa[N];
int n,q;

int find(int x){
    if(fa[x]==x)
        return fa[x];
    return fa[x]=find(fa[x]);
}

int main(){
    cin >> n >> q;
    for(int k=1;k<=n;k++)
        fa[k] = ans[k] = c[k] = w[k] = k;
    int siz = n,idx = n;
    while(q--){
        int ops;
        cin >> ops;
        if(ops==1){
            int x,y;
            cin >> x >> y;
            fa[c[y]] = c[x];
            c[y] = ++idx;
            fa[idx] = idx;//对于 y 新建一个并查集节点
            ans[idx] = y;
        }
        else if(ops==2){
            int x;
            cin >> x;
            w[++siz] = c[x];
        }
        else{
            int x;
            cin >> x;
            cout << ans[find(w[x])] << endl;
        }
    }
    return 0;
}
```

每个合并操作新建一个节点，至多有 �+�n+q 个节点。