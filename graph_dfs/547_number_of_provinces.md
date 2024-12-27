# 547. Number of Provinces

## 1.1. 解題思路

- **目標**：
  - 計算城市間的連通分量數量，也就是總共有多少個省份（Province）。每個省份是一組城市，其中每個城市都能直接或間接連接到其他城市。
- **限制**：
  - `n == isConnected.length`
  - `n == isConnected[i].length`
  - `isConnected[i][j]` 是 0 或 1。
  - `isConnected[i][i] == 1`（城市與自己直接相連）。
  - `isConnected[i][j] == isConnected[j][i]`（連接是對稱的）。
- **問題拆分**：
  - 如何劃分省份並記錄數量？
- **做法**：

## 1.2. 程式碼實作

### DFS

```php
class Solution {

    /**
     * @param Integer[][] $isConnected
     * @return Integer
     */
    function findCircleNum($isConnected) {
        $visited = array_fill(0, count($isConnected), false);
        $province = 0;

        // 遍歷各個城市
        foreach ($isConnected as $city => $connectedCity) {
            // 若城市尚未被造訪，代表找到新省份，並使用 DFS 往下找相關城市
            if (!$visited[$city]) {
                $province++;
                $this->dfs($city, $isConnected, $visited);
            }
        }

        return $province;
    }

    private function dfs($city, $graph, &$visited) {
        $visited[$city] = true;

        foreach ($graph[$city] as $neighbor => $isConnected) {
            if ($isConnected && !$visited[$neighbor]) {
                $this->dfs($neighbor, $graph, $visited);
            }
        }
    }
}
```

### BFS

```php
class Solution {

    /**
     * @param Integer[][] $isConnected
     * @return Integer
     */
    function findCircleNum($isConnected) {
        $n = count($isConnected);
        $visited = array_fill(0, $n, false);
        $provinces = 0;

        for ($i = 0; $i < $n; $i++) {
            if (!$visited[$i]) {
                $this->bfs($isConnected, $visited, $i);
                $provinces++;
            }
        }

        return $provinces;
    }

    private function bfs($isConnected, &$visited, $i) {
        $queue = new SplQueue();
        $queue->enqueue($i);

        while (!$queue->isEmpty()) {
            $city = $queue->dequeue();
            $visited[$city] = true;

            for ($j = 0; $j < count($isConnected); $j++) {
                if ($isConnected[$city][$j] === 1 && !$visited[$j]) {
                    $queue->enqueue($j);
                }
            }
        }
    }
}

```

### 演算法複雜度

- **Time complexity:**
  - 外層迴圈遍歷所有城市 O(n)。
  - 每個城市的鄰接節點檢查 O(n)，因此總計為 O(n ^ 2)
- **Space complexity:** O(n)

## 1.3. 解法優化
