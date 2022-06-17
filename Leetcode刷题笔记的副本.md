# Leetcode刷题笔记

### 1.两数之和

**问题：**

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

示例 1：

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**解答：**

```
//Go
func twoSum(nums []int, target int) []int {
	if nums == nil {
		return nil
	}
	temp := make(map[int]int, 2)
	for index, key := range nums {
		value, ok := temp[key]
		if ok {
			return []int{index, value}
		} else {
			temp[target-key] = index
		}
	}
	return nil
}
```

```
//Java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> objectObjectHashMap = new HashMap<>();
        int length = nums.length;
        for (int i = 0; i < length; i++) {
            int num = nums[i];
            if (objectObjectHashMap.containsKey(num)) {
                Integer itemOne = objectObjectHashMap.get(num);
                return new int[]{itemOne, i};
            } else {
                objectObjectHashMap.put(target - num, i);
            } 
        }
        return null;
    }
}
```

```
//Python
class Solution:
    def twoSum(self, nums, target):
        dict_element = dict()
        size = len(nums)
        for i in range(size):
            item = nums[i]
            if item in dict_element:
                return [dict_element[item], i]
            else:
                dict_element[target - item] = i
```


### 2.两数相加

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

```
// java 递归
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        return addTwoNumbers(l1, l2, null, 0);
    }

    private ListNode addTwoNumbers(ListNode l1, ListNode l2, ListNode temp, int jin) {
        if (l1 == null && jin == 0) {
            return l2;
        }
        if (l2 == null && jin == 0) {
            return l1;
        }
        int value = getValue(l1) + getValue(l2) + jin;
        jin = value / 10;
        value = value % 10;
        ListNode newNode = new ListNode(value);
        if (temp == null) {
            temp = newNode;
        } else {
            temp.next = newNode;
            temp = temp.next;
        }
        temp.next = addTwoNumbers(getNext(l1), getNext(l2), temp, jin);
        return newNode;
    }

    private int getValue(ListNode node) {
        if (node == null) {
            return 0;
        }
        return node.val;
    }

    private ListNode getNext(ListNode node) {
        if (node == null) {
            return null;
        }
        return node.next;
    }
}


// java 循环
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        }
        if (l2 == null) {
            return l1;
        }
        int jin = l1.val + l2.val;
        ListNode head = new ListNode(jin % 10);
        jin /= 10;
        ListNode temp = head;
        l1 = l1.next;
        l2 = l2.next;
        while (l1 != null || l2 != null) {
            if (l1 != null) {
                jin += l1.val;
                l1 = l1.next;
            }
            if (l2 != null) {
                jin += l2.val;
                l2 = l2.next;
            }
            temp.next = new ListNode(jin % 10);
            jin /= 10;
            temp = temp.next;
        }
        if (jin > 0) {
            temp.next = new ListNode(jin);
        }
        return head;
    }
}

```

### 14.最小的K个元素/TopK/Kth element

设计一个算法，找出数组中最小的k个数。以任意顺序返回这k个数均可。

示例：

```
输入： arr = [1,3,5,7,2,4,6,8], k = 4
输出： [1,2,3,4]
```

```
// Go
func smallestK(arr []int, k int) []int {
	if k <= 0 {
		return nil
	}
	if k >= len(arr) {
		return arr
	}
	partition := partition(arr, 0, len(arr)-1)
	if partition == k {
		return arr[0:k]
	} else if partition < k {
		temp := arr[0:partition]
		return append(temp, smallestK(arr[partition:], k-partition)...)
	} else if partition > k {
		return smallestK(arr[0:partition], k)
	}
	return nil
}

func partition(arr []int, left int, right int) int {
    // rand.Seed(time.Now().Unix())
	random := left + rand.Intn(right-left+1)
	swap(arr, left, random)
	index := left
	temp := arr[index]
	for i := left; i <= right; i++ {
		if arr[i] < temp {
			swap(arr, i, index+1)
			index++
		}
	}
	swap(arr, index, left)
	return index + 1
}

func swap(arr []int, i int, j int) {
	arr[i], arr[j] = arr[j], arr[i]
}
```


```
// java
class Solution {
    
    public int[] smallestK(int[] arr, int k) {
        if (arr == null || k <= 0) {
            return new int[0];
        }
        if (k >= arr.length) {
            return arr;
        }
        int partition = partition(arr, 0, arr.length - 1);
        if (partition == k) {
            return Arrays.copyOfRange(arr, 0, k);
        } else if (partition > k) {
            return smallestK(Arrays.copyOfRange(arr, 0, partition), k);
        } else if (partition < k) {
            int[] array1 = Arrays.copyOfRange(arr, 0, partition);
            int[] array2 = smallestK(Arrays.copyOfRange(arr, partition, arr.length), k - partition);
            return merge(array1, array2);
        }
        return new int[0];
    }

    private int[] merge(int[] array1, int[] array2) {
        int[] result = new int[array1.length + array2.length];
        for (int i = 0; i < array1.length; i++) {
            result[i] = array1[i];
        }
        for (int i = 0; i < array2.length; i++) {
            result[i + array1.length] = array2[i];
        }
        return result;
    }

    private int partition(int[] arr, int left, int right) {
        int random = left + (int) ((right - left) * Math.random());
        swap(arr, left, random);
        int pivot = arr[left];
        int index = left;
        for (int i = left; i <= right; i++) {
            if (arr[i] < pivot) {
                swap(arr, i, index + 1);
                index++;
            }
        }
        swap(arr, left, index);
        return index + 1;
    }

    private void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

### 204. 计数质数

给定整数 n ，返回 所有小于非负整数 n 的质数的数量 。

**解答：**

**Leetcode高赞👍**

这题搜到一个非常牛逼的算法,叫做厄拉多塞筛法. 比如说求20以内质数的个数,首先0,1不是质数.2是第一个质数,然后把20以内所有2的倍数划去.2后面紧跟的数即为下一个质数3,然后把3所有的倍数划去.3后面紧跟的数即为下一个质数5,再把5所有的倍数划去.以此类推.

代码的实现上用了非常好的技巧:

```
// python
class Solution:
    def countPrimes(self, n: int) -> int:
        if n < 3:
            return 0     
        else:
            # 首先生成了一个全部为1的列表
            output = [1] * n
            # 因为0和1不是质数,所以列表的前两个位置赋值为0
            output[0],output[1] = 0,0
             # 此时从index = 2开始遍历,output[2]==1,即表明第一个质数为2,然后将2的倍数对应的索引
             # 全部赋值为0. 此时output[3] == 1,即表明下一个质数为3,同样划去3的倍数.以此类推.
            for i in range(2,int(n**0.5)+1): 
                if output[i] == 1:
                    output[i*i:n:i] = [0] * len(output[i*i:n:i])
         # 最后output中的数字1表明该位置上的索引数为质数,然后求和即可.
        return sum(output)

```

在上面遍历索引的时候用到了一个非常好的技巧. 即i是从(2,int(n***0.5)+1)而非(2,n).这个技巧是可以验证的,比如说求9以内的质数个数,那么只要划掉sqrt(9)以内的质数倍数,剩下的即全为质数. 所以在划去倍数的时候也是从i*i开始划掉,而不是i+i.

这个解法真是太赞了!又学到了很多~~~ 和大家分享一下

```
// java
class Solution {
    public int countPrimes(int n) {
        if (n < 3) {
            return 0;
        }
        int[] result = new int[n];
        Arrays.fill(result, 1);
        result[0] = 0;
        result[1] = 0;
        for (int i = 2; i < Math.sqrt(n); i++) {
            if (result[2] == 0) {
                continue;
            }
            for (int j = i * i; j < n; j += i) {
                result[j] = 0;
            }
        }
        return Arrays.stream(result).sum();
    }
}
```


### 226. 反转二叉树

给你一棵二叉树的根节点 root ，翻转这棵二叉树，并返回其根节点。


**解答：**

```
//Go
func invertTree(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}
	root.Left, root.Right = root.Right, root.Left
	invertTree(root.Left)
	invertTree(root.Right)
	return root
}
```

```
//Java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        TreeNode temp = root.right;
        root.right = root.left;
        root.left = temp;
        invertTree(root.left);
        invertTree(root.right);
        return root;
    }
}
```

```
//Python
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None
        root.left,root.right = root.right,root.left
        root.left = self.invertTree(root.left)
        root.right = self.invertTree(root.right)
        return root
```

