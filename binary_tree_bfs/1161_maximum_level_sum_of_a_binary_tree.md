# 1161. Maximum Level Sum of a Binary Tree

## 1.1. 解題思路

- **目標**：
  - 在一棵 `B-tree` 二元樹中，找到節點值總和最大的層，並返回其層數（從 `1` 開始計數）。
  - 如果有多個層具有相同的最大總和，則返回層數最小的那一層。

- **限制**
  - 樹的根節點一定存在且層數從 1 開始。
- **問題拆分**：
  - 如何按層遍歷樹
  - 如何紀錄每層的總和

- **做法**
  - 使用 `BFS`
  -

## 1.2. 程式碼實作

### 演算法複雜度

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
    function maxLevelSum($root) {
        if ($root === null) {
            return;
        }

        $queue = new SplQueue();
        $queue->enqueue($root);
        $currentLayer = 1;
        $maxLayer = null;
        $max = PHP_INT_MIN;

        while (!$queue->isEmpty()) {
            $layerCount = $queue->count();
            $layerTotal = 0;

            for ($i = 0; $i < $layerCount; $i++) {
                $node = $queue->dequeue();

                $layerTotal += $node->val;

                if ($node->left !== null) {
                    $queue->enqueue($node->left);
                }

                if ($node->right !== null) {
                    $queue->enqueue($node->right);
                }
            }

            if ($layerTotal > $max) {
                $max = $layerTotal;
                $maxLayer = $currentLayer;
            }

            $currentLayer++;
        }

        return $maxLayer;
    }
}
```

- **Time complexity:** O(n)
- **Space complexity:** O(w)
  - w 為二元樹最大寬度

## 1.3. 解法優化
