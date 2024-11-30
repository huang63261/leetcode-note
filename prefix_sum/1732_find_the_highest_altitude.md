# 1732. Find the Highest Altitude

## 1.1. 解題思路

- **目標**：
  - 給定一個整數數組 `gain`，代表自行車手在每段路程中的淨海拔增益，計算在整個路程中的最高海拔。
    - 輸入：整數數組 `gain`，長度為 `𝑛`
    - 輸出：在整個路程中達到的最高海拔值。
- **限制**：
  - 無
- **問題拆分**：
  - 如何計算當前海拔
  - 如何記錄最高海拔
- **做法**：
  - 初始化 highest = 0，用於記錄最高海拔。
  - 初始化 altitude = 0，用於計算當前海拔。
  - 遍歷 gain 數組：
    - 每次更新當前海拔：altitude += gain[i]。
    - 將 highest 更新為 max(highest, altitude)。
  - 返回 highest。

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param Integer[] $gain
     * @return Integer
     */
    function largestAltitude($gain) {
        $highest = 0;
        $altitude = 0;

        foreach ($gain as $alt) {
            $altitude += $alt;
            $highest = max($highest, $altitude);
        }

        return $highest;
    }
}
```

## 1.3. 解法優化
