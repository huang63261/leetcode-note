# 841. Keys and Rooms

## 1.1. 解題思路

- **目標**
  - 確認是否能夠造訪所有房間，從房間 `0` 開始，並根據每個房間中的鑰匙進行探索。如果可以訪問所有房間，返回 `true`；否則返回 `false`。
- **限制**：
  - 每個房間內的鑰匙值是唯一的。
- **問題拆分**：
  - 如何搜尋模擬探索房間？
  - 如何紀錄房間已被探索？
- **做法**：
  - 使用 `BFS` 或是 `DFS` 探索房間
  - 使用 `visited` 布林陣列紀錄房間是否已被探索，避免重複探索。

## 1.2. 程式碼實作

### BFS

```php
class Solution {

    /**
     * @param Integer[][] $rooms
     * @return Boolean
     */
    function canVisitAllRooms($rooms) {
        $visited = array_fill(0, count($rooms), false);
        $queue = new SplQueue();
        $queue->enqueue(0);
        $visited[0] = true;
        $visitedCount = 1;

        while(!$queue->isEmpty()) {
            $room = $queue->dequeue();

            foreach ($rooms[$room] as $key) {
                if (!$visited[$key]) {
                    $queue->enqueue($key);
                    $visited[$key] = true;
                    $visitedCount++;
                }
            }
        }

        return count($rooms) === $visitedCount;
    }
}
```

### DFS

```php
class Solution {

    /**
     * @param Integer[][] $rooms
     * @return Boolean
     */
    function canVisitAllRooms($rooms) {
        $visited = array_fill(0, count($rooms), false);
        $this->dfs(0, $rooms, $visited);

        foreach ($visited as $visit) {
            if (!$visit) return false;
        }

        return true;
    }

    private function dfs($room, $rooms, &$visited) {
        $visited[$room] = true;

        foreach ($rooms[$room] as $key) {
            if (!$visited[$key]) {
                $this->dfs($key, $rooms, $visited);
            }
        }
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n+E)
  - E 為房間鑰匙總數
- **Space complexity:** O(n)

## 1.3. 解法優化
