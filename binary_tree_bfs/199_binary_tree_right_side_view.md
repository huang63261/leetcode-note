# 199. Binary Tree Right Side View

## 1.1. 解題思路

- **目標**：
  - 從二叉樹的右側觀察，返回每層能夠看見的節點值，按照從上到下的順序排列。
- **限制**：
  - 無
- **問題拆分**：
  - 如何確認是否為右側節點？
  - 如何遍歷樹？
  - 如何處理空樹或不完整的樹
- **做法**：
  - 使用 BFS 遍歷
  - 遍歷每層節點時，記錄當前層的最後一個節點值，該值即為右側可見的節點。

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
     * @return Integer[]
     */
    function rightSideView($root) {
        if ($root == null) {
            return [];
        }

        $queue = new SplQueue();
        $queue->enqueue($root);
        $rightSide = [];

        while (!$queue->isEmpty()) {
            $layer_count = $queue->count();

            for ($i = 0; $i < $layer_count; $i++) {
                $node = $queue->dequeue();

                if ($i === $layer_count - 1) {
                    $rightSide[] = $node->val;
                }

                if ($node->left !== null) {
                    $queue->enqueue($node->left);
                }
                if ($node->right !== null) {
                    $queue->enqueue($node->right);
                }
            }
        }

        return $rightSide;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(w)
  - w 為二元樹最大寬度

## 1.3. 解法優化
