# 1493. Longest Subarray of 1's After Deleting One Element

## 1.1. 解題思路

- **目標**：
  - 給定一個二進位的陣列，刪除一個元素後（必要），返回最長連續為 `1` 的子陣列長度。若無符合此條件的子陣列，返回 `0`。
- **限制**：
  - 刪除一個元素為必要
- **問題拆分**：
  - 如何找最長連續 `1` 的子陣列長度
- **做法**：
  - 使用 sliding window
  - for loop 中
    - 若 window 中 `0` 的數量不超過一，則向右擴增
    - 反之，縮減 window 直至 `0` 的數量等於一
  - 返回最大長度

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @return Integer
     */
    function longestSubarray($nums) {
        $max_length = 0;
        $left = 0;
        $zero = 0;

        for ($right = 0; $right < count($nums); $right++) {
            if ($nums[$right] == 0) {
                $zero++;
            }

            while ($zero > 1) {
                if ($nums[$left] == 0) {
                    $zero--;
                }
                $left++;
            }

            $max_length = max($max_length, $right - $left);
        }

        return $max_length;
    }
}
```

## 1.3. 解法優化
