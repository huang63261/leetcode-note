# 2130. Maximum Twin Sum of a Linked List

## 1.1. 解題思路

- **目標**：
  - 計算給定鏈表中「雙胞胎和」的最大值，並返回該值。
    - 雙胞胎節點定義為第 i 個節點和第 (n−1−i) 個節點。
    - 雙胞胎和為兩個雙胞胎節點的值相加。
- **限制**：
  - `n` 必定為偶數
- **問題拆分**：
  - 如何配對雙胞胎節點？
- **做法**：
  - 使用快慢指針找中點，同時於棧中紀錄中點以前的值
  - 鏈表後半段與棧頂相加，並比對是否大於目前最大值
  - 返回雙胞胎和最大值

## 1.2. 程式碼實作

```php
/**
 * Definition for a singly-linked list.
 * class ListNode {
 *     public $val = 0;
 *     public $next = null;
 *     function __construct($val = 0, $next = null) {
 *         $this->val = $val;
 *         $this->next = $next;
 *     }
 * }
 */
class Solution {

    /**
     * @param ListNode $head
     * @return Integer
     */
    function pairSum($head) {
        $slow = $fast = $head;
        $max = 0;
        $stack = [];

        while ($fast !== null && $fast->next !== null) {
            $stack[] = $slow->val;
            $slow = $slow->next;
            $fast = $fast->next->next;
        }

        while ($slow !== null) {
            $max = max($max, array_pop($stack) + $slow->val);
            $slow = $slow->next;
        }

        return $max;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(n / 2)

## 1.3. 解法優化

- 可以通過將後半段鏈表反轉來代替棧，將空間複雜度從 O(n / 2) 降為 O(n)

```php
/**
 * Definition for a singly-linked list.
 * class ListNode {
 *     public $val = 0;
 *     public $next = null;
 *     function __construct($val = 0, $next = null) {
 *         $this->val = $val;
 *         $this->next = $next;
 *     }
 * }
 */
class Solution {

    /**
     * @param ListNode $head
     * @return Integer
     */
    function pairSum($head) {
        $slow = $fast = $head;

        // 快慢指針找中點
        while ($fast !== null && $fast->next !== null) {
            $slow = $slow->next;
            $fast = $fast->next->next;
        }


        // 反轉後半段鏈表
        $prev = null;
        while ($slow !== null) {
            $next = $slow->next;
            $slow->next = $prev;
            $prev = $slow;
            $slow = $next;
        }

        //計算最大雙胞胎和
        $frontPointer = $head;
        $backPointer = $prev;
        $max = 0;
        while ($backPointer !== null) {
            $max = max($max, $frontPointer->val + $backPointer->val);
            $frontPointer = $frontPointer->next;
            $backPointer = $backPointer->next;
        }

        return $max;
    }
}
```
