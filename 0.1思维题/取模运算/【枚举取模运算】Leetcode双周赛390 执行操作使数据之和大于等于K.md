## 题意
给你一个正整数 k 。最初，你有一个数组 $nums = [1]$ 你可以对数组执行以下任意操作任意次数 (可能为零) :
 · 选择数组中的任何一个元素，然后将它的值增加 1 。
 · 复制数组中的任何一个元素，然后将它附加到数组的末尾。
 返回使得最终数组元素之和大于或等于 k 所需的最少操作次数。

## 范围：
- `1 <= k <= 105`

## 样例：
```
11
```

```
5
```

## 题解
本题，我们不难发现，我们一定是先进行我们的加法然后再去进行我们的乘法是更优的，因此，我们只需要关进我们进行多少次加法，再去进行多少次乘法，能够得到我们的答案。

这里，我们显然，由于我们数据范围的原因，我们可以考虑枚举先把我们的最大的数变成哪一个，假设我们要变为 $x$,然后我们的最终答案就是：$x-1+[\frac{k}{x}](up)$，。最后，我们如果不考虑其他因素的话，我们只需要枚举到我们的 $1-1e5$,显然足以通过本题。

但是，我们本题存在优化，注意到上面的式子类似于我们的对勾函数，而我们的对勾函数在我们的 $\sqrt{ k }$ 处附近取到我们的最值，因此我们只需要计算我们的对勾函数附近的个数，即可的大我们的答案。