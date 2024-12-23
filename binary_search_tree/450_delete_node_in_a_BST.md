# 450. Delete Node in a BST

## 1.1. 解題思路

- **目標**：
  - 給定二元搜索樹（BST）的根節點 `root` 和一個 `key`，刪除值為 `key` 的節點，並返回更新後的 `BST` 根節點。

- **限制**
  - 每個節點的值是唯一的，並且二元搜索樹性質有效。
  - 如果 key 不在樹中，則返回原樹。
- **問題拆分**：
  - 如何處理刪除操作？繼承者找誰？
- **做法**
  - 根據 `BST` 特性找出欲刪除節點
  - 該節點若
    - 無子節點：直接刪除
    - 一個子節點：以子節點作為繼承者取代
    - 兩個子節點：找出左子樹最大值節點（或是右子樹最小值節點），替換目標的值後。因為替代後需重新調整子樹，所以需遞迴刪除左子樹（右子樹）中取代目標的節點。

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
     * @param Integer $key
     * @return TreeNode
     */
    function deleteNode($root, $key) {
        if ($root === null) {
            return null;
        }

        if ($root->val > $key) {
            $root->left = $this->deleteNode($root->left, $key);
        } else if ($root->val < $key) {
            $root->right = $this->deleteNode($root->right, $key);
        } else {
            // 找到刪除目標
            // 若只有一個子節點
            if ($root->left === null) {
                return $root->right; // 用右子樹代替
            } else if ($root->right === null) {
                return $root->left; // 用左子樹代替
            }
            // 若有兩個節點，找出左子樹最大節點
            $maxNode = $this->findMax($root->left);
            // 將欲刪除的節點值以左子樹最大節點的值代替
            $root->val = $maxNode->val;
            // 遞迴刪除左子樹最大節點
            $root->left = $this->deleteNode($root->left, $maxNode->val);
        }

        return $root;
    }

    private function findMax($node) {
        while ($node->right !== null) {
            $node = $node->right;
        }
        return $node;
    }
}
```

### 演算法複雜度

- **Time complexity:**
  - 最差情況：O(n) （退化成鏈表）
  - 最佳情況：O(log n) （平衡二元樹）
- **Space complexity:** O(1)

## 1.3. 解法優化
