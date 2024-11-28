# 1004. Max Consecutive Ones III

## 1.1. 解題思路

- **目標**
  - 給定一個二進位陣列以及一整數 `k`，可以反轉 `0 -> 1` `k` 次，得出最多連續 `1` 的數量。
- **限制**：
  - 無
- **問題拆分**：
  - sliding window
    - 迴圈條件
    - 擴增/縮減 sliding window 的條件為何
    - 如何更新最大數量
- **做法**：
  - while 迴圈直到 sliding window 右邊界到達陣列最後一位
    - 若 sliding window 中的 `0` 數量小於等於 `k`，`right` ++
    - 若 sliding window 中的 `0` 數量大於 `k`，`left` ++ 直到 `left` 小於等於 `k`

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @param Integer $k
     * @return Integer
     */
    function longestOnes($nums, $k) {
        $max = $zero = $left = $right = 0;

        for ($i = 0; $i < count($nums); $i++) {
            if ($nums[$right] == 0) {
                $zero++;
            }

            while ($zero > $k) {
                if ($nums[$left] == 0) {
                    $zero--;
                }
                $left++;
            }
            $right++;

            $max = max($max, $right - $left + 1);
        }

        return $max;
    }
}
```

## 1.3. 解法優化

- 上方原解法定義了 `$right` 以及迴圈 `$i`，會導致在迴圈末端時，取得值 `$nums[$right]` 時發生undefined offset 錯誤，返回 `0`。
- 最後在計算最大連續 `1` 時發生計算錯誤。

**解法**：

- 讓 `right` 直接作為 for 迴圈變量，避免越界。

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @param Integer $k
     * @return Integer
     */
    function longestOnes($nums, $k) {
        $max = $zero = $left = 0;

        for ($right = 0; $right < count($nums); $right++) {
            if ($nums[$right] == 0) {
                $zero++;
            }

            while ($zero > $k) {
                if ($nums[$left] == 0) {
                    $zero--;
                }
                $left++;
            }

            $max = max($max, $right - $left + 1);
        }

        return $max;
    }
}
```
