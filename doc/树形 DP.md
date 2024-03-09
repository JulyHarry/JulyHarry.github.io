# 树形 DP

# 选与不选

## [337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/)

> 小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 `root` 。
>
> 除了 `root` 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 **两个直接相连的房子在同一天晚上被打劫** ，房屋将自动报警。
>
> 给定二叉树的 `root` 。返回 ***在不触动警报的情况下** ，小偷能够盗取的最高金额* 。
>
>  
>
> **示例 1:**
>
> <img src="https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg" alt="img" style="zoom: 50%;" />
>
> ```
> 输入: root = [3,2,3,null,3,null,1]
> 输出: 7 
> 解释: 小偷一晚能够盗取的最高金额 3 + 3 + 1 = 7
> ```
>
> **示例 2:**
>
> <img src="https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg" alt="img" style="zoom:50%;" />
>
> ```
> 输入: root = [3,4,5,1,3,null,1]
> 输出: 9
> 解释: 小偷一晚能够盗取的最高金额 4 + 5 = 9
> ```
>
>  
>
> **提示：**
>
> 
>
> - 树的节点数在 `[1, 104]` 范围内
> - `0 <= Node.val <= 104`

### 方法

树形 DP

### 思路

父节点 选择 / 不选择 ?

选择父节点: 那么其子节点不能选择, 还要加上根节点的值

不选择父节点: 其子节点可选可不选, 根据最大值判断

所以状态转移方程为

$dfs[node] = \begin{cases} dfs(node.left)[not] + dfs(node.right)[not] + node.val, if\;rob\;node\\\\max(dfs(node.left)[rob], dfs(node.left)[not])+max(dfs(node.right)[rob], dfs(node.right)[not]) \end{cases}$​



边界条件

空节点  $dfs(node) = 0, 0$​

返回值

$max(dfs(root)[rob], dfs(root)[not])$

### 代码

```Python
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        def dfs(root):
            # 返回: [选择此节点, 不选择此节点]
            if not root:
                return 0, 0
            l = dfs(root.left)
            r = dfs(root.right)
            return root.val + l[1] + r[1], max(l[0], l[1]) + max(r[0], r[1])

        return max(dfs(root))
```

```Go
func rob(root *TreeNode) int {
	var dfs func(*TreeNode) []int
	dfs = func(node *TreeNode) []int {
		if node == nil {
			return []int{0, 0}
		}
		l, r := dfs(node.Left), dfs(node.Right)
		return []int{l[1] + r[1] + node.Val, max(l[0], l[1]) + max(r[0], r[1])}
	}
	a := dfs(root)
	return max(a[0], a[1])
}
```



## P1352 没有上司的舞会

> ## 题目描述
>
> 某大学有 $n$ 个职员，编号为 $1\ldots n$。
>
> 他们之间有从属关系，也就是说他们的关系就像一棵以校长为根的树，父结点就是子结点的直接上司。
>
> 现在有个周年庆宴会，宴会每邀请来一个职员都会增加一定的快乐指数 $r_i$，但是呢，如果某个职员的直接上司来参加舞会了，那么这个职员就无论如何也不肯来参加舞会了。
>
> 所以，请你编程计算，邀请哪些职员可以使快乐指数最大，求最大的快乐指数。
>
> ## 输入格式
>
> 输入的第一行是一个整数 $n$。
>
> 第 $2$ 到第 $(n + 1)$ 行，每行一个整数，第 $(i+1)$ 行的整数表示 $i$ 号职员的快乐指数 $r_i$。
>
> 第 $(n + 2)$ 到第 $2n$ 行，每行输入一对整数 $l, k$，代表 $k$ 是 $l$ 的直接上司。
>
> ## 输出格式
>
> 输出一行一个整数代表最大的快乐指数。
>
> ## 样例 #1
>
> ### 样例输入 #1
>
> ```
> 7
> 1
> 1
> 1
> 1
> 1
> 1
> 1
> 1 3
> 2 3
> 6 4
> 7 4
> 4 5
> 3 5
> ```
>
> ### 样例输出 #1
>
> ```
> 5
> ```
>
> ## 提示
>
> #### 数据规模与约定
>
> 对于 $100\%$ 的数据，保证 $1\leq n \leq 6 \times 10^3$，$-128 \leq r_i\leq 127$，$1 \leq l, k \leq n$，且给出的关系一定是一棵树。

### 方法

### 思路

同 [337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/) 

由二叉树变为多叉树, 方法是一致的.

### 代码

```Python
import sys

sys.setrecursionlimit(100000)
n = int(input())
happy = [0] * (n + 1)
for i in range(n):
    happy[i + 1] = int(input())
g = [[] for _ in range(n + 1)]
for i in range(n - 1):
    m = input().split()
    x, y = int(m[0]), int(m[1])
    g[x].append(y)
    g[y].append(x)


def dfs(i, fa):
    # 返回: 选择, 不选择
    gosum, nosum = happy[i], 0
    for j in g[i]:
        if j != fa:
            go, no = dfs(j, i)
            gosum += no
            nosum += max(go, no)
    return gosum, nosum


print(max(dfs(1, -1)))
```



## [1377. T 秒后青蛙的位置](https://leetcode.cn/problems/frog-position-after-t-seconds/)

> 给你一棵由 n 个顶点组成的无向树，顶点编号从 1 到 n。青蛙从 顶点 1 开始起跳。规则如下：
>
> 在一秒内，青蛙从它所在的当前顶点跳到另一个 未访问 过的顶点（如果它们直接相连）。
> 青蛙无法跳回已经访问过的顶点。
> 如果青蛙可以跳到多个不同顶点，那么它跳到其中任意一个顶点上的机率都相同。
> 如果青蛙不能跳到任何未访问过的顶点上，那么它每次跳跃都会停留在原地。
> 无向树的边用数组 edges 描述，其中 edges[i] = [ai, bi] 意味着存在一条直接连通 ai 和 bi 两个顶点的边。
>
> 返回青蛙在 t 秒后位于目标顶点 target 上的概率。与实际答案相差不超过 10-5 的结果将被视为正确答案。
>
>  
>
> 示例 1：
>
> <img src="https://assets.leetcode.com/uploads/2021/12/21/frog1.jpg" style="zoom:50%;" />
>
> 输入：n = 7, edges = [[1,2],[1,3],[1,7],[2,4],[2,6],[3,5]], t = 2, target = 4
> 输出：0.16666666666666666 
> 解释：上图显示了青蛙的跳跃路径。青蛙从顶点 1 起跳，第 1 秒 有 1/3 的概率跳到顶点 2 ，然后第 2 秒 有 1/2 的概率跳到顶点 4，因此青蛙在 2 秒后位于顶点 4 的概率是 1/3 * 1/2 = 1/6 = 0.16666666666666666 。 
> 示例 2：
>
> <img src="https://assets.leetcode.com/uploads/2021/12/21/frog2.jpg" style="zoom:50%;" />
>
> 输入：n = 7, edges = [[1,2],[1,3],[1,7],[2,4],[2,6],[3,5]], t = 1, target = 7
> 输出：0.3333333333333333
> 解释：上图显示了青蛙的跳跃路径。青蛙从顶点 1 起跳，有 1/3 = 0.3333333333333333 的概率能够 1 秒 后跳到顶点 7 。 
>
> 
>
>
> 提示：
>
> 1 <= n <= 100
> edges.length == n - 1
> edges[i].length == 2
> 1 <= ai, bi <= n
> 1 <= t <= 50
> 1 <= target <= n

### 方法

树形 DP

### 思路

青蛙到了一个没有 target 的子树, 还能到达 target 吗? 不会的,因为无法回头

### 考虑自顶向下

从父节点出发, 每个子节点的概率是 $\frac{1}{child\_cnt}$ 

* 如果遇到 $target$ , 如果时间没有了, 或者是叶子节点, 可以直接返回概率
* 否则, 遇到 $target$ 但不是叶子节点, 时间还有剩余, 或者没有时间, 那么都无法到达
* DFS 遍历整棵树, 当 y 是 x 的子节点, 且 target 在其子树中时, 才继续递归

### 细节

* 小数的精度: 可以采用相乘 $child\_cnt$, 最后计算 $\frac{1}{p}$ 得到
* <span style="color:red">顶点 $1$ 可以加个 $0$ 号邻居, 避免特判 $n==1$ 的情况</span>



### 代码

```Python
# 自顶向下
class Solution:
    def frogPosition(self, n: int, edges: List[List[int]], t: int, target: int) -> float:
        g = [[] for _ in range(n + 1)]
        g[1] = [0] # 减少额外判断的小技巧
        for x, y in edges:
            g[x].append(y)
            g[y].append(x)

        def dfs(x, fa, p, t):
            if target == x and (t == 0 or len(g[x]) == 1):
                nonlocal ans
                ans = 1 / p
                return True
            if target == x or t == 0:
                return False
            for y in g[x]:
                if y != fa and dfs(y, x, p * (len(g[x]) - 1), t - 1):
                    return True
            return False

        ans = 0
        dfs(1, 0, 1, t)
        return ans
```

```Python
# 自底向上
class Solution:
    def frogPosition(self, n: int, edges: List[List[int]], t: int, target: int) -> float:
        g = [[] for _ in range(n + 1)]
        g[1] = [0]  # 减少额外判断的小技巧
        for x, y in edges:
            g[x].append(y)
            g[y].append(x)  # 建树

        def dfs(x: int, fa: int, left_t: int) -> int:
            # t 秒后必须在 target（恰好到达，或者 target 是叶子停在原地）
            if left_t == 0: return x == target
            if x == target: return len(g[x]) == 1
            for y in g[x]:  # 遍历 x 的儿子 y
                if y != fa:  # y 不能是父节点
                    prod = dfs(y, x, left_t - 1)  # 寻找 target
                    if prod: return prod * (len(g[x]) - 1)  # 乘上儿子个数，并直接返回
            return 0  # 未找到 target

        prod = dfs(1, 0, t)
        return 1 / prod if prod else 0
```



## [2646. 最小化旅行的价格总和](https://leetcode.cn/problems/minimize-the-total-price-of-the-trips/)

> 现有一棵无向、无根的树，树中有 `n` 个节点，按从 `0` 到 `n - 1` 编号。给你一个整数 `n` 和一个长度为 `n - 1`的二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示树中节点 `ai` 和 `bi` 之间存在一条边。
>
> 每个节点都关联一个价格。给你一个整数数组 `price` ，其中 `price[i]` 是第 `i` 个节点的价格。
>
> 给定路径的 **价格总和** 是该路径上所有节点的价格之和。
>
> 另给你一个二维整数数组 `trips` ，其中 `trips[i] = [starti, endi]` 表示您从节点 `starti` 开始第 `i` 次旅行，并通过任何你喜欢的路径前往节点 `endi` 。
>
> 在执行第一次旅行之前，你可以选择一些 **非相邻节点** 并将价格减半。
>
> 返回执行所有旅行的最小价格总和。
>
>  
>
> **示例 1：**
>
> <img src="https://assets.leetcode.com/uploads/2023/03/16/diagram2.png" alt="img" style="zoom:50%;" />
>
> ```
> 输入：n = 4, edges = [[0,1],[1,2],[1,3]], price = [2,2,10,6], trips = [[0,3],[2,1],[2,3]]
> 输出：23
> 解释：
> 上图表示将节点 2 视为根之后的树结构。第一个图表示初始树，第二个图表示选择节点 0 、2 和 3 并使其价格减半后的树。
> 第 1 次旅行，选择路径 [0,1,3] 。路径的价格总和为 1 + 2 + 3 = 6 。
> 第 2 次旅行，选择路径 [2,1] 。路径的价格总和为 2 + 5 = 7 。
> 第 3 次旅行，选择路径 [2,1,3] 。路径的价格总和为 5 + 2 + 3 = 10 。
> 所有旅行的价格总和为 6 + 7 + 10 = 23 。可以证明，23 是可以实现的最小答案。
> ```
>
> **示例 2：**
>
> <img src="https://assets.leetcode.com/uploads/2023/03/16/diagram3.png" alt="img" style="zoom:50%;" />
>
> ```
> 输入：n = 2, edges = [[0,1]], price = [2,2], trips = [[0,0]]
> 输出：1
> 解释：
> 上图表示将节点 0 视为根之后的树结构。第一个图表示初始树，第二个图表示选择节点 0 并使其价格减半后的树。 
> 第 1 次旅行，选择路径 [0] 。路径的价格总和为 1 。 
> 所有旅行的价格总和为 1 。可以证明，1 是可以实现的最小答案。
> ```
>
>  
>
> **提示：**
>
> - `1 <= n <= 50`
> - `edges.length == n - 1`
> - `0 <= ai, bi <= n - 1`
> - `edges` 表示一棵有效的树
> - `price.length == n`
> - `price[i]` 是一个偶数
> - `1 <= price[i] <= 1000`
> - `1 <= trips.length <= 100`
> - `0 <= starti, endi <= n - 1`

### 方法

DFS + 树形 DP

### 思路

问题拆解:  答案返回所有旅行的最小价格总和, 因此不用单独算每个 $trip_i$ 的结果再累加.

根据 $trips$ 我们可以通过 $DFS$ 统计每一个节点通过的次数, 那么这个节点的值就变成了 $price[i] \times cnt[i]$​ .

接下来的问题类似于 [337. 打家劫舍 III](##337. 打家劫舍 III)  [P1352 没有上司的舞会](##P1352 没有上司的舞会) , 转化为计算不能父节点和子节点同时取半后的最小值.

 $DFS$ 统计每一个节点通过的次数 类似 [1377. T 秒后青蛙的位置](##1377. T 秒后青蛙的位置) 的自定向下的方法.

### 代码

```Python
class Solution:
    def minimumTotalPrice(self, n: int, edges: List[List[int]], price: List[int], trips: List[List[int]]) -> int:
        g = [[] for _ in range(n)]
        for x, y in edges:
            g[x].append(y)
            g[y].append(x)

        cnt = [0] * n
        for s, e in trips:
            def dfs(x, fa):
                if x == e:
                    cnt[x] += 1
                    return True
                for y in g[x]:
                    if y != fa and dfs(y, x):
                        cnt[x] += 1
                        return True
                return False
            dfs(s, -1)

        def dfs(x, fa):
            # 返回: 不减半, 减半
            no_hf = price[x] * cnt[x]
            hf = no_hf // 2
            for y in g[x]:
                if y != fa:
                    n, h = dfs(y, x)
                    no_hf += min(n, h)
                    hf += n
            return no_hf, hf
        
        return min(dfs(0, -1))
```

```Go

```





## [968. 监控二叉树](https://leetcode.cn/problems/binary-tree-cameras/)

> 给定一个二叉树，我们在树的节点上安装摄像头。
>
> 节点上的每个摄影头都可以监视**其父对象、自身及其直接子对象。**
>
> 计算监控树的所有节点所需的最小摄像头数量。
>
>  
>
> **示例 1：**
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/29/bst_cameras_01.png)
>
> ```
> 输入：[0,0,null,0,0]
> 输出：1
> 解释：如图所示，一台摄像头足以监控所有节点。
> ```
>
> **示例 2：**
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/29/bst_cameras_02.png)
>
> ```
> 输入：[0,0,null,0,null,0,null,null,0]
> 输出：2
> 解释：需要至少两个摄像头来监视树的所有节点。 上图显示了摄像头放置的有效位置之一。
> ```
>
> 
> **提示：**
>
> 1. 给定树的节点数的范围是 `[1, 1000]`。
> 2. 每个节点的值都是 0。

### 方法

#### 遍历方式

为了使整棵树能安装监控的最少，叶子节点不应该放置监控(贪心)， 因此应该从下往上进行遍历，即**二叉树的后序遍历**

#### 状态

会有三种状态

* $choose$：安装
* $by\_fa$：不安装但父节点安装
* $by\_child$：不安装但至少有一个子节点安装

#### 转移方程
$choose$：子节点可以是任意一种状态, 所以取左、右子节点的值取各个状态的最小值，求和后再加上当前节点因安装的1，即该节点**安装**状态下的最小值。

$choose = min(l\_choose, l\_by\_fa, l\_by\_child) + min(r\_choose, r\_by\_fa, r\_by\_child) + 1$

$by\_fa$：子节点只能是 $choose$, $by\_child$

$by\_fa = min(l\_choose, l\_by\_child) + min(r\_choose, r\_by\_child)$

$by\_child$：子节点只能是 $choose$, $by\_child$, 且**两个子节点至少有一个是$choose$**。

$by\_child = min(min(l\_choose, r\_by\_child), min(l\_by\_child, r\_choose), min(l\_choose, r\_choose))$

#### 边界条件

空节点肯定不能放置监控，因此$choose$ 的初始值为无穷大

我们**为了使整棵树能安装监控的最少，叶子节点不应该放置监控**，监控应该放在叶子节点的父节点中，叶子节点的子节点都是空节点，因此空节点的 $by\_fa$ 状态应该是无穷大，来避免选择此状态。

空节点的子节点不会防止监控，因此为 $0$。

因此初始值为：$ choose =  \infty , by\_fa = \infty, by\_child = 0$

#### 返回值

根节点不会存在 $by\_fa$ 状态， 因此返回 $choose$ 和 $by\_child$ 的最小值

### 代码

```Python []
class Solution:
    def minCameraCover(self, root: Optional[TreeNode]) -> int:
        def dfs(node):
            if not node:
                return float('inf'), 0, 0
            l_choose, l_by_fa, l_by_child = dfs(node.left)
            r_choose, r_by_fa, r_by_child = dfs(node.right)
            choose = min(l_choose, l_by_fa, l_by_child) + min(r_choose, r_by_fa, r_by_child) + 1
            by_fa = min(l_choose, l_by_child) + min(r_choose, r_by_child)
            by_child = min(l_choose+r_by_child, l_choose+r_choose, l_by_child+r_choose)
            return choose, by_fa, by_child
        choose, _, by_child = dfs(root)
        return min(choose, by_child)
```



### 复杂度

#### 时间复杂度

$O(n)$

#### 空间复杂度

$O(n)$

### 变形1

#### 题干
如果每个节点的安装费用是 $cost[i]$ ，求安装费用的最小值

#### 解决方案
$choose = min(l\_choose, l\_by\_fa, l\_by\_child) + min(r\_choose, r\_by\_fa, r\_by\_child) + cost[i]$

### 变形2

#### 题干
如果二叉树换为多叉树，如何求取 (**点覆盖问题**)

#### 解决方案
如果换为多叉树($n$叉)， 那么 $by\_child$：子节点只能是 $choose$, $by\_child$, 且 $n$ 个子节点至少有一个是$choose$。

将需要列举 $2^n - 1$ 个

以 $n = 3$ 为例

$by\_child = min\begin{cases}choose1, choose2, choose3\\child1, choose2, choose3\\choose1, child2, choose3\\choose1, choose2, child3\\child1, child2, choose3\\child1, choose2, child3\\choose1, child2, child3\end{cases} \stackrel{compare}{\Longleftrightarrow}min(choose1, child1)+min(choose2, child2)+min(choose3, child3)$ 

右侧表达式只有在 $child_i < choose_i, i \in [1, n]$ 的情况下，才会出现 $child1, child2, child3$ 这一个不在方案中的情况， 因此我们可以在右侧表达式中加一个 $min(choose_i-child_i), i \in [1, n]$ 即可, 如果 $min(choose_i-child_i) < 0$, 说明不会出现 $child1, child2, child3$ 的情况， 因此不用加该值。

因此方程为

$by\_fa = min(choose1, child1) + min(choose2, child2) + min(choose3, child3)$

$by\_child = by\_fa + max(0, min(choose1-child1, choose2-child2, choose3-child3))$

由于 $by\_child > by\_fa$, 因此下列等式改为

$choose = min(choose1, fa1) + min(choose2, fa2) + min(choose3, fa3) + 1$

例题详见 [P2458 保安站岗]()



## [P2458 [SDOI2006] 保安站岗](https://www.luogu.com.cn/problem/P2458)

> ## 题目描述
>
> 五一来临，某地下超市为了便于疏通和指挥密集的人员和车辆，以免造成超市内的混乱和拥挤，准备临时从外单位调用部分保安来维持交通秩序。
>
> 已知整个地下超市的所有通道呈一棵树的形状；某些通道之间可以互相望见。总经理要求所有通道的每个端点（树的顶点）都要有人全天候看守，在不同的通道端点安排保安所需的费用不同。
>
> 一个保安一旦站在某个通道的其中一个端点，那么他除了能看守住他所站的那个端点，也能看到这个通道的另一个端点，所以一个保安可能同时能看守住多个端点（树的结点），因此没有必要在每个通道的端点都安排保安。
>
> 编程任务：
>
> 请你帮助超市经理策划安排，在能看守全部通道端点的前提下，使得花费的经费最少。
>
> ## 输入格式
>
> 第 $1$ 行 $n$，表示树中结点的数目。
>
> 第 $2$ 行至第 $n+1$ 行，每行描述每个通道端点的信息，依次为：该结点标号 $i$（$0<i \le n$），在该结点安置保安所需的经费 $k$（$ \le 10000$），该边的儿子数 $m$，接下来 $m$ 个数，分别是这个节点的 $m$ 个儿子的标号 $r_1,r_2,\cdots, r_m$。
>
> 对于一个 $n$（$0<n \le 1500$）个结点的树，结点标号在 $1$ 到 $n$ 之间，且标号不重复。
>
> ## 输出格式
>
> 输出一行一个整数，表示最少的经费。
>
> ## 样例 #1
>
> ### 样例输入 #1
>
> ```
> 6
> 1 30 3 2 3 4
> 2 16 2 5 6
> 3 5 0
> 4 4 0
> 5 11 0
> 6 5 0
> ```
>
> ### 样例输出 #1
>
> ```
> 25
> ```
>
> ## 提示
>
> ### 样例解释
>
> <img src="https://cdn.luogu.com.cn/upload/image_hosting/g013tlmh.png" style="zoom:25%;" />
>
> 在结点 $2,3,4$ 安置 $3$ 个保安能看守所有的 $6$ 个结点，需要的经费最小：$25$。

### 方法

### 思路

### 代码

```Python

```

```Go

```



## [LCP 34. 二叉树染色](https://leetcode.cn/problems/er-cha-shu-ran-se-UGC/)

> 小扣有一个根结点为 `root` 的二叉树模型，初始所有结点均为白色，可以用蓝色染料给模型结点染色，模型的每个结点有一个 `val` 价值。小扣出于美观考虑，希望最后二叉树上每个蓝色相连部分的结点个数不能超过 `k` 个，求所有染成蓝色的结点价值总和最大是多少？
>
> **示例 1：**
>
> > 输入：`root = [5,2,3,4], k = 2`
> >
> > 输出：`12`
> >
> > 解释：`结点 5、3、4 染成蓝色，获得最大的价值 5+3+4=12`![image.png](https://pic.leetcode-cn.com/1616126267-BqaCRj-image.png)
>
> **示例 2：**
>
> > 输入：`root = [4,1,3,9,null,null,2], k = 2`
> >
> > 输出：`16`
> >
> > 解释：结点 4、3、9 染成蓝色，获得最大的价值 4+3+9=16![image.png](https://pic.leetcode-cn.com/1616126301-gJbhba-image.png)
>
> **提示：**
>
> - `1 <= k <= 10`
> - `1 <= val <= 10000`
> - `1 <= 结点数量 <= 10000`

### 方法

树形 DP

### 思路

$dfs(x)[i]$ 根节点为 $x$ 时，已经涂色 $i$ 个，可以获得的最大价值

如果 $x$ 不涂色， 那么可以直径计算 左子树的最大值 和 右子树的最大值
$dfs(x)[0] = max(l) + max(r)$

如果 $x$ 涂色，要循环遍历左右子树， 如左子树有 $i$ 个， 右子树有 $j$ 个， 如果 $i +j < k$， 那么
$dfs[x][i+j+1] = max(-dfs[x][i+j+1], l[i] + r[j] +x.val)$

边界值
空节点  $[0, 0, ...., 0]$

返回值
$max(dfs(x))$

### 代码

```Python
class Solution:
    def maxValue(self, root: TreeNode, k: int) -> int:

        def dfs(node):
            f = [0] * (k + 1)
            if not node:
                return f

            l = dfs(node.left)
            r = dfs(node.right)

            # 染色
            for i in range(k):
                for j in range(k - i):
                    f[i + j + 1] = max(f[i + j + 1], l[i] + r[j] + node.val)

            # 不染色
            f[0] = max(f[0], max(l) + max(r))
            return f

        return max(dfs(root))
```

```Go

```



## [LCP 64. 二叉树灯饰](https://leetcode.cn/problems/U7WvvU/)

> 「力扣嘉年华」的中心广场放置了一个巨型的二叉树形状的装饰树。每个节点上均有一盏灯和三个开关。节点值为 `0` 表示灯处于「关闭」状态，节点值为 `1` 表示灯处于「开启」状态。每个节点上的三个开关各自功能如下：
>
> - 开关 `1`：切换当前节点的灯的状态；
> - 开关 `2`：切换 **以当前节点为根** 的子树中，所有节点上的灯的状态；
> - 开关 `3`：切换 **当前节点及其左右子节点**（若存在的话） 上的灯的状态；
>
> 给定该装饰的初始状态 `root`，请返回最少需要操作多少次开关，可以关闭所有节点的灯。
>
> **示例 1：**
>
> <img src="https://pic.leetcode-cn.com/1629357030-GSbzpY-b71b95bf405e3b223e00b2820a062ba4.gif" style="zoom:50%;" />
>
> ```
> 输入：root = [1,1,0,null,null,null,1]
> 输出：2
> 解释：以下是最佳的方案之一，如图所示
> ```
>
> **示例 2：**
>
> <img src="https://pic.leetcode-cn.com/1629356950-HZsKZC-a4091b6448a0089b4d9e8f0390ff9ac6.gif" style="zoom:50%;" />
>
> ```
> 输入：root = [1,1,1,1,null,null,1]
> 输出：1
> 解释：以下是最佳的方案，如图所示
> ```
>
> **示例 3：**
>
> ```
> 输入：root = [0,null,0]
> 输出：0
> 解释：无需操作开关，当前所有节点上的灯均已关闭
> ```
>
> **提示：**
>
> - `1 <= 节点个数 <= 10^5`
> - `0 <= Node.val <= 1`

### 方法

树形 DP

### 思路

#### 一个节点, 除自身外, 还会受到哪些状态的影响?

- 会受到开关 2 的祖先节点影响: 如果祖先节点执行偶数次, 那么相当于不变, 奇数次相当于改变, 因此可以只用一个 bool 值表示
- 会受到开关 3 父节点的影响: 也可以使用 bool 值表示.

因此可以定义状态为 $(node, switch2\_odd, switch3)$​

#### 哪些情况开关会是开的?

- 节点本身是开的  $node.val == 1$
  - 开关 2 次数偶数 $switch2\_odd == false$,  开关 3 关闭 $switch3 == false$
  - 开关 2 次数奇数 $switch2\_odd == true$,  开关 3 开启 $switch3 == true$​​

* 节点本身是关的  $node.val == 0$

  - 开关 2 次数偶数 $switch2\_odd == false$,  开关 3 关闭 $switch3 == true$

  - 开关 2 次数奇数 $switch2\_odd == true$,  开关 3 关闭 $switch3 == false$​​

以上情况可以概况为  $(node.val == 1) \wedge (switch2\_odd==switch3)$​

定义 $dfs(node)$ 使 $node$ 关闭时, 最小操作次数

#### 转移方程

- 开关是开的时候, 使其关闭, 可以使用三个开关各一次或者三个开关一起使用

##### 使用开关 1

$res1 = dfs(node.left, switch2\_odd, false) + dfs(node.right, switch2\_odd, false) + 1$​

##### 使用开关 2

$res2 = dfs(node.left, \neg switch2\_odd, false) + dfs(node.right, \neg switch2\_odd, false) + 1$

##### 使用开关 3

$res3 = dfs(node.left, switch2\_odd, true) + dfs(node.right, switch2\_odd, true) + 1$

##### 使用开关 123

$res123 = dfs(node.left, \neg switch2\_odd, true) + dfs(node.right, \neg switch2\_odd, true) + 3$​

- 开关是关的时候, 使其保持关闭, 可以使用三个开关的任意 2 个或者三个开关都不使用

##### 使用开关 12

$res12 = dfs(node.left, \neg switch2\_odd, false) + dfs(node.right, \neg switch2\_odd, false) + 2$​

##### 使用开关 13

$res13 = dfs(node.left, switch2\_odd, true) + dfs(node.right, switch2\_odd, true) + 2$

##### 使用开关 23

$res23 = dfs(node.left, \neg switch2\_odd, true) + dfs(node.right, \neg switch2\_odd, true) + 2$

##### 不使用任何开关

$res0 = dfs(node.left, switch2\_odd, false) + dfs(node.right, switch2\_odd, false)$​

#### 边界值

空节点 $0$

#### 返回值

$dfs(root)$​



需要记忆化搜索

### 代码

```Python
class Solution:
    def closeLampInTree(self, root: TreeNode) -> int:
        @cache
        def dfs(node, s2, s3):
            if not node:
                return 0
            if (node.val == 1) == (s2 == s3):
                r1 = dfs(node.left, s2, False) + dfs(node.right, s2, False) + 1
                r2 = dfs(node.left, not s2, False) + dfs(node.right, not s2, False) + 1
                r3 = dfs(node.left, s2, True) + dfs(node.right, s2, True) + 1
                r123 = dfs(node.left, not s2, True) + dfs(node.right, s2, True) + 3
                return min(r1, r2, r3, r123)
            else:
                r12 = dfs(node.left, not s2, False) + dfs(node.right, not s2, False) + 2
                r13 = dfs(node.left, s2, True) + dfs(node.right, s2, True) + 2
                r23 = dfs(node.left, not s2, True) + dfs(node.right, not s2, True) + 2
                r0 = dfs(node.left, s2, False) + dfs(node.right, s2, False)
                return min(r12, r13, r23, r0)

        return dfs(root, False, False)
```

### 复杂度分析

#### 时间复杂度

 $O(n)$​ 

<span style="color:red">动态规划的时间复杂度 = 状态个数 * 单个状态的转移个数</span>

状态个数 $n * 2 * 2$  (node 的状态 n 个, s2 状态 2 个, s3 状态 2 个)

单个状态的转移个数 $8$

因此时间复杂度 $O(32*n) = O(n)$

#### 空间复杂度

$O(n)$



# 最大路径和

## [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)

> 给你一棵二叉树的根节点，返回该树的 **直径** 。
>
> 二叉树的 **直径** 是指树中任意两个节点之间最长路径的 **长度** 。这条路径可能经过也可能不经过根节点 `root` 。
>
> 两节点之间路径的 **长度** 由它们之间边数表示。
>
>  
>
> **示例 1：**
>
> ![img](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)
>
> ```
> 输入：root = [1,2,3,4,5]
> 输出：3
> 解释：3 ，取路径 [4,2,1,3] 或 [5,2,1,3] 的长度。
> ```
>
> **示例 2：**
>
> ```
> 输入：root = [1,2]
> 输出：1
> ```
>
>  
>
> **提示：**
>
> - 树中节点数目在范围 `[1, 104]` 内
> - `-100 <= Node.val <= 100`

### 方法

树形 DP

### 思路

$dfs(x)$ 是以 $x$ 为根节点到子树中最远的叶子结点的长度

所以 $dfs(x) = max(dfs(x.left), dfs(x.right)) +1$

最长的直径， 应该在遍历时计算 $dfs(x.left) +dfs(x.right)$ 的最大值

### 代码

```Python
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        def dfs(node):
            nonlocal ans
            if not node:
                return 0
            l = dfs(node.left)
            r = dfs(node.right)
            ans = max(ans, l + r)
            return max(l, r) + 1

        ans = 0
        dfs(root)
        return ans
```

```Go

```



## [124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/)

> 二叉树中的 **路径** 被定义为一条节点序列，序列中每对相邻节点之间都存在一条边。同一个节点在一条路径序列中 **至多出现一次** 。该路径 **至少包含一个** 节点，且不一定经过根节点。
>
> **路径和** 是路径中各节点值的总和。
>
> 给你一个二叉树的根节点 `root` ，返回其 **最大路径和** 。
>
>  
>
> **示例 1：**
>
> ![img](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)
>
> ```
> 输入：root = [1,2,3]
> 输出：6
> 解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6
> ```
>
> **示例 2：**
>
> ![img](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)
>
> ```
> 输入：root = [-10,9,20,null,null,15,7]
> 输出：42
> 解释：最优路径是 15 -> 20 -> 7 ，路径和为 15 + 20 + 7 = 42
> ```
>
>  
>
> **提示：**
>
> - 树中节点数目范围是 `[1, 3 * 104]`
> - `-1000 <= Node.val <= 1000`

### 方法

树形 DP

### 思路

同 543. 二叉树的直径

$dfs(x)$ 是以 $x$ 为根节点的最大路径和， 路径和由根节点的值、左子树的值、右子树的值共同组成。

所以 $dfs(x) = max(dfs(x.left), dfs(x.right)) +x.val $

为保证当前根节点的路径和最大， 如果左右子树的值小于 0 ， 我们需舍弃。

最长的直径， 应该在遍历时计算 $max(dfs(x.left), 0) +max(dfs(x.right), 0) + x.val $ 的最大值

### 代码

```Python
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        def dfs(node):
            if not node:
                return 0
            l, r = max(dfs(node.left), 0), max(dfs(node.right), 0)
            nonlocal ans
            ans = max(ans, l + r + node.val)
            return max(l, r) + node.val

        ans = -inf
        dfs(root)
        return ans
```



## [2246. 相邻字符不同的最长路径](https://leetcode.cn/problems/longest-path-with-different-adjacent-characters/) [2126]

> 给你一棵 **树**（即一个连通、无向、无环图），根节点是节点 `0` ，这棵树由编号从 `0` 到 `n - 1` 的 `n` 个节点组成。用下标从 **0** 开始、长度为 `n` 的数组 `parent` 来表示这棵树，其中 `parent[i]` 是节点 `i` 的父节点，由于节点 `0` 是根节点，所以 `parent[0] == -1` 。
>
> 另给你一个字符串 `s` ，长度也是 `n` ，其中 `s[i]` 表示分配给节点 `i` 的字符。
>
> 请你找出路径上任意一对相邻节点都没有分配到相同字符的 **最长路径** ，并返回该路径的长度。
>
>  
>
> **示例 1：**
>
> ![img](https://assets.leetcode.com/uploads/2022/03/25/testingdrawio.png)
>
> ```
> 输入：parent = [-1,0,0,1,1,2], s = "abacbe"
> 输出：3
> 解释：任意一对相邻节点字符都不同的最长路径是：0 -> 1 -> 3 。该路径的长度是 3 ，所以返回 3 。
> 可以证明不存在满足上述条件且比 3 更长的路径。 
> ```
>
> **示例 2：**
>
> ![img](https://assets.leetcode.com/uploads/2022/03/25/graph2drawio.png)
>
> ```
> 输入：parent = [-1,0,0,0], s = "aabc"
> 输出：3
> 解释：任意一对相邻节点字符都不同的最长路径是：2 -> 0 -> 3 。该路径的长度为 3 ，所以返回 3 。
> ```
>
>  
>
> **提示：**
>
> - `n == parent.length == s.length`
> - `1 <= n <= 105`
> - 对所有 `i >= 1` ，`0 <= parent[i] <= n - 1` 均成立
> - `parent[0] == -1`
> - `parent` 表示一棵有效的树
> - `s` 仅由小写英文字母组成

### 方法

树形 DP

### 思路

通过 $parent$ 数组，可以直接得到不包含父节点的邻接图。

定义 $dfs(x)$ 是经过 $x$ 可以得到的最长路径

由于$x$ 有多个子节点， 路径只是两个节点相连的， 因此可以记录 $x$ 中最长的路径长度和第二长的路径长度， 或者说经过 $x$ 的路径的最大值， 应该从其两个最大的子树中得到。

$dfs$ 时， 得到的是边的长度， 答案要求是节点数量，因此答案=边+1

任意一对相邻节点都没有分配到相同字符， 只需要判断 x 和其子节点的字符不同即可， 不同不计算答案， 但是 $dfs(y)$ 是需要的， 因为有可能最长路径在子节点中。

### 代码

```Python
class Solution:
    def longestPath(self, parent: List[int], s: str) -> int:
        n = len(parent)
        g = [[] for _ in range(n)]
        for i, p in enumerate(parent[1:], 1):
            g[p].append(i)

        def dfs(x):
            x_len = 0
            for y in g[x]:
                y_len = dfs(y) + 1
                if s[x] == s[y]:
                    continue
                nonlocal ans
                ans = max(ans, x_len + y_len)
                x_len = max(x_len, y_len)
            return x_len

        ans = 0
        dfs(0)
        return ans+1
```



## [687. 最长同值路径 ](https://leetcode.cn/problems/longest-univalue-path/)

> 给定一个二叉树的 `root` ，返回 *最长的路径的长度* ，这个路径中的 *每个节点具有相同值* 。 这条路径可以经过也可以不经过根节点。
>
> **两个节点之间的路径长度** 由它们之间的边数表示。
>
>  
>
> **示例 1:**
>
> ![img](https://assets.leetcode.com/uploads/2020/10/13/ex1.jpg)
>
> ```
> 输入：root = [5,4,5,1,1,5]
> 输出：2
> ```
>
> **示例 2:**
>
> ![img](https://assets.leetcode.com/uploads/2020/10/13/ex2.jpg)
>
> ```
> 输入：root = [1,4,5,4,4,5]
> 输出：2
> ```
>
>  
>
> **提示:**
>
> - 树的节点数的范围是 `[0, 104]` 
> - `-1000 <= Node.val <= 1000`
> - 树的深度将不超过 `1000` 

### 方法

树形 DP

### 思路

同 [543. 二叉树的直径](##543. 二叉树的直径) 只不过增加了父节点和子节点值相等的条件

$dfs(x)$ 是以 $x$​ 为根节点, 子节点和根节点值相同, 到子树中最远的叶子结点的边的个数

$l = dfs(x.left) , r = dfs(x.right)$​ 表示左子树和右子树的同值的边的长度

如果 $x.left.val != x.val$ , $l = 0$, 对 $x.right$ 同理 

所以 $dfs(x) = max(l, r) + 1$

最长的直径， 应该在遍历时计算 $l + r$ 的最大值

### 代码

```Python
class Solution:
    def longestUnivaluePath(self, root: Optional[TreeNode]) -> int:
        def dfs(node):
            if not node:
                return 0
            l, r = dfs(node.left), dfs(node.right)
            l1 = l if node.left and node.left.val == node.val else 0
            r1 = r if node.right and node.right.val == node.val else 0
            nonlocal ans
            ans = max(ans, l1 + r1)
            return max(l1, r1) + 1

        ans = 0
        dfs(root)
        return ans
```

```Go

```





## [1617. 统计子树中城市之间最大距离](https://leetcode.cn/problems/count-subtrees-with-max-distance-between-cities/) [2308]

> 给你 `n` 个城市，编号为从 `1` 到 `n` 。同时给你一个大小为 `n-1` 的数组 `edges` ，其中 `edges[i] = [ui, vi]` 表示城市 `ui` 和 `vi` 之间有一条双向边。题目保证任意城市之间只有唯一的一条路径。换句话说，所有城市形成了一棵 **树** 。
>
> 一棵 **子树** 是城市的一个子集，且子集中任意城市之间可以通过子集中的其他城市和边到达。两个子树被认为不一样的条件是至少有一个城市在其中一棵子树中存在，但在另一棵子树中不存在。
>
> 对于 `d` 从 `1` 到 `n-1` ，请你找到城市间 **最大距离** 恰好为 `d` 的所有子树数目。
>
> 请你返回一个大小为 `n-1` 的数组，其中第 `d` 个元素（**下标从 1 开始**）是城市间 **最大距离** 恰好等于 `d` 的子树数目。
>
> **请注意**，两个城市间距离定义为它们之间需要经过的边的数目。
>
>  
>
> **示例 1：**
>
> **![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/11/p1.png)**
>
> ```
> 输入：n = 4, edges = [[1,2],[2,3],[2,4]]
> 输出：[3,4,0]
> 解释：
> 子树 {1,2}, {2,3} 和 {2,4} 最大距离都是 1 。
> 子树 {1,2,3}, {1,2,4}, {2,3,4} 和 {1,2,3,4} 最大距离都为 2 。
> 不存在城市间最大距离为 3 的子树。
> ```
>
> **示例 2：**
>
> ```
> 输入：n = 2, edges = [[1,2]]
> 输出：[1]
> ```
>
> **示例 3：**
>
> ```
> 输入：n = 3, edges = [[1,2],[2,3]]
> 输出：[2,1]
> ```
>
>  
>
> **提示：**
>
> - `2 <= n <= 15`
> - `edges.length == n-1`
> - `edges[i].length == 2`
> - `1 <= ui, vi <= n`
> - 题目保证 `(ui, vi)` 所表示的边互不相同。

### 方法

回溯 +树形DP

### 思路

#### 回溯

因为我们无法确认当前城市的大小。比如示例1中子树 {1,2,3}, {1,2,4}, {2,3,4} 和 {1,2,3,4} 最大距离都为 2， 我们需要通过枚举， 得知当前的城市是 {2,3,4} 还是 {1,2,3,4}。

经典的选与不选的问题

```
visited = [False] * n
def backtrack(i)):
    if i == n:
		   pass
    # 不选
		backtrack(i+1)
		#选
		visited[i] = True
		bachtrack(i+1)
		visited[i] = False
```

#### 树形 DP

同  计算二叉树树的最大直径

#### 注意

首先子集选出来的点未必是一颗树， 可能是多棵树。因为在计算树形DP时会遍历整棵树， 所以可以将树形DP遍历出的结果和子集结果进行对比，如果一致才记录答案。

虽然利用树形DP求取树的直径只需要遍历任意一个节点即可，但是本题中无法确定哪个节点是在树中的，因此还需要从 in_set 中遍历出一个在树中的节点才可以。

### 代码

```Python []
class Solution:
    def countSubgraphsForEachDiameter(
        self, n: int, edges: List[List[int]]
    ) -> List[int]:
        g = [[] for _ in range(n)]
        for x, y in edges:
            g[x - 1].append(y - 1)
            g[y - 1].append(x - 1)

        in_set = [False] * n
        ans = [0] * (n - 1)

        def backtrack(i):
            if i == n:
                mx = 0
                v = [False] * n
                for x, ok in enumerate(in_set):
                    if not ok:
                        continue

                    def dfs(x):
                        v[x] = True
                        d = 0
                        for y in g[x]:
                            if not v[y] and in_set[y]:
                                cur_d = dfs(y) + 1
                                nonlocal mx
                                mx = max(mx, d + cur_d)
                                d = max(d, cur_d)
                        return d

                    dfs(x)
                    break
                if mx and v == in_set:
                    ans[mx - 1] += 1
                return

            backtrack(i + 1)
            in_set[i] = True
            backtrack(i + 1)
            in_set[i] = False

        backtrack(0)
        return ans
```

```Go

```



## [2538. 最大价值和与最小价值和的差值](https://leetcode.cn/problems/difference-between-maximum-and-minimum-price-sum/) [2397]

> 给你一个 `n` 个节点的无向无根图，节点编号为 `0` 到 `n - 1` 。给你一个整数 `n` 和一个长度为 `n - 1` 的二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示树中节点 `ai` 和 `bi` 之间有一条边。
>
> 每个节点都有一个价值。给你一个整数数组 `price` ，其中 `price[i]` 是第 `i` 个节点的价值。
>
> 一条路径的 **价值和** 是这条路径上所有节点的价值之和。
>
> 你可以选择树中任意一个节点作为根节点 `root` 。选择 `root` 为根的 **开销** 是以 `root` 为起点的所有路径中，**价值和** 最大的一条路径与最小的一条路径的差值。
>
> 请你返回所有节点作为根节点的选择中，**最大** 的 **开销** 为多少。
>
>  
>
> **示例 1：**
>
> ![img](https://assets.leetcode.com/uploads/2022/12/01/example14.png)
>
> ```
> 输入：n = 6, edges = [[0,1],[1,2],[1,3],[3,4],[3,5]], price = [9,8,7,6,10,5]
> 输出：24
> 解释：上图展示了以节点 2 为根的树。左图（红色的节点）是最大价值和路径，右图（蓝色的节点）是最小价值和路径。
> - 第一条路径节点为 [2,1,3,4]：价值为 [7,8,6,10] ，价值和为 31 。
> - 第二条路径节点为 [2] ，价值为 [7] 。
> 最大路径和与最小路径和的差值为 24 。24 是所有方案中的最大开销。
> ```
>
> **示例 2：**
>
> ![img](https://assets.leetcode.com/uploads/2022/11/24/p1_example2.png)
>
> ```
> 输入：n = 3, edges = [[0,1],[1,2]], price = [1,1,1]
> 输出：2
> 解释：上图展示了以节点 0 为根的树。左图（红色的节点）是最大价值和路径，右图（蓝色的节点）是最小价值和路径。
> - 第一条路径包含节点 [0,1,2]：价值为 [1,1,1] ，价值和为 3 。
> - 第二条路径节点为 [0] ，价值为 [1] 。
> 最大路径和与最小路径和的差值为 2 。2 是所有方案中的最大开销。
> ```
>
>  
>
> **提示：**
>
> - `1 <= n <= 105`
> - `edges.length == n - 1`
> - `0 <= ai, bi <= n - 1`
> - `edges` 表示一棵符合题面要求的树。
> - `price.length == n`
> - `1 <= price[i] <= 105`

### 方法

树形 DP

### 思路

选择 root 为根的 开销 是以 root 为起点的所有路径中，价值和 最大的一条路径与最小的一条路径的差值。由于价值都是正数， 且需要以 root 为起点， 因此最小路径和一定是一个路径的端点。

求从根节点出发的最大的路径上去除根节点的价值和 => 求最长价值和的路径去掉两个端点中的一个 => 利用树的最大直径的方法，记录一个节点的包含端点的最大价值和和不包含端点的最大价值

$ni = max(ni, yni + price[x])$ 只有端点才会不记录，非叶子结点的要记录

### 代码

```Python []
class Solution:
    def maxOutput(self, n: int, edges: List[List[int]], price: List[int]) -> int:
        g = [[] for _ in range(n)]
        for x, y in edges:
            g[x].append(y)
            g[y].append(x)

        def dfs(x, fa):
            # 返回： 包含端点的最大路径和 不包含端点的最大路径和
            i, ni = price[x], 0
            for y in g[x]:
                if y != fa:
                    yi, yni = dfs(y, x)
                    nonlocal ans
                    ans = max(ans, ni + yi, i + yni)
                    i = max(i, yi + price[x])
                    ni = max(ni, yni + price[x])
            return i, ni

        ans = 0
        dfs(0, -1)
        return ans
```

```Go

```

