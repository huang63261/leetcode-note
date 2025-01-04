# 1926. Nearest Exit from Entrance in Maze

## 1.1. 解題思路

- **目標**：
  - 找到從給定的迷宮入口到最近出口的最短步數，如果無法到達出口，返回 -1。
- **限制**：
  - 迷宮由 m x n 的矩陣表示，`maze[i][j]` 為空格 `'.'` 或牆壁 `'+'`。
  - 每一步只能上下左右移動到相鄰的單元格。
  - 無法穿越牆壁 '+'，也無法走出迷宮範圍。
  - 出口是邊界上的空格（不包括入口位置本身）。
- **問題拆分**：
  - 如何表示移動方向？
  - 如何判斷出口？
  - 如何標記已走訪的點？
  - 如何尋找最短路徑？
  - 終止條件為何？
- **做法**：
  - 初始化 BFS 所需數據結構：
    - 建立一個隊列，將入口位置和初始步數（0）加入隊列。
    - 定義四個方向的移動坐標。
  - 執行 BFS：
    - 從隊列中取出當前節點，嘗試向四個方向移動。
    - 如果移動到邊界且該點為空格，則判定為出口，返回當前步數 + 1。
    - 如果不是出口且未訪問過，將該點加入隊列並標記為訪問過。
  - 結束條件：
    - 如果隊列為空且未找到出口，返回 -1。

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param String[][] $maze
     * @param Integer[] $entrance
     * @return Integer
     */
    function nearestExit($maze, $entrance) {
        $m = count($maze);
        $n = count($maze[0]);
        $queue = new SplQueue();
        $directions = [[0, 1], [0, -1], [-1, 0], [1, 0]];
        [$startX, $startY] = $entrance;
        $queue->enqueue([$startX, $startY, 0]);
        // 標記為訪問過
        $maze[$startX][$startY] = '+';

        while (!$queue->isEmpty()) {
            [$currentX, $currentY, $steps] = $queue->dequeue();

            foreach ($directions as [$dx, $dy]) {
                $newX = $currentX + $dx;
                $newY = $currentY + $dy;

                //  確認是否仍在迷宮中 且 不是牆壁
                if ($newX >= 0 && $newY >= 0 && $newX < $m && $newY < $n && $maze[$newX][$newY] === '.') {
                    // 檢查是否到出口
                    if (($newX == 0 || $newY == 0 || $newX == $m - 1 || $newY == $n - 1) && !($startX == $newX && $startY == $newY)) {
                        return $steps + 1;
                    }
                    // 將當前節點加入隊列並標記為訪問
                    $maze[$newX][$newY] = '+';
                    $queue->enqueue([$newX, $newY, $steps + 1]);
                }
            }
        }

        return -1;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(m x n)
- **Space complexity:** O(m x n)

## 1.3. 解法優化
