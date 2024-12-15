# 104. Maximum Depth of Binary Tree

## 1.1. 解題思路

- **目標**：
  - 返回二元樹的最大深度
- **限制**：

- **問題拆分**
  - 如何找二元樹的最大深度？
  - 如何處理空節點

- **做法**：
  - 遞迴法
    - 判斷若當前節點為 `null` 返回 0
    - 否則，遞迴計算節點左子樹和右子樹深度，並取最大值 + 1 返回

## 1.2. 程式碼實作

```php
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
     * @param TreeNode $root
     * @return Integer
     */
    function maxDepth($root) {
        if ($root == null) {
            return 0;
        }

        return max($this->maxDepth($root->left), $this->maxDepth($root->right)) + 1;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
  - 每個節點訪問一次
- **Space complexity:** O(h)
  - 遞迴函數調用棧的深度

## 1.3. 解法優化
