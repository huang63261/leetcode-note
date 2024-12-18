# 236. Lowest Common Ancestor of a Binary Tree

## 1.1. 解題思路

- **目標**：
  - 在一棵二叉樹中找到兩個節點 `p` 和 `q` 的 最低公共祖先（LCA）。
    - 最低公共祖先（Lowest Common Ancestor） 定義為：
    - 在樹 `T` 中，某個節點 `x` 同時是 `p` 和 `q` 的祖先，且 `x` 是距離 `p` 和 `q` 最近的祖先節點。
- **限制**
  - `p` != `q`
  - 每個節點的值是獨立不重複的
- **問題拆分**：
  - 如何確定當前節點是否為 `LCA`？
    - 若該節點的左右子樹中分別包含著 `p` 以及 `q`，該節點即為 `LCA`
  - 如何表示找到 `p`, `q`？
  - 如何處理節點自身成為 `LCA` 的情況？
- **做法**：
  - 使用 `DFS` 遞迴

## 1.2. 程式碼實作

```php
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     public $val = null;
 *     public $left = null;
 *     public $right = null;
 *     function __construct($value) { $this->val = $value; }
 * }
 */

class Solution {
    /**
     * @param TreeNode $root
     * @param TreeNode $p
     * @param TreeNode $q
     * @return TreeNode
     */
    function lowestCommonAncestor($root, $p, $q) {
        return $this->dfs($root, $p, $q);
    }

    function dfs($root, $p, $q) {
        if ($root == null || $root == $p || $root == $q) {
            return $root;
        }

        $left = $this->dfs($root->left, $p, $q);
        $right = $this->dfs($root->right, $p, $q);

        if ($right !== null && $left !== null) {
            return $root;
        }

        return $left !== null ? $left : $right;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(n)

## 1.3. 解法優化
