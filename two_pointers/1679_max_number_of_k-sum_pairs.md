# 1679. Max Number of K-Sum Pairs

## 1.1. 解題思路

- **目標**：
  - 給定一個整數陣列 `nums` 和整數 `k`，每次選擇兩個數，其和等於 `k`，並將其移除。返回可以執行的最大操作次數。
- **限制**：
  - 使用過的數字需從陣列中移除，即數字僅能配對一次。
- **問題拆分**：
  - 數組是否需要排序？
  - 如何找到數對？
  - 如何計數和移除？
- **做法**：
  - 雜湊表：
    - 遍歷 `nums`，若當前值的補數 (`k - num`) 存在於雜湊表中，該補數計數減一，並增加配對次數；反之，將當前值紀錄在雜湊表中。
    - 返回配對總次數
  - 雙指針：
  - 需將 `nums` 正序排序後，判斷兩數和若等於 `k`，則配對次數加一；大於 `k`，右指針往左移動；小於 `k`，左指針往右移動。

## 1.2. 程式碼實作

### 雜湊表

- 複雜度分析
  - 時間複雜度： 𝑂(𝑛)，因為每個元素最多遍歷一次。
  - 空間複雜度： 𝑂(𝑛)，取決於哈希表的大小。

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @param Integer $k
     * @return Integer
     */
    function maxOperations($nums, $k) {
        $operations = 0;
        $hash_table = [];

        foreach ($nums as $num) {
            $complement = $k - $num;

            if (isset($hash_table[$complement]) && $hash_table[$complement] > 0) {
                $operations += 1;
                $hash_table[$complement] -= 1;
            } else {
                if (!isset($hash_table[$num])) {
                    $hash_table[$num] = 0;
                }
                $hash_table[$num] += 1;
            }
        }
        return $operations;
    }
}
```

### 雙指針

- 複雜度分析
  - **時間複雜度**：
    1. 排序部分
         - 雙指針解法的時間複雜度主要來源為排序
         - 假設整數陣列長度為 `𝑛`，主流排序（如快速排序、合併排序）的時間複雜度為 `𝑂(𝑛log⁡𝑛)`。

    2. 雙指針部分
         - 排序完成後，左右兩指針向中間移動，最多移動 𝑛 步。
         - 遍歷整組陣列的時間複雜度為 `𝑂(𝑛)`。

    整體時間複雜度為： `𝑂(𝑛log⁡𝑛) * 𝑂(𝑛) = 𝑂(𝑛log⁡𝑛)`

  - **空間複雜度**： `𝑂(1)`

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @param Integer $k
     * @return Integer
     */
    function maxOperations($nums, $k) {
        $left = 0;
        $right = count($nums) - 1;
        $operations = 0;
        sort($nums);

        while ($left < $right) {
            $sum = $nums[$left] + $nums[$right];

            if ($sum == $k) {
                $operations += 1;
                $left += 1;
                $right -= 1;
            } elseif ($sum < $k) {
                $left += 1;
            } else {
                $right -= 1;
            }
        }

        return $operations;
    }
}
```

## 1.3. 解法優化
