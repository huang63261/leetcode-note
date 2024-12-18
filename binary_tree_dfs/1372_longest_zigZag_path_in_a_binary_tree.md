# 1372. Longest ZigZag Path in a Binary Tree

## 1.1. 解題思路

- **目標**：
  - 在給定的二叉樹中，找到其包含的最長 `ZigZag` 路徑，並返回其長度。
  - `ZigZag` 路徑的定義：
    - 從某個節點開始，按照 `"左 -> 右 -> 左"` 或 `"右 -> 左 -> 右"` 的方向交替移動。
      - 路徑長度是訪問到的節點數 - 1（單個節點的路徑長度為 0）。
- **限制**：
- **問題拆分**：
  - 如何維持 `ZigZag` 路徑定義？
  - 如何計算路徑最大值？
- **做法**：
  - 使用 DFS 算法，並傳入上一次路徑方向以及累計路徑次數。若與上次方向同向，重置路徑次數為 `1`；反之持續累加。
  - 遞迴處理左右子樹。
  - 比較至當前節點為止、左子樹、右子樹的最大路徑次數並返回。

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
    function longestZigZag($root) {
        return max($this->dfs($root->left, 'left', 1), $this->dfs($root->right,'right', 1));
    }

    function dfs($root, $direction, $count) {
        if ($root === null) {
            return $count - 1;
        }

        $leftCount = $this->dfs($root->left, 'left', $direction === 'left' ? 1 : $count + 1);
        $rightCount = $this->dfs($root->right, 'right', $direction === 'right' ? 1 : $count + 1);

        return max($count, $leftCount, $rightCount);
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(h) (h=logn for balanced tree, h=n for skewed tree)

## 1.3. 解法優化
