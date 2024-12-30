# 109. Convert Sorted List to Binary Search Tree

## 1.1. 解題思路

- **目標**：
  - 將一個遞增排序的單向鏈表轉換為高度平衡的二元搜索樹（Binary Search Tree, BST），並返回該樹的根節點。
- **限制**
  - 必須構造出 高度平衡 的二元搜索樹
- **問題拆分**
  - 如何選擇根節點？
  - 如何處理鏈表的中間節點？
- **做法**：
  - 將鏈表轉為陣列
  - 陣列遞迴找中點並建立節點，左右子樹遞迴
  - 返回根節點

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
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     public $val = null;
 *     public $left = null;
 *     public $right = null;
 *     function __construct($val = 0, $left = null, $right = null) {
 *         $this->val = $val;
 *         $this->left = $left;
 *         $this->right = $right;
 *     }
 * }
 */
class Solution {

    /**
     * @param ListNode $head
     * @return TreeNode
     */
    function sortedListToBST($head) {
        $list = [];
        $node = $head;

        while ($node !== null) {
            $list[] = $node->val;
            $node = $node->next;
        }

        return $this->helper($list, 0, count($list) - 1);
    }

    private function helper($list, $left, $right) {
        if ($left > $right) {
           return null; 
        }

        $mid = ceil(($left + $right) / 2);
        $node = new TreeNode($list[$mid]);

        $node->left = $this->helper($list, $left, $mid - 1);
        $node->right = $this->helper($list, $mid + 1, $right);

        return $node;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(n)

## 1.3. 解法優化

- 優化步驟
  - 使用快慢指針找出中間節點
  - 從中間截斷鏈表
  - 建立根節點，左右子樹分別遞迴處理
  - 返回根節點

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
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     public $val = null;
 *     public $left = null;
 *     public $right = null;
 *     function __construct($val = 0, $left = null, $right = null) {
 *         $this->val = $val;
 *         $this->left = $left;
 *         $this->right = $right;
 *     }
 * }
 */
class Solution {

    /**
     * @param ListNode $head
     * @return TreeNode
     */
    function sortedListToBST($head) {
        // 若鏈表為空返回null
        if ($head === null) {
            return null;
        }

        // 只有一個節點返回節點
        if ($head->next == null) {
            return new TreeNode($head->val);
        }

        // 快慢指針找中心點
        $slow = $fast = $head;
        $prev = null;
        while ($fast !== null && $fast->next !== null) {
            $prev = $slow;
            $slow = $slow->next;
            $fast = $fast->next->next;
        }

        // 中斷鏈表
        if ($prev !== null) {
            $prev->next = null;
        }

        $node = new TreeNode($slow->val);
        $node->left = $this->sortedListToBST($head);
        $node->right = $this->sortedListToBST($slow->next);

        return $node;
    }
}
```

- **Time complexity:** O(nlogn)
- **Space complexity:** O(nlogn)
