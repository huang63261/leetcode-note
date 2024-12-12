# 206. Reverse Linked List

## 1.1. 解題思路

- **目標**
  - 將給定的單向鏈表反轉，返回反轉後的鏈表頭節 `head`。

- **限制**
  - 無
- **問題拆分**：
  - 如何反轉鏈表的指針？
  - 如何處理邊界條件？

- **做法**
  - 遍歷鏈表，將每個節點的 `next` 指針指向前一個節點。
  - 使用三個指針變量來追蹤當前節點、前一個節點和下一個節點。

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
     * @return ListNode
     */
    function reverseList($head) {
        $prev = null;
        $current = $head;

        while ($current !== null) {
            $next = $current->next;
            $current->next = $prev;

            $prev = $current;
            $current = $next;
        }

        return $prev;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(1)

## 1.3. 解法優化
