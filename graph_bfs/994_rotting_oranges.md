# 994. Rotting Oranges

## 1.1. 解題思路

- **目標**：
  - 在一個 m x n 的網格中，找到將所有新鮮橘子（值為 1）腐爛（值為 2）所需的最小分鐘數。如果不可能，返回 -1。
- **限制**：
  - 每分鐘，只有與腐爛橘子上下左右相鄰的新鮮橘子會腐爛。
  - 網格的大小是 1 <= m, n <= 10。
  - 網格的值只能是 0（空單元格）、1（新鮮橘子）、2（腐爛橘子）。
- **問題拆分**：
  - 如何模擬橘子腐爛過程？
  - 最後如何判斷是否所有橘子已腐爛？
- **做法**：
  - 初始化 BFS 隊列：
    - 遍歷所有網格，將所有腐爛橘子加入列隊，並且紀錄新鮮橘子數量
  - 進行 BFS：
    - 使用隊列記錄當前所有腐爛橘子的位置。
    - 遍歷隊列，對每個腐爛橘子檢查其上下左右相鄰的橘子：
    - 如果相鄰橘子是新鮮的，將其腐爛並加入隊列，並將新鮮橘子的數量減 1。
  - 檢查新鮮橘子數量：
    - 如果 BFS 結束時仍有新鮮橘子，返回 -1。
    - 否則，返回 BFS 過程中的分鐘數。

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param Integer[][] $grid
     * @return Integer
     */
    function orangesRotting($grid) {
        $m = count($grid);
        $n = count($grid[0]);
        $queue = new SplQueue();
        $freshOranges = 0;

        // 將所有腐爛橘子加入 Queue
        for ($i = 0; $i < $m; $i++) {
            for ($j = 0; $j < $n; $j++) {
                if ($grid[$i][$j] === 2) {
                    $queue->enqueue([$i, $j]);
                } else if ($grid[$i][$j] === 1) {
                    $freshOranges++;
                }
            }
        }

        // 若沒有新鮮橘子，直接返回 0
        if ($freshOranges == 0) return 0;

        $minutes = 0;
        $direction = [[0, 1], [0, -1], [-1, 0], [1, 0]];

        // BFS 腐爛過程
        while (!$queue->isEmpty()) {
            $size = $queue->count();
            // 是否有腐爛
            $hasRotten = false;

            // 同一分鐘
            for ($i = 0; $i < $size; $i++) {
                [$currentX, $currentY] = $queue->dequeue();

                foreach ($direction as [$dx, $dy]) {
                    $newX = $currentX + $dx;
                    $newY = $currentY + $dy;

                    // 檢查位置是否有效 且 為新鮮橘子
                    if ($newX >= 0 && $newY >= 0 && $newX < $m && $newY < $n && $grid[$newX][$newY] == 1) {
                        $freshOranges--;
                        $grid[$newX][$newY] = 2;
                        $queue->enqueue([$newX, $newY]);
                        $hasRotten = true;
                    }
                }
            }
            // 若此輪有腐爛，分鐘數 + 1
            if ($hasRotten) $minutes++;
        }

        // 若沒有新鮮橘子，返回分鐘數
        return ($freshOranges == 0) ? $minutes : -1;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(m × n)
- **Space complexity:** O(m × n)

## 1.3. 解法優化
