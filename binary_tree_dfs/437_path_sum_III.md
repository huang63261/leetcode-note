# 437. Path Sum III

## 1.1. 解題思路

- **目標**：
  - 給定一棵二叉樹的根節點 `root` 和一個整數 `targetSum`，找出所有從某個節點出發向下（只能從父節點到子節點）的路徑，其節點值的總和等於 `targetSum`。
- **限制**：
  - 每個路徑的方向只能是從父節點到子節點，不能逆向。
- **問題拆分**：
  - 如何遍歷整個二元樹？
  - 如何計算所有可能路徑？
- **做法**：
  - 遞迴訪問每個節點：
    - 每個節點都會檢查自身是否符合 `targetSum` 的條件。
    - 遞迴訪問左右子樹，將左右子樹的路徑和返回，並與當前節點值相加。
  - 路徑和的累計與檢查：
    - 維護一個數組 `leftSum` 和 `rightSum`，記錄從子樹傳遞上來的所有路徑和。
    - 對於左右子樹的每條路徑和，加上當前節點的值，並檢查是否等於 `targetSum`。
  - 合併子路徑和：
    - 將左右子樹的路徑和與當前節點的值合併，作為新的路徑和，傳遞給父節點。
  - 計數：
    - 遞迴返回時，累加當前節點計算出的路徑數，以及左右子樹的路徑數。

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
     * @param Integer $targetSum
     * @return Integer
     */
    function pathSum($root, $targetSum) {
        [$count, $sum] = $this->dfs($root, $targetSum);
        return $count;
    }

    function dfs($root, $targetSum) {
        if ($root === null) {
            return [0, []];
        }

        $count = 0;

        [$leftCount, $leftSum] = $this->dfs($root->left, $targetSum);

        foreach ($leftSum as $index => $preSum) {
            $sum = $root->val + $preSum;
            if ($sum == $targetSum) {
                $count++;
            }
            $leftSum[$index] = $sum;
        }

        [$rightCount, $rightSum] = $this->dfs($root->right, $targetSum);

        foreach ($rightSum as $index => $preSum) {
            $sum = $root->val + $preSum;
            if ($sum == $targetSum) {
                $count++;
            }
            $rightSum[$index] = $sum;
        }

        $sum = array_merge($leftSum, $rightSum);
        $sum[] = $root->val;

        if ($root->val == $targetSum) {
            $count++;
        }

        return [$count + $leftCount + $rightCount, $sum];
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n ^ 2)
- **Space complexity:** O(n ^ 2)

## 1.3. 解法優化

- 每個節點的路徑和列表需要遍歷和更新，執行效率差
- 使用前綴和記錄當前路徑的累計和，可以快速計算以某節點為終點的路徑和。
  - 核心公式：
    - 如果路徑和 `sum[i,j] = prefix[j] − prefix[i−1]`，當 `prefix[j] − targetSum = prefix[i−1]`，則找到一條符合條件的路徑。

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
     * @param Integer $targetSum
     * @return Integer
     */
    function pathSum($root, $targetSum) {
        // 初始化前綴和哈希表，表示從根節點到節點的和
        $prefixSum = [0 => 1];
        return $this->dfs($root, 0, $targetSum, $prefixSum);
    }

    function dfs($root, $currentSum, $targetSum, &$prefixSum) {
        if ($root == null) {
            return 0;
        }

        // 計算根節點至當前節點總和
        $currentSum += $root->val;

        // 計算符合條件的路徑數量
        $count = $prefixSum[$currentSum - $targetSum] ?? 0;

        // 更新哈希表中的前綴和
        $prefixSum[$currentSum] = ($prefixSum[$currentSum] ?? 0) + 1;

        // 遞迴處理左右子樹
        $count += $this->dfs($root->left, $currentSum, $targetSum, $prefixSum);
        $count += $this->dfs($root->right, $currentSum, $targetSum, $prefixSum);

        // 回溯，移除當前路徑的前綴和，避免影響其他路徑
        $prefixSum[$currentSum]--;

        return $count;
    }
}
```

- **Time complexity:** O(n)
- **Space complexity:** O(n)
