# 108. Convert Sorted Array to Binary Search Tree

## 1.1. 解題思路

- **目標**：
  - 將給定的遞增排序數組 `nums` 轉換為一棵高度平衡的二元搜尋樹 (BST)，並返回其根節點。
  - 高度平衡的定義：一棵二叉樹中的每個節點，其左右子樹的高度差不超過 1。
- **限制**：
  - `nums` 是嚴格遞增排序的數組。
- **問題拆分**：
  - 如何找出根節點？
  - 如何找左右子樹？
- **做法**：
  - 找出陣列中心作為樹的根節點，再以陣列中心分為左右子樹遞迴處理，最後返回根節點。

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
     * @param Integer[] $nums
     * @return TreeNode
     */
    function sortedArrayToBST($nums) {
        if (empty($nums)) {
            return null;
        }

        // 找出中點位置
        $rootIndex = (int) floor(count($nums)/2);
        $root = new TreeNode($nums[$rootIndex]);
        $leftSubTree = array_slice($nums, 0, $rootIndex);
        $rightSubTree = array_slice($nums, $rootIndex + 1);

        $root->left = $this->sortedArrayToBST($leftSubTree);
        $root->right = $this->sortedArrayToBST($rightSubTree);

        return $root;
    }
}
```

### 演算法複雜度

- **Time complexity:**
  - 每次遞迴調用 `array_slice`，該操作需要 O(n) 時間，其中 `n` 是數組長度。
  - 總共有 O(logn) 次遞歸，因為每次分割數組為一半。
  - 總時間複雜度：O(nlogn)。
- **Space complexity:** O(logn)

## 1.3. 解法優化

- 優化建議
  - `array_slice` 的使用會導致額外的空間和時間消耗。可以通過在遞迴中直接傳遞數組索引的方式來避免切片操作。

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
     * @param Integer[] $nums
     * @return TreeNode
     */
    function sortedArrayToBST($nums) {
        return $this->helper($nums, 0, count($nums) - 1);
    }

    private function helper($nums, $left, $right) {
        if ($left > $right) {
            return null;
        }

        // 找出中點位置
        $mid = intdiv($left + $right, 2);
        $root = new TreeNode($nums[$mid]);

        $root->left = $this->helper($nums, $left, $mid - 1);
        $root->right = $this->helper($nums, $mid + 1, $right);

        return $root;
    }
}
```

- 演算法複雜度
  - **Time complexity:** O(n)
  - **Space complexity:** O(logn)
