# 724. Find Pivot Index

## 1.1. 解題思路

- **目標**

  - 找到陣列的 `pivot index`，該索引的左側數字總和等於右側數字總和
    - 若 `pivot index` 存在，返回最左側的索引
    - 反之，返回 -1

- **限制**
  - 如果索引在陣列的邊界：
    - 左邊界：左側總和視為 0。
    - 右邊界：右側總和視為 0。
  - 陣列元素可以為正數、負數或零。

- **問題拆分**
  - 如何計算左側和右側的總和？
  - 如何判斷是否為樞軸索引 `pivot index`？
  - 如何處理邊界條件？

- **做法**
  - 初始時，左側總和為 0，右側總和為數組的總和。
  - 每次遍歷索引 `i` 時，更新左右總和：
    - 左側總和：累加 `nums[i−1]`。
    - 右側總和：減去 `nums[i]`。
  - 在每個索引 `i`，檢查是否滿足條件： `left_sum == right_sum`
    - 如果條件成立，返回該索引 `i`。
  - 邊界條件：
    - 在 `i=0` 時，左側總和為 0。
    - 在 `i=len(nums) - 1` 時，右側總和為 0。

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @return Integer
     */
    function pivotIndex($nums) {
        $left_sum = 0;
        $right_sum = array_sum($nums);

        for ($i = 0; $i < count($nums); $i++) {
            $left_sum += $nums[$i - 1];
            $right_sum -= $nums[$i];

            if ($right_sum == $left_sum) {
                return $i;
            }
        }

        return -1;
    }
}
```

## 1.3. 解法優化

### 代碼問題分析

#### 問題

1. 邏輯錯誤：
   1. `left_sum += $nums[$i - 1];` 在第一個索引 `i=0` 時，嘗試訪問 `nums[−1]`，這會導致未定義行為 `undefined behavior`。

#### 修正點說明

1. 將 `left_sum += $nums[$i - 1];` 改為 `left_sum += $nums[$i];` 並將其放在比較之後，確保當前索引的值只影響下一次迴圈。也可以防止未定義行為發生。

#### 修正後程式碼

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @return Integer
     */
    function pivotIndex($nums) {
        $left_sum = 0;
        $right_sum = array_sum($nums);

        for ($i = 0; $i < count($nums); $i++) {
            $right_sum -= $nums[$i];

            if ($right_sum == $left_sum) {
                return $i;
            }

            $left_sum += $nums[$i];
        }

        return -1;
    }
}
```
