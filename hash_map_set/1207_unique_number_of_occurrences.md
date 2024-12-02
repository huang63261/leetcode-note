# 1207. Unique Number of Occurrences

## 1.1. 解題思路

- **目標**：
  - 判斷一個整數陣列中，每個數字的出現次數是否都是唯一的
- **限制**：
  - 無
- **問題拆分**：
  - 如何統計每個數字的出現次數？
  - 如何檢查出現次數是否唯一？

- **做法**
  1. 跑for迴圈將陣列中的數字出現次數紀錄在哈希表
  2. 檢查去 value 重複 `array_unique` 後的哈希表數量是否一樣

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param Integer[] $arr
     * @return Boolean
     */
    function uniqueOccurrences($arr) {
        $freq = [];

        foreach ($arr as $num) {
            if (!array_key_exists($num, $freq)) {
                $freq[$num] = 1;
            } else {
                $freq[$num]++;
            }
        }

        return count($freq) == count(array_unique($freq));
    }
}
```

- 演算法複雜度分析
  - **Time complexity:** O(n)
  - **Space complexity:** O(n)

## 1.3. 解法優化

1. 使用 PHP 內建方法 `array_count_values` 取代手動跑迴圈，使程式碼更為簡潔

```php
class Solution {

    /**
     * @param Integer[] $arr
     * @return Boolean
     */
    function uniqueOccurrences($arr) {
        $freq = array_count_values($arr);
        return count(array_unique($freq)) === count($freq);
    }
}
```
