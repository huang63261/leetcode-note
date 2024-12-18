# 1448. Count Good Nodes in Binary Tree

## 1.1. 解題思路

- **目標**：
  - 當從根節點到某節點的路徑中，該節點的值是路徑上的最大值時，該節點被認為是 `Good Node`。計算二叉樹中「Good Nodes」的數量。
- **限制**：
  - 根節點永遠是 `Good Node`
- **問題拆分**：
  - 如何判斷節點是否為 `Good Node`？
  - 如何計算 `Good Node`s 的數量？
- **做法**：
  - 使用DFS遍歷二元樹。
  - 使用一變量 `maxSoFar` 紀錄從根節點到當前節點的最大值。
  - 若當前節點的值是否大於當前最大值 `maxSoFar`，計數器`count`加一

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
    function goodNodes($root) {
        return $this->dfs($root, PHP_INT_MIN);
    }

    function dfs($node, $maxSoFar) {
        if ($node == null) {
            return 0;
        }

        $isGoodNode = $node->val >= $maxSoFar ? 1 : 0;
        $maxSoFar = max($node->val, $maxSoFar);

        $left = $this->dfs($node->left, $maxSoFar);
        $right = $this->dfs($node->right, $maxSoFar);

        return $isGoodNode + $left + $right;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(1)

## 1.3. 解法優化
