
### 剑指 Offer II 022. 链表中环的入口节点

给定一个链表，返回链表开始入环的第一个节点。 从链表的头节点开始沿着 next 指针进入环的第一个节点为环的入口节点。如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。

说明：不允许修改给定的链表。

解题思路：

题目与141.环形链表有类似的地方。

首先使用快慢指针，如果两个指针能够相遇，说明存在环，否则可以断定链表中不存在环结构。

下面对寻找入环结点的方法进行严格的证明。

```
假设入环结点距离链表头长度为c，环长度为b，指针相遇时顺指针距离入环结点为a。
        c
-------------------
        ｜        ｜    b
        ｜        ｜  
      a  ----------
      
在两指针相遇时，快指针走过的距离是慢指针走过距离的2倍，所以有：
2 *（c + b-a） = c + nb + b - a

其中n是快指针绕环的圈数。经过化简后可以得出

a = (1-n)b + c

也就是说两个指针相遇时，距离入环结点的距离和c有以上关系。

因为b是环的长度，所以如果使用一个新指针从头结点开始遍历，则新指针最终会和慢指针在入环结点相遇。
         
```


```
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next == null|| head.next.next == null) {
            return null;
        }
        ListNode fast = head.next.next;
        ListNode slow = head.next;
        while (fast != slow) {
            if (fast.next == null || fast.next.next == null) {
                return null;
            }
            fast = fast.next.next;
            slow = slow.next;
        }
        fast = head;
        while (fast != slow) {
            if (fast.next == null) {
                return null;
            }
            fast = fast.next;
            slow = slow.next;
        }
        return fast;
    }
}
```