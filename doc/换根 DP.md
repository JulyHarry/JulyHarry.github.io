# 换根 DP



## [2581. 统计可能的树根数目](https://leetcode.cn/problems/count-number-of-possible-root-nodes/) []

> Alice 有一棵 `n` 个节点的树，节点编号为 `0` 到 `n - 1` 。树用一个长度为 `n - 1` 的二维整数数组 `edges` 表示，其中 `edges[i] = [ai, bi]` ，表示树中节点 `ai` 和 `bi` 之间有一条边。
>
> Alice 想要 Bob 找到这棵树的根。她允许 Bob 对这棵树进行若干次 **猜测** 。每一次猜测，Bob 做如下事情：
>
> - 选择两个 **不相等** 的整数 `u` 和 `v` ，且树中必须存在边 `[u, v]` 。
> - Bob 猜测树中 `u` 是 `v` 的 **父节点** 。
>
> Bob 的猜测用二维整数数组 `guesses` 表示，其中 `guesses[j] = [uj, vj]` 表示 Bob 猜 `uj` 是 `vj` 的父节点。
>
> Alice 非常懒，她不想逐个回答 Bob 的猜测，只告诉 Bob 这些猜测里面 **至少** 有 `k` 个猜测的结果为 `true` 。
>
> 给你二维整数数组 `edges` ，Bob 的所有猜测和整数 `k` ，请你返回可能成为树根的 **节点数目** 。如果没有这样的树，则返回 `0`。
>
>  
>
> **示例 1：**
>
> <img src="https://assets.leetcode.com/uploads/2022/12/19/ex-1.png" alt="img" style="zoom: 50%;" />
>
> ```
> 输入：edges = [[0,1],[1,2],[1,3],[4,2]], guesses = [[1,3],[0,1],[1,0],[2,4]], k = 3
> 输出：3
> 解释：
> 根为节点 0 ，正确的猜测为 [1,3], [0,1], [2,4]
> 根为节点 1 ，正确的猜测为 [1,3], [1,0], [2,4]
> 根为节点 2 ，正确的猜测为 [1,3], [1,0], [2,4]
> 根为节点 3 ，正确的猜测为 [1,0], [2,4]
> 根为节点 4 ，正确的猜测为 [1,3], [1,0]
> 节点 0 ，1 或 2 为根时，可以得到 3 个正确的猜测。
> ```
>
> **示例 2：**
>
> <img src="https://assets.leetcode.com/uploads/2022/12/19/ex-2.png" alt="img" style="zoom:50%;" />
>
> ```
> 输入：edges = [[0,1],[1,2],[2,3],[3,4]], guesses = [[1,0],[3,4],[2,1],[3,2]], k = 1
> 输出：5
> 解释：
> 根为节点 0 ，正确的猜测为 [3,4]
> 根为节点 1 ，正确的猜测为 [1,0], [3,4]
> 根为节点 2 ，正确的猜测为 [1,0], [2,1], [3,4]
> 根为节点 3 ，正确的猜测为 [1,0], [2,1], [3,2], [3,4]
> 根为节点 4 ，正确的猜测为 [1,0], [2,1], [3,2]
> 任何节点为根，都至少有 1 个正确的猜测。
> ```
>
>  
>
> **提示：**
>
> - `edges.length == n - 1`
> - `2 <= n <= 105`
> - `1 <= guesses.length <= 105`
> - `0 <= ai, bi, uj, vj <= n - 1`
> - `ai != bi`
> - `uj != vj`
> - `edges` 表示一棵有效的树。
> - `guesses[j]` 是树中的一条边。
> - `guesses` 是唯一的。
> - `0 <= k <= guesses.length`

### 方法

换根 DP

### 思路

针对图中没有明确根节点的树，可以使用换根DP，计算当前根节点和相邻节点的变化量，只针对变化量进行计算。

本题中，如果以 $x$ 为根节点，$guesses$ 作为哈希表， 那么遍历时，可以统计出以 $x$ 为根节点的复合 $guesses$ 的数量 $cnt_x$ . 当遍历其相邻节点（如 $y$）时， $cnt_x$ 仅在 $(x, y)$ $(y, x)$ 出现在 $guesses$ 中会发生变化。

如果  $(x, y)$ 出现，则 $cnt_y = cnt_x  - 1$
如果  $(y, x)$ 出现，则 $cnt_y = cnt_x + 1$

遍历时统计 $cnt_y >= k$ 的情况即可

### 代码

```Python
class Solution:
    def rootCount(self, edges: List[List[int]], guesses: List[List[int]], k: int) -> int:
        n = len(edges) + 1
        g = [[] for _ in range(n)]
        for x, y in edges:
            g[x].append(y)
            g[y].append(x)

        s = {(x, y) for x, y in guesses}

        def dfs(x, fa):
            nonlocal cnt0
            for y in g[x]:
                if y != fa:
                    cnt0 += (x, y) in s
                    dfs(y, x)

        def reroot(x, fa, cnt):
            nonlocal ans
            ans += cnt >= k
            for y in g[x]:
                if y != fa:
                    reroot(y, x, cnt - ((x, y) in s) + ((y, x) in s))

        cnt0, ans = 0, 0
        dfs(0, -1)
        reroot(0, -1, cnt0)
        return ans
```



## [834. 树中距离之和](https://leetcode.cn/problems/sum-of-distances-in-tree/) [2197]

> 给定一个无向、连通的树。树中有 `n` 个标记为 `0...n-1` 的节点以及 `n-1` 条边 。
>
> 给定整数 `n` 和数组 `edges` ， `edges[i] = [ai, bi]`表示树中的节点 `ai` 和 `bi` 之间有一条边。
>
> 返回长度为 `n` 的数组 `answer` ，其中 `answer[i]` 是树中第 `i` 个节点与所有其他节点之间的距离之和。
>
>  
>
> **示例 1:**
>
> <img src="https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist1.jpg" alt="img" style="zoom:50%;" />
>
> ```
> 输入: n = 6, edges = [[0,1],[0,2],[2,3],[2,4],[2,5]]
> 输出: [8,12,6,10,10,10]
> 解释: 树如图所示。
> 我们可以计算出 dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5) 
> 也就是 1 + 1 + 2 + 2 + 2 = 8。 因此，answer[0] = 8，以此类推。
> ```
>
> **示例 2:**
>
> <img src="https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist2.jpg" alt="img" style="zoom:50%;" />
>
> ```
> 输入: n = 1, edges = []
> 输出: [0]
> ```
>
> **示例 3:**
>
> <img src="https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist3.jpg" alt="img" style="zoom:50%;" />
>
> ```
> 输入: n = 2, edges = [[1,0]]
> 输出: [1,1]
> ```
>
>  
>
> **提示:**
>
> - `1 <= n <= 3 * 104`
> - `edges.length == n - 1`
> - `edges[i].length == 2`
> - `0 <= ai, bi < n`
> - `ai != bi`
> - 给定的输入保证为有效的树

### 方法

换根 DP

### 思路

<img src="https://pic.leetcode.cn/1689398667-omjvbD-lc834.png"  />

我们先计算 $0$ 的距离和， 再计算 $2$ 的距离和，可以发现相较于起点 $0$ 的，起点 $2$ 的的子树（包含自身）距离都减少 $1$ ， 而不在 $2$ 的子树（包含自身）的其他点都增加了 $1$ 。

这种关系会发生在父节点和其子节点上，当我们求得父节点的距离和后，可以推算出其子节点的距离和，递归后可以自上而下得到所有节点的距离和。

假设父节点 $x$ ，子节点 $y$ ， $y$ 对应的子树（包含自身）的大小为 $size[y]$ ， 节点一共有 $n$ 个， 那么可以得到以下公式：
$ans[y] = ans[x] - size[y] + n - size[y]  = ans[x] + n - 2 * size[y] $


以图中的这棵树为例，从「以 $0$ 为根」换到「以 $2$ 为根」时，原来 $2$ 的子节点还是 $2$ 的子节点，原来 $1$ 的子节点还是 $1$ 的子节点，**唯一改变的是 $0$ 和 $2$ 的父子关系**。由此可见，一对节点的距离的「变化量」应该是很小的，那么找出「变化量」的规律，就可以基于 $\textit{ans}[0]$ 算出 $\textit{ans}[2]$ 了。这种算法叫做**换根 DP**。



### 代码

```Python
class Solution:
    def sumOfDistancesInTree(self, n: int, edges: List[List[int]]) -> List[int]:
        g = [[] for _ in range(n)]
        for x, y in edges:
            g[x].append(y)
            g[y].append(x)
        ans = [0] * n
        size = [1] * n

        def dfs(x, fa, h):
            ans[0] += h
            for y in g[x]:
                if y != fa:
                    dfs(y, x, h + 1)
                    size[x] += size[y]

        def reroot(x, fa):
            for y in g[x]:
                if y != fa:
                    ans[y] = ans[x] + n - 2 * size[y]
                    cal(y, x)

        dfs(0, -1, 0)
        reroot(0, -1)
        return ans
```

```Go
```

