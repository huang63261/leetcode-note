# 2352. Equal Row and Column Pairs

## 1.1. 解題思路

- **目標**：
  - 給定一個 `n x n` 的矩陣 `grid`，計算矩陣中相等的行和列對(pair)數量。行和列對被認為相等的條件是：它們包含相同的元素，且順序相同。輸出該相等對的總數。
- **限制**：
  - 無
- **問題拆分**：
  - 如何將行與列抽象為可比較的結構？
  - 如何高效比較行與列？
  - 如何計算總對數？

- **做法**
  - 將矩陣的行與列整理成標識並分別放入 `rows`, `columns`
  - 統計行和列標識的出現頻率
  - 計算相等對的總數

## 1.2. 程式碼實作

### 演算法複雜度

```php
class Solution {

    /**
     * @param Integer[][] $grid
     * @return Integer
     */
    function equalPairs($grid) {
        $columns = [];
        $rows = [];
        $length = count($grid);

        for ($i = 0; $i < $length; $i++) {
            $rows[] = implode('-', $grid[$i]) . '-';

            $c = '';
            for ($j = 0; $j < $length; $j++) {
                $c .= $grid[$j][$i] . '-';
            }
            $columns[] = $c;
        }

        $freq_rows = array_count_values($rows);
        $freq_columns = array_count_values($columns);
        $pairs = 0;

        foreach (array_unique(array_intersect($columns, $rows)) as $pair_num) {
            $pairs += $freq_rows[$pair_num] * $freq_columns[$pair_num];
        }

        return $pairs;
    }
}
```

- **Time complexity:** O(n ^ 2)
- **Space complexity:** O(n ^ 2)

## 1.3. 解法優化

1. 整理標識的方式優化。刪除了多餘的字符串拼接操作，減少了處理行數。
2. 避免了 `array_intersect` 和 `array_unique` 的冗餘操作，直接基於哈希表操作，提高效率。

```php
class Solution {

    /**
     * @param Integer[][] $grid
     * @return Integer
     */
    function equalPairs($grid) {
        $rows = [];
        $columns = [];
        $length = count($grid);

        for ($i = 0; $i < $length; $i++) {
            $colKey = [];
            for ($j = 0; $j < $length; $j++) {
                $colKey[] = $grid[$j][$i];
            }
            $rows[] = implode('-', $grid[$i]);
            $columns[] = implode('-', $colKey);
        }

        $freq_rows = array_count_values($rows);
        $freq_columns = array_count_values($columns);
        $pairs = 0;
        foreach ($freq_rows as $key => $row_count) {
            if (isset($freq_columns[$key])) {
                $pairs += $row_count * $freq_columns[$key];
            }
        }

        return $pairs;
    }
}
```

- **Time complexity:** O(n ^ 2)
- **Space complexity:** O(n ^ 2)
