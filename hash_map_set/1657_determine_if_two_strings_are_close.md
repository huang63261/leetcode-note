# 1657. Determine if Two Strings Are Close

## 1.1. 解題思路

- **目標**：
  - 判斷兩個字符串是否可以通過以下操作轉換為彼此：
    - 操作 1：交換任意兩個字符的位置。
    - 操作 2：將所有某字符 x 替換為另一字符 y，並同時將 y 替換為 x（字符互換操作）。
  - 若兩字符串符合條件，返回 `true`；反之，`false`

- **限制**：
  - 無

- **問題拆分**
  - 分析操作 1，代表著無論順序如何，字串元素相同即可。所以可以透過檢查兩字符串長度是否相同即可。
  - 操作 2 代表我們透過比對兩字符串中的元素出現頻率，就可以知道是否符合條件。

- **做法**
  - 使用兩個哈希表來分別記錄 `word1` 和 `word2` 中每個字符的頻率。
  - 使用 `array_diff_key` 檢查 `word1` 和 `word2` 的字符集合是否一致。
  - 提取每個字符的頻率值，進行排序，檢查兩者是否完全相等。
  - 綜合條件判斷：
    - 如果字符集合和頻率分佈均相等，則返回 true；否則返回 false。

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param String $word1
     * @param String $word2
     * @return Boolean
     */
    function closeStrings($word1, $word2) {
        $hash_map1 = [];
        $hash_map2 = [];

        if (strlen($word1) !== strlen($word2)) {
            return false;
        }

        for ($i = 0; $i < strlen($word1); $i++) {
            if (array_key_exists($word1[$i], $hash_map1)) {
                $hash_map1[$word1[$i]] += 1;
            } else {
                $hash_map1[$word1[$i]] = 1;
            }
        }

        for ($i = 0; $i < strlen($word2); $i++) {
            if (array_key_exists($word2[$i], $hash_map2)) {
                $hash_map2[$word2[$i]] += 1;
            } else {
                $hash_map2[$word2[$i]] = 1;
            }
        }

        $values1 = array_values($hash_map1);
        $values2 = array_values($hash_map2);
        sort($values1);
        sort($values2);

        if (array_diff_key($hash_map1, $hash_map2) || array_diff_key($hash_map2, $hash_map1)
            || $values1 !== $values2)
        {
            return false;
        }

        return true;
    }
}
```

## 1.3. 解法優化

1. 統計字符頻率時使用內建函數：
   - 使用 `count_chars($word, 1)` 替代手動統計，代碼更簡潔。
2. 字符集合比較可以簡化：
   - 直接比較 `array_keys($freq1) 和 array_keys($freq2c)`。
3. 頻率分佈比較可以優化：
   - 對頻率哈希表做排序 `sort` 後，即可驗證兩字的頻率分佈是否完全相同。（p.s. 因經過 `sort` 對 value 值排序後，index 已被重置為數字索引）

```php
class Solution {

    /**
     * @param String $word1
     * @param String $word2
     * @return Boolean
     */
    function closeStrings($word1, $word2) {
        if (strlen($word1) !== strlen($word2)) {
            return false;
        }

        $freq1 = count_chars($word1, 1);
        $freq2 = count_chars($word2, 1);

        if (array_keys($freq1) !== array_keys($freq2)) {
            return false;
        }

        sort($freq1);
        sort($freq2);

        return $freq1 === $freq2;
    }
}
```
