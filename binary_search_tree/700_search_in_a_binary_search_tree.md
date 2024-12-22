# 700. Search in a Binary Search Tree

## 1.1. 解題思路

- **目標**：
  - 給定一個 `Binary Search Tree (BST)` 以及 目標值 `val`，搜尋 `BST` 中是否存在與 `val` 相同的節點並返回其節點；若無，返回 `null`
- **限制**：
- **問題拆分**：
- **做法**：
  - 使用 `DFS` 遍歷 `BST`

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
     * @param Integer $val
     * @return TreeNode
     */
    function searchBST($root, $val) {
        return $this->dfs($root, $val);
    }

    function dfs($root, $val) {
        if ($root === null) return null;

        if ($root->val === $val) {
            return $root;
        }

        $left = $this->dfs($root->left, $val);
        $right = $this->dfs($root->right, $val);

        if ($left !== null) {
            return $left;
        }

        if ($right !== null) {
            return $right;
        }
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
  - `n` 為 `BST` 總節點數
- **Space complexity:** O(1)

## 1.3. 解法優化

- 未利用二元搜索樹 (BST) 的特性：
  - 原算法進行了遍歷左右子樹的操作（同時檢查左右），這樣會導致冗餘計算。BST 的特性使得只需要檢查一邊的子樹即可。
- 搜索過程中，無需同時記錄左右結果，只需沿著正確的分支搜索即可。

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
     * @param Integer $val
     * @return TreeNode
     */
    function searchBST($root, $val) {
        if ($root === null || $root->val === $val) {
            return $root;
        }

        if ($val < $root->val) {
            return $this->searchBST($root->left, $val);
        } else {
            return $this->searchBST($root->right, $val);
        }
    }
}
```

- **Time complexity:**
  - 最差情況：O(n) （退化成鏈表）
  - 最佳情況：O(log n) （平衡二元樹）
- **Space complexity:** O(1)
