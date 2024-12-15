# 872. Leaf-Similar Trees

## 1.1. 解題思路

- **目標**：
  - 給定兩個二元樹，分別取得其二的樹葉順序（由左至右）並比對。若相同返回 `true`，反之 `false`。
- **限制**：
  - 無
- **問題拆分**：
  - 如何走訪樹？
  - 使用遞迴法還是非遞迴法（棧）？
- **做法**：
  - 非遞迴法 DFS 找出葉子順序並進行比對。

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
     * @param TreeNode $root1
     * @param TreeNode $root2
     * @return Boolean
     */
    function leafSimilar($root1, $root2) {
        return $this->getLeavesSequence($root1) === $this->getLeavesSequence($root2);
    }

    function getLeavesSequence($root) {
        $seq = [];
        $stack = [$root];

        while (!empty($stack)) {
            $node = array_pop($stack);

            if ($node->left == null && $node->right == null) {
                $seq[] = $node->val;
            } else {
                if ($node->right !== null) {
                    $stack[] = $node->right;
                }
                if ($node->left !== null){
                    $stack[] = $node->left;
                }
            }
        }

        return $seq;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(h)

## 1.3. 解法優化

無

## 1.4. 其他

### 遞迴法

#### 程式碼實作

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
     * @param TreeNode $root1
     * @param TreeNode $root2
     * @return Boolean
     */
    function leafSimilar($root1, $root2) {
        $seq1 = [];
        $seq2 = [];
        $this->getLeavesSequence($root1, $seq1);
        $this->getLeavesSequence($root2, $seq2);
        return $seq1 === $seq2;
    }

    function getLeavesSequence($root, &$value) {
        // DFS
        if ($root === null) {
            return;
        }
        if ($root->left == null && $root->right == null) {
            $value[] = $root->val;
            return;
        }

        $this->getLeavesSequence($root->left, $value);
        $this->getLeavesSequence($root->right, $value);
    }
}
```

- 演算法複雜度
  - **Time complexity:** O(n)
  - **Space complexity:** O(h)
