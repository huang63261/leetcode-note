# 2095. Delete the Middle Node of a Linked List

## 1.1. 解題思路

- **目標**：
  - 從給定的單向鏈表中刪除中間節點，並返回修改後的鏈表頭節點 `head`。
    - 中間節點的定義為鏈表長度 `n` 的第 `⌊n/2⌋` 個節點（基於 0 索引）。
    - 如果鏈表長度為 1，則刪除後返回 `null`。

- **限制**：
- **問題拆分**：
  - 如何找到中間節點？
  - 如何刪除中間節點？
- **做法**：
  - 先遍歷一次用於確認鏈表長度，並找出中間節點。
  - 移動至中間節點並移除。（將中間節點前一節點的 `next` 改為中間節點後一節點）

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
    function deleteMiddle($head) {
        if ($head === null || $head->next === null) {
            return false;
        }

        // 遍歷一次，計算
        $length = 1;
        $current = $head;
        while ($current->next !== null) {
            $length++;
            $current = $current->next;
        }

        // 取得中間點並移除
        $middle = floor($length / 2);
        $current = $head;
        // 從鏈表頭節點開始， $i = 1
        for ($i = 1; $i < $middle; $i++) {
            $current = $current->next;
        }
        $current->next = $current->next->next;

        return $head;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(1)

## 1.3. 解法優化

- 使用雙指針/快慢指針來找中間節點，可避免遍歷鏈計算長度

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
    function deleteMiddle($head) {
        if ($head === null || $head->next === null) {
            return false;
        }

        $prev = null;
        $fast = $head;
        $slow = $head;

        while ($fast !== null && $fast->next !== null) {
            $prev = $slow;
            $slow = $slow->next;
            $fast = $fast->next->next;
        }

        $prev->next = $slow->next;

        return $head;
    }
}
```

- **Time complexity:** O(n)
- **Space complexity:** O(1)
