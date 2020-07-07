

# 206  反转链表

##  迭代实现

反转链表的核心思想在于，让每个节点指向前一个结点。

注意：在操作链表时，如果前一个结点指向其他的结点，那么必须记住该结点。

![image-20200312214659602](.\picture\image-20200312214659602.png)

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

## 递归实现

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

# 24 两两交换链表中的节点

![image-20200312233012678](.\picture\image-20200312233012678.png)

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

# 141 环形链表

# 142 环形链表2



# 283 移动零

【[题目描述](https://leetcode-cn.com/problems/move-zeroes/)】

> 给定一个数组nums，编写一个函数将所有0移动到数组的末尾，同时保持非零元素的相对顺序。
>
> 示例：
>
> 输入: [0,1,0,3,12]
> 输出: [1,3,12,0,0]

【解题思路】

> **思路一：**
>
> 遍历整个数组，将数组中非零的数添加到新的数组中；
>
> 将新数组中的数从左向右复制到原来的数组中；
>
> 最后将原来数组中剩余的数置为0。
>
> **思路二：**
>
> 遍历整个数组，将非0的数放在数组前面
>
> k-[0...k) 中保存所有当前遍历过的非0元素
>
> k指向的是下一次存放非零的元素
>
> 最后从当前的k开始将所有的元素赋值为0
>
> 遍历到第i个元素后，保证[0...i]中所有非0元素都按照顺序排列在[0...k) 中
>
> **思路三：**
>
> 思路二中最后要做的额外的操作时，将数组中剩余的部分置为0，那么有没有一种方法不用去做这一步额外的操作。
>
> 遍历整个数组，将非0的数放在数组前面
>
> k-[0...k) 中保存所有当前遍历过的非0元素
>
> k指向的是下一次存放非零的元素
>
> 遍历到第i个元素后，保证[0...i]中所有非0元素都按照顺序排列在[0...k) 中同时，[k...i]为0

【代码实现】

```go
func moveZeroes(nums []int)  {
    k := -1  // [0,k]代表大于0的区域，k=-1表示该区域还不存在
    for i := 0; i < len(nums); i++ {
        if nums[i] != 0 {
            nums[k+1], nums[i] = nums[i], nums[k+1]
            k++
        }
    }
}

// 另解
func moveZeroes(nums []int)  {
    pos := 0 // 指向下一个不为0的位置
    for i := 0; i < len(nums); i++ {
        if nums[i] != 0 {
            nums[pos] = nums[i]
            if pos != i {
                nums[i] = 0
            }
        }
        pos++
    }
}
```



# 1 两数之和

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

# 15 三数之和

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

【解题思路】

> `a+b+c=0`换而言之，是找出数组中是否包含`a+b=-c`。
>
> **思路1：**
>
> 使用暴力的方法：
>
> 三层循环嵌套，遍历数组，看是否有复合的`a,b,c`
>
> 因为题目中要求的答案中不可以包含重复的三元组，在暴力的解法中，对每一个满足`a+b+c=0`的`a,b,c`进行排序，并将排序号的三元组添加到map中，使用map去重，最后得到最终结果。

【代码实现】

```go
func threeSum(nums []int) [][]int {
    set := make([[3]int]bool)  // 去重
    res := make([][]int, 0)  // 存储最后的结果
    
    for i := 0; i < len(nums) - 2; i++ {
        for j := i + 1; j < len(nums) - 1; j++ {
            for k := j + 1; k < len(nums); k++ {
                if nums[i] + nums[j] + nums[k] == 0 {
                    tmp := []int{nums[i], nums[j], nums[k]}
                    sort.Ints(tmp)   
                    newTmp := [3]int{tmp[0], tmp[1], tmp[2]}
                    _, ok := set[newTmp]
                    if != ok {
                        set[newTmp] = true
                        res = append(res, tmp)
                    }
                }
            }
        }
    }
    return tmp
}
```

> 思路2：
>
> 使用双指针法
>
> 整体思路如下图所示：
>
> ![image-20200705093536494](E:\Dabric\Dabric\Learning-algorithm\picture\image-20200705093536494.png)
>
> 特判，对于数组长度 n，如果数组为 null 或者数组长度小于 3，返回 [][]。
> 对数组进行排序。
> 遍历排序后数组：
>
> * 若 `nums[i]>0`：因为已经排序好，所以后面不可能有三个数加和等于 00，直接返回结果。
> * 对于重复元素：跳过，避免出现重复解
> * 令左指针 `L=i+1`，右指针 `R=n-1`，当 `L<R` 时，执行循环：
>   * 当 `nums[i]+nums[L]+nums[R]==0`，执行循环，判断左界和右界是否和下一位置重复，去除重复解。并同时将 `L,R` 移到下一位置，寻找新的解
>   * 若和大于 0，说明 `nums[R]`太大，R 左移
>   * 若和小于 0，说明 `nums[L]` 太小，L 右移

【代码实现】

```go
func threeSum(nums []int) [][]int {
	var results [][]int
	sort.Ints(nums)
	for i := 0; i < len(nums)-2; i++ {
		if i > 0 && nums[i] == nums[i-1] {
			continue//To prevent the repeat
		}
		target, left, right := -nums[i], i+1, len(nums)-1
		for left < right {
			sum := nums[left] + nums[right]
			if sum == target {
				results = append(results, []int{nums[i], nums[left], nums[right]})
				left++
				right--
				for left < right && nums[left] == nums[left-1] {
					left++
				}
				for left < right && nums[right] == nums[right+1] {
					right--
				}
			} else if sum > target {
				right--
			} else if sum < target {
				left++
			}
		}
	}
	return results
}
```



# 88 合并两个有序数组

【[题目描述](https://leetcode-cn.com/problems/merge-sorted-array)】

> 给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。
>
>  
>
> 说明:
>
> 初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。
> 你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。
>
>
> 示例:
>
> 输入:
> nums1 = [1,2,3,0,0,0], m = 3
> nums2 = [2,5,6],       n = 3
>
> 输出: [1,2,2,3,5,6]
>

【解题思路】

> 思路一：
>
> 由于nums1中有足够的空间存储nums2中的数据，所以可以将nums2中的数据全部拷贝到nums1中，然后对nums1进行排序。
>
> 该思路的时间复杂度为`O(nlogn)`，时间主要花费在排序的过程中。

> 思路二：
>
> nums1中有足够的空间存储nums2，所以最终的结果nums1的长度为`m+n`
>
> 从后往前分别遍历`nums1`和`nums2`：
>
> 谁大将谁复制到`m+n`长度的 `nums1`的后面；
>
> 当nums1遍历完后，nums2还没遍历完，将nums2剩下的元素拷贝到nums1的前面。
>
> 思路的示意图如下：

![image-20200517082033949](.\picture\image-20200517082033949.png)

【代码实现】

```go
func MergeSortedArray(nums1 []int, m int, nums2 []int, n int) {
    for m > 0 && n > 0 {
        if nums1[m-1] > nums2[n-1] {
            nums1[m+n-1] = nums1[m-1]
            m--
        } else {
            nums1[m+n-1] = nums2[n-1]
            n--
        }
    }
    
    if m == 0 {
        for i := 0; i < n; i++ {
            nums1[i] = nums2[i]
        }
    }
    
}
```

# 138 复制带随机指针的链表

# 面试题40 最小的k个数

【题目描述】

> 给定一个整数数组，找出其中最小的 `k` 个数。例如，输入`[4,5,1,6,2,7,3,8]`这8个数字，则最小的4个数字是[1,2,3,4]
>
> 注意：返回的最小k个数的顺序为任意。

【解题思路】

> 思路一：
>
> 直接对数组进行从小到大的排序，然后取出数组中的前k个元素即可。
>
> 思路二：
>
> 使用堆结构
>
> 遍历数组中的元素，将前k个元素添加到大根堆中；
>
> 从k+1个元素开始，如果比堆顶元素小，将堆顶元素替换，重新生成大根堆；
>
> 等数组遍历完成后，大根堆中存储的就是数组中最小的k个数。

# 11 盛最多水的容器

【[题目描述](https://leetcode-cn.com/problems/container-with-most-water/)】

> 给你n个非负整数`a1,a2,...,an`，每个数带哦表坐标中的一个点`(i,ai)`。在坐标内画n条垂直线，垂直线i的两个端点分别为`(i, ai)`和`(i,0)`。找出其中的两条线，使得它们与x轴共共同构成的容器可以容纳最多的水。
>
> **说明：**你不能倾斜容器，且n值至少为2。
>
> ![image-20200705150501421](.\picture\image-20200705150501421.png)
>
> 图中垂直线代表输入数组`[1,8,6,2,5,4,8,3,7]`。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为49。
>
> **示例：**
>
> **输入：**`[1,8,6,2,5,4,8,3,7]`
>
> **输出：**49

【解题思路】

> **思路1：**
>
> 使用暴力解法，双层循环

【代码实现】

```go
func maxArea(height []int) int {
    maxarea := 0
    
    for i := 0; i < len(height) - 1; i++ {
        for j := i + 1; j < len(height); j++ {
            area := (j-i) * min(height[i], height[j])
            maxarea = max(maxarea, area)
        }
    }

    return maxarea
}

func max(i, j int) int {
    if i > j {
        return i
    }
    return j
}

func min(i, j int) int {
    if i > j {
        return j
    }
    return i
}
```

> **思路2：**
>
> 双指针法：
>
> 由于是找最大的面积，首先先考虑数组中最左和最右的元素，因为这两个元素所有构成的面积的长是最长的，计算出二者所构成的面积，然后两边的边界向内收缩，两边的边界谁小谁向内收缩，因为其高度低，虽然和另一个边界所构成的长是最长的但是在高度可能低于里面的值。

【代码实现】

```go
func maxArea(height []int) int {
    maxarea := 0
    left := 0
    right := len(height) - 1

    for left < right {
        area := (right-left) * min(height[left], height[right])
        maxarea = max(maxarea, area)

        if height[left] < height[right] {
            left++
        } else {
            right--
        }
    }
    return maxarea
}

func max(i, j int) int {
    if i > j {
        return i
    }
    return j
}

func min(i, j int) int {
    if i > j {
        return j
    }
    return i
}
```

# 21 合并两个有序链表

【[题目描述](https://leetcode-cn.com/problems/merge-two-sorted-lists/)】

> 将两个升序链表合并为一个新的**升序**链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。
>
> **示例：**
>
> **输入：**1->2->4, 1->3->4
> **输出：**1->1->2->3->4->4

【解题思路】

> 思路1：
>
> 使用归并的思想，遍历两个链表，谁小将当前节点添加到新的链表中；
>
> 最后判断哪个链表不为空，将不为空的添加到新的链表后

【代码实现】

```go
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    cur1 := l1
    cur2 := l2
    newHead := new(ListNode)
    index := newHead
    
    for cur1 != nil && cur2 != nil {
        if cur1.Val < cur2.Val {
            index.Next = cur1
            index = index.Next
            cur1 = cur1.Next
        } else {
            index.Next = cur2
            index = index.Next
            cur2 = cur2.Next
        }
    }
    
    if cur1 == nil {
        index.Next = cur2
    }
    
    if cur2 == nil {
        index.Next = cur1
    }
	return newHead.Next
}
```

> 思路2：
>
> 使用递归，具体代码如下：

【代码实现】

```go
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    if l1 == nil {
        return l2
    }
    if l2 == nil {
        return l1
    }
    
    if l1.Val < l2.Val {
        l1.Next = mergeTwoLists(l1.Next, l2)
        return l1
    }
    l2.Next = mergeTwoLists(l1, l2.Next)
    return l2
}
```

# 26 删除排序数组中的重复项

【[题目描述](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)】

【解题思路】

> 思路：
>
> 首先设置两个指p和q，p指向数组的第一个位置，q指向数组的第二个位置
>
> 如果p和q所指的位置上的数相等，q向后移
>
> 不相等，将q位置上的数复制到p+1的位置上，p和q同时后移
>
> 重复上面的过程，直到q等于数组的长度
>
> 最后返回p+1，即为不重复数组的长度

【代码实现】

```go
func removeDuplicates(nums []int) int {
    p := 0
    q := 1
    for q < len(nums) {
        if nums[p] != nums[q] {
            nums[p+1] = nums[q]
            p++
        }
        q++
    }
    return p+1
}
```

