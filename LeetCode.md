# 1 链表

## 1.1 反转链表

### 1.1.1 迭代实现

反转链表的核心思想在于，让每个节点指向前一个结点。

注意：在操作链表时，如果前一个结点指向其他的结点，那么必须记住该结点。

![image-20200312214659602](C:\Users\Dabric\AppData\Roaming\Typora\typora-user-images\image-20200312214659602.png)

```go
func reverseList(head *ListNode) *ListNode {
    var pre *ListNode = nil
    cur := head
    for cur != nil {
        next := cur.Next
        cur.Next = pre
        pre = cur
        cur = next
    }
    return pre
}
```

### 1.1.2 递归实现

解题思路
链表是经典的递归定义的数据结构，链表相关的题目常常考察递归，翻转链表是其中的经典题目。
在思考递归问题的时候，我们要从上到下思考：

1. 子问题是什么

2. base case是什么

3. 在当前层要干什么

对翻转链表来说，以1->2->3->4->5为例：

子问题是：除去current node，翻转剩余链表，即除去1, reverseList(2->3->4->5),递归得到的解是 5->4->3->2

base case:当前节点为空，返回空，当前节点的next为空（只剩余一个节点），返回该节点

在当前层要干什么：翻转链表，即把1->2变为2->1.

最后return的是结果链表的头，也就是递归解的头

```go
// 递归实现
func reverseList(head *ListNode) *ListNode {
    //var pre *ListNode = nil 

    if head == nil || head.Next == nil {
        return head
    }

    pre := reverseList(head.Next)
    head.Next.Next = head
    head.Next = nil
    return pre
}
```

## 1.2 两两交换链表中的节点

![image-20200312233012678](C:\Users\Dabric\AppData\Roaming\Typora\typora-user-images\image-20200312233012678.png)

代码实现：

```go
func swapPairs(head *ListNode) *ListNode {
    
    if head == nil || head.Next == nil {
        return head
    }
    
    var pre *ListNode = &ListNode{0, head}
    var hint *ListNode = pre
    
    for pre.Next != nil && pre.Next != nil {
        a := pre.Next
        b := a.Next
        
        pre.Next, b.Next, a.Next = b, a, b.Next
        pre = a 
    }
    return hint.Next
    
}
```



## 1.3 判断一个链表是否有环

# 283. 移动零

思路一：

遍历整个数组，将数组中非零的数添加到新的数组中；

将新数组中的数从左向右复制到原来的数组中；

最后将原来数组中剩余的数置为0。

思路二：

遍历整个数组，将非0的数放在数组前面

k-[0...k) 中保存所有当前遍历过的非0元素

k指向的是下一次存放非零的元素

最后从当前的k开始将所有的元素赋值为0

遍历到第i个元素后，保证[0...i]中所有非0元素都按照顺序排列在[0...k) 中

思路三：

思路二中最后要做的额外的操作时，将数组中剩余的部分置为0，那么有没有一种方法不用去做这一步额外的操作。

遍历整个数组，将非0的数放在数组前面

k-[0...k) 中保存所有当前遍历过的非0元素

k指向的是下一次存放非零的元素

遍历到第i个元素后，保证[0...i]中所有非0元素都按照顺序排列在[0...k) 中

同时，[k...i]为0

# 1. 两数之和

【[题目描述](https://leetcode-cn.com/problems/two-sum/)】

> 给定一个整数数组`nums`和一个目标值`target`，请你在该数组中找出和为目标值的那**两个**整数，并返回他们的数组下标。
>
> **示例：**
>
> ```go
> 给定 nums = [2, 7, 11, 15], target = 9
> 
> 因为 nums[0] + nums[1] = 2 + 7 = 9
> 所以返回 [0, 1]
> ```

【解题思路】

> 思路一：
>
> 暴力解法，循环遍历两次，找出和为`target`两个数。
>
> 时间复杂度为`O(N^2)`

【代码实现】

```go
func twoSum(nums []int, target int) []int {
	var i, j int
	for i = 0; i < len(nums) - 1; i++ {
		for j = i + 1; j < len(nums); j++ {
			if nums[i] + nums[j] == target {
				return []int{i, j}
			}
		}
	}
	return nil
}
```

> 思路二：
>
> 使用`map`
>
> `map`能让每次的查找的时间复杂度为`O(1)`。
>
> 两次迭代`map`：
>
> 将`nums`中的值和对应的索引存储在`map`中
>
> 检查每个元素对应的目标元素是否在`map`中，该目标元素不能是检查的元素的本身

【代码实现】

```go
func twoSum1(nums []int, target int) []int {
	var numMap map[int]int
	numMap = make(map[int]int)
	for i := 0; i < len(nums); i++ {
		numMap[nums[i]] = i
	}
	for i := 0; i < len(nums); i++ {
		diff := target - nums[i]
		_, ok := numMap[diff]
		if  ok && numMap[diff] != i{
			return []int{i, numMap[diff]}
		}
	}
	return nil
}
```

> 思路三：
>
> 一次迭代过程：
>
> 在每次向`map`中添加元素时，可以先在`map`中检查是否有当前元素对应的目标元素。

【代码实现】

```go
func twoSum2(nums []int, target int) []int {
	var numMap map[int]int
	numMap = make(map[int]int)
	for i := 0; i < len(nums); i++ {
		diff := target - nums[i]
		_, ok := numMap[diff]
		if ok {
			return []int{numMap[diff], i}
		}
		numMap[nums[i]] = i
	}
	return nil
}
```

# 15. 三数之和

【[题目描述][https://leetcode-cn.com/problems/3sum/]】

> 给你一个包含 `n` 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 `a，b，c` ，使得 `a + b + c = 0` ？请你找出所有满足条件且不重复的三元组。
>
> **注意：**答案中不可以包含重复的三元组。
>
> 
>
> **示例：**
>
> ```go
> 给定数组 nums = [-1, 0, 1, 2, -1, -4]，
> 
> 满足要求的三元组集合为：
> [
>   [-1, 0, 1],
>   [-1, -1, 2]
> ]
> ```

