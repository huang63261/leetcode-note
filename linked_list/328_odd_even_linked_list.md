# 328. Odd Even Linked List

## 1.1. 解題思路

- **目標**：
  - 重新排列單向鏈表，使所有奇數索引節點排在偶數索引節點之前，同時保持原有順序。
    - 第 1 個節點為奇數索引，第 2 個節點為偶數索引，以此類推。
    - 返回重新排列後的鏈表頭節點。

- **限制**
  - 無
- **問題拆分**：
  - 如何辨識當前節點為奇數或偶數？
  - 如何分離奇數節點與偶數節點？
- **做法**：
  - 使用奇偶數雙指標分別重新排列，最後將奇數鏈接上偶數鏈。

## 1.2. 程式碼實作

```php
class ListNode {
    public $val = 0;
    public $next = null;
    function __construct($val = 0, $next = null) {
        $this->val = $val;
        $this->next = $next;
    }
}

class Solution {
    /**
     * @param ListNode $head
     * @return ListNode
     */
    function oddEvenList($head) {
        if ($head === null || $head->next === null) {
            // 如果鏈表為空或只有一個節點，直接返回
            return $head;
        }

        // 奇數鏈表的指針
        $odd = $head;
        // 偶數鏈表的指針
        $even = $head->next;
        // 儲存偶數鏈表的表頭
        $evenHead = $even;

        // 因需要奇數鏈的尾節點，終止條件為偶數 Pointer 當前節點為 null 或 next 為 null。
        while ($even !== null && $even->next !== null) {
            // 將奇數鏈表連接到下一個奇數節點
            $odd->next = $even->next
            // 更新奇數鏈表的指針
            $odd = $odd->next;
            // 將偶數鏈表連接到下一個偶數節點
            $even->next = $odd->next
            // 更新偶數鏈表的指針
            $even = $even->next;
        }

        // 合併奇數和偶數鏈表
        $odd->next = $evenHead;

        return $head;
    }
}

```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(1)

## 1.3. 解法優化
