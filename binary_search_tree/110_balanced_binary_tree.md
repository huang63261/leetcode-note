# 110. Balanced Binary Tree

## 1.1. 解題思路

- **目標**：
  - 判斷給定的二元樹是否為 高度平衡的二元樹。
  - 一棵二元樹被認為是高度平衡的，若且唯若每個節點的左右子樹的高度差不超過 1。
- **限制**：
  - 輸入樹可以是空樹
- **問題拆分**：
  - 如何計算樹高？
  - 如何判斷子樹是否平衡？
  - 如何全局判斷平衡性？
- **做法**：
  - 遞迴函數計算
    - 左右子樹高度
    - 檢查左右子樹高度是否相差大於一或左右任一子樹返回 `-1`，返回 `-1` 代表當前節點不平衡
    - 否則返回該節點高度

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
     * @return Boolean
     */
    function isBalanced($root) {
        return $this->checkHeight($root) !== -1;
    }

    function checkHeight($root) {
        if ($root === null) {
            return 0;
        }

        $leftHeight = $this->checkHeight($root->left);
        // 若左側子樹不平衡
        if ($leftHeight === -1) {
            return -1;
        }

        $rightHeight = $this->checkHeight($root->right);
        // 若右側子樹不平衡
        if ($rightHeight === -1) {
            return -1;
        }

        // 當前節點不平衡
        if (abs($leftHeight - $rightHeight) > 1) {
            return -1;
        }

        // 返回當前子樹高度
        return max($leftHeight, $rightHeight) + 1;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
  - `n` 為節點數量
- **Space complexity:** O(h) ~= O(logn)
  - `h` 為樹高，約為 log(n)

## 1.3. 解法優化
