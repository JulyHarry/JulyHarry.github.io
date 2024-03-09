# 平衡树



## 实现

### Python

```python
from sortedcontainers import SortedList

# 初始化
s = SortedList()
# 添加
s.add(100) 
# 删除
s.remove(100)
# 最大值
s[-1]
# 最小值
s[0]         
```



## Go

```Go
import "math/rand"

type node struct {
    ch       [2]*node
    priority int
    key      int
    cnt      int
}

func (o *node) cmp(b int) int {
    switch {
    case b < o.key:
        return 0
    case b > o.key:
        return 1
    default:
        return -1
    }
}

func (o *node) rotate(d int) *node {
    x := o.ch[d^1]
    o.ch[d^1] = x.ch[d]
    x.ch[d] = o
    return x
}

type treap struct {
    root *node
}

func (t *treap) ins(o *node, key int) *node {
    if o == nil {
        return &node{priority: rand.Int(), key: key, cnt: 1}
    }
    if d := o.cmp(key); d >= 0 {
        o.ch[d] = t.ins(o.ch[d], key)
        if o.ch[d].priority > o.priority {
            o = o.rotate(d ^ 1)
        }
    } else {
        o.cnt++
    }
    return o
}

func (t *treap) del(o *node, key int) *node {
    if o == nil {
        return nil
    }
    if d := o.cmp(key); d >= 0 {
        o.ch[d] = t.del(o.ch[d], key)
    } else {
        if o.cnt > 1 {
            o.cnt--
        } else {
            if o.ch[1] == nil {
                return o.ch[0]
            }
            if o.ch[0] == nil {
                return o.ch[1]
            }
            d = 0
            if o.ch[0].priority > o.ch[1].priority {
                d = 1
            }
            o = o.rotate(d)
            o.ch[d] = t.del(o.ch[d], key)
        }
    }
    return o
}

func (t *treap) insert(key int) {
    t.root = t.ins(t.root, key)
}

func (t *treap) delete(key int) {
    t.root = t.del(t.root, key)
}

func (t *treap) min() (min *node) {
    for o := t.root; o != nil; o = o.ch[0] {
        min = o
    }
    return
}

func (t *treap) max() (max *node) {
    for o := t.root; o != nil; o = o.ch[1] {
        max = o
    }
    return
}

func main() {
    // 初始化
    t = &treap{}
    // 添加
    t.insert(100)
    // 删除
    t.delete(100)
    // 最大值
    t.max().key
    // 最小值
    t.min().key
}
```







## [1438. 绝对差不超过限制的最长连续子数组](https://leetcode.cn/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

> 给你一个整数数组 `nums` ，和一个表示限制的整数 `limit`，请你返回最长连续子数组的长度，该子数组中的任意两个元素之间的绝对差必须小于或者等于 `limit` *。*
>
> 如果不存在满足条件的子数组，则返回 `0` 。
>
>  
>
> **示例 1：**
>
> ```
> 输入：nums = [8,2,4,7], limit = 4
> 输出：2 
> 解释：所有子数组如下：
> [8] 最大绝对差 |8-8| = 0 <= 4.
> [8,2] 最大绝对差 |8-2| = 6 > 4. 
> [8,2,4] 最大绝对差 |8-2| = 6 > 4.
> [8,2,4,7] 最大绝对差 |8-2| = 6 > 4.
> [2] 最大绝对差 |2-2| = 0 <= 4.
> [2,4] 最大绝对差 |2-4| = 2 <= 4.
> [2,4,7] 最大绝对差 |2-7| = 5 > 4.
> [4] 最大绝对差 |4-4| = 0 <= 4.
> [4,7] 最大绝对差 |4-7| = 3 <= 4.
> [7] 最大绝对差 |7-7| = 0 <= 4. 
> 因此，满足题意的最长子数组的长度为 2 。
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [10,1,2,4,7,2], limit = 5
> 输出：4 
> 解释：满足题意的最长子数组是 [2,4,7,2]，其最大绝对差 |2-7| = 5 <= 5 。
> ```
>
> **示例 3：**
>
> ```
> 输入：nums = [4,2,2,2,4,4,2,2], limit = 0
> 输出：3
> ```

### 方法1

见 单调队列

### 方法2

滑动窗口+平衡树

### 思路2

连续子数组 - 可以考虑滑动窗口

当窗体内, 新元素滑入后, 最大值最小值的差值是否小于 limit , 如果超出, 则缩小左端点, 直到满足为止.

### 代码2

```python
from sortedcontainers import SortedList

class Solution:
    def longestSubarray(self, nums: List[int], limit: int) -> int:
        s = SortedList()
        l, r, n, ans = 0, 0, len(nums), 0
        while r < n:
            s.add(nums[r])
            while s[-1]-s[0]>limit:
                s.remove(nums[l])
                l+=1
            ans = max(ans, r-l+1)
            r += 1
        return ans
```

```Go
import "math/rand"

type node struct {
	ch       [2]*node
	priority int
	key      int
	cnt      int
}

func (o *node) cmp(b int) int {
	switch {
	case b < o.key:
		return 0
	case b > o.key:
		return 1
	default:
		return -1
	}
}

func (o *node) rotate(d int) *node {
	x := o.ch[d^1]
	o.ch[d^1] = x.ch[d]
	x.ch[d] = o
	return x
}

type treap struct {
	root *node
}

func (t *treap) ins(o *node, key int) *node {
	if o == nil {
		return &node{priority: rand.Int(), key: key, cnt: 1}
	}
	if d := o.cmp(key); d >= 0 {
		o.ch[d] = t.ins(o.ch[d], key)
		if o.ch[d].priority > o.priority {
			o = o.rotate(d ^ 1)
		}
	} else {
		o.cnt++
	}
	return o
}

func (t *treap) del(o *node, key int) *node {
	if o == nil {
		return nil
	}
	if d := o.cmp(key); d >= 0 {
		o.ch[d] = t.del(o.ch[d], key)
	} else {
		if o.cnt > 1 {
			o.cnt--
		} else {
			if o.ch[1] == nil {
				return o.ch[0]
			}
			if o.ch[0] == nil {
				return o.ch[1]
			}
			d = 0
			if o.ch[0].priority > o.ch[1].priority {
				d = 1
			}
			o = o.rotate(d)
			o.ch[d] = t.del(o.ch[d], key)
		}
	}
	return o
}

func (t *treap) insert(key int) {
	t.root = t.ins(t.root, key)
}

func (t *treap) delete(key int) {
	t.root = t.del(t.root, key)
}

func (t *treap) min() (min *node) {
	for o := t.root; o != nil; o = o.ch[0] {
		min = o
	}
	return
}

func (t *treap) max() (max *node) {
	for o := t.root; o != nil; o = o.ch[1] {
		max = o
	}
	return
}

func longestSubarray(nums []int, limit int) int {
	t := &treap{}
	l := 0
	ans := 0
	for r, num := range nums {
		t.insert(num)
		for t.max().key-t.min().key > limit {
			t.delete(nums[l])
			l++
		}
		ans = max(ans, r-l+1)
	}
	return ans
}

func max(a, b int) int {
	if a < b {
		return b
	}
	return a
}
```

### 复杂度分析2

#### 时间复杂度

$O(n\,log⁡\,n)$ ，其中 $n$ 是数组长度。向有序集合中添加或删除元素都是 $O(n\,log⁡\,n)$ 的时间复杂度。每个元素最多被添加与删除一次。

#### 空间复杂度

$O(n)$ ，其中 $n$ 是数组长度。最坏情况下有序集合将和原数组等大。

