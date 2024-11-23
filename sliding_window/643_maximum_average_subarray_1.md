# 643. Maximum Average Subarray I

## 1.1. 解題思路

- **目標**：
  - 給定一整數陣列 `nums`，其中包含 n 個整數，除此之外給定一整數 `k`。
  - 找出 `nums` 中連續 `k` 個數的最大平均值。
- **限制**：
  - 無
- **問題拆分**：
  - 暴力解法
    - 如何定義迴圈範圍？
- **做法**：
  - 暴力解法
    - 遍歷陣列，計算出平均值，並同時檢查是否為最大。
    - 於內迴圈檢查 `$sums[j]` 是否為 null
    - 返回最大平均值

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @param Integer $k
     * @return Float
     */
    function findMaxAverage($nums, $k) {
        $maximum = null;
        $length = count($nums);

        for ($i = 0; $i < $length; $i++) {
            $sum = 0;
            for ($j = $i; $j < $i + $k; $j++) {
                if (!isset($nums[$j])) {
                    return $maximum;
                }
                $sum += $nums[$j];
            }
            $ave = $sum / $k;
            if (!isset($maximum) || $ave > $maximum) {
                $maximum = $ave;
            }
        }

        return $maximum;
    }
}
```

## 1.3. 解法優化

- 暴力解法缺點
  - 其算法的 **時間複雜度** 為 `O(𝑛⋅𝑘)`，當 `𝑛` 和 `𝑘` 都很大時，效率極低。
- 優化方式：滑動窗口 (sliding window)
  1. 初始化： 計算第一個子數組的總和。
  2. 滑動窗口： 在後續的迭代中，移動窗口時僅需更新子數組的總和：
     - 減去窗口左端的數字。
     - 加上窗口右端的數字。
- 這樣的做法避免了內層循環，將時間複雜度降低到 `𝑂(𝑛)`。

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @param Integer $k
     * @return Float
     */
    function findMaxAverage($nums, $k) {
        $length = count($nums);

        // initiate first window
        $max_sum = $window = array_sum(array_slice($nums, 0, $k));

        for ($i = $k; $i < $length; $i++) {
            $window = $window - $nums[$i - $k] + $nums[$i];

            $max_sum = max($max_sum, $window);
        }

        return $max_sum / $k;
    }
}
```
