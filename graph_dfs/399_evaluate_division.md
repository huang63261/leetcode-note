# 399. Evaluate Division

## 1.1. 解題思路

- **目標**：
  - 根據給定的等式和數值，回答一系列查詢，判斷變數間的比值。如果查詢中變數不在已知範圍內，或者無法推導，則返回 -1.0。
- **限制**：
  - 每個方程都有效且不會產生矛盾。
  - 方程中涉及的變數可能不存在於查詢中。
  - 返回值必須是浮點數格式。

- **問題拆分**
  - 如何表示變數關係？
  - 如何處理查詢？
- **做法**：
  - 將變數與其比值關係建模為一張有向圖（關鍵點）
    - 如果 `A / B = x`，則圖中存在一條從 `A` 到 `B` 的邊，權重為 `x`，反向邊權重為 `1/x`。
  - 對於每個查詢 (`C`, `D`)，需判斷：
    - `C` 或 `D` 是否在圖中。如果任一不存在，返回 -1.0。
    - `C` 是否能通過某些路徑到達 `D`。如果無法到達，返回 -1.0。
    - 若能到達，計算從 `C` 到 `D` 的比值。

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param String[][] $equations
     * @param Float[] $values
     * @param String[][] $queries
     * @return Float[]
     */
    function calcEquation($equations, $values, $queries) {
        $graph = [];
        $output = [];

        for ($i = 0; $i < count($equations); $i++) {
            $a = $equations[$i][0];
            $b = $equations[$i][1];
            $graph[$a][$b] = $values[$i];
            $graph[$b][$a] = 1 / $values[$i];
        }

        foreach ($queries as [$c, $d]) {
            if (!isset($graph[$c]) || !isset($graph[$d])) {
                $output[] = -1.0;
            } else {
                $output[] = $this->dfs($c, $d, $graph, [], 1.0);
            }
        }

        return $output;
    }

    function dfs($current, $target, $graph, $visited, $value) {
        if ($current === $target) return $value;
        $visited[$current] = true;

        foreach($graph[$current] as $neighbor => $weight) {
            if (!$visited[$neighbor]) {
                $result = $this->dfs($neighbor, $target, $graph, $visited, $value * $weight);
            if ($result !== -1.0) return $result;
            }
        }

        // 找不到目標點
        return -1.0;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(E + Q * V)
  - `E` 為方程式數量；`Q` 為查詢數量；`V` 為變數數量。

- **Space complexity:**
  - 圖的鄰接表：O(E)。
  - 遞迴訪問記錄：O(V)。

## 1.3. 解法優化
