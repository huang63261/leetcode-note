# 1466. Reorder Routes to Make All Paths Lead to the City Zero

## 1.1. 解題思路

- **目標**：
  - 計算最少需要改變的道路方向數量，讓每個城市都能到達城市 0。
- **限制**
  - connections.length = n−1
  - connections[i].length = 2
  - 0≤`ai`,`bi`≤n−1
  - `ai` != `bi`
- **問題拆分**：
  - 如何找出需要調整的邊？
    - 如果某條邊的方向是從父節點到子節點，這邊就需要改變方向，因為城市 0 的路徑應該是從子節點指向父節點。
  - 如何處理有向邊？
    - 初始的邊是有方向的，為了方便遍歷，可以將其視作無向邊來進行建模。
    - 在建模的過程中，為每條邊標記方向（例如用標誌 1 表示原始邊需要改變方向，用 0 表示人工邊不需要改變方向）。
- **做法**：
  - 透過 `Edge List` (`$connections`) 建立 `Adjacency List` 並同時建立人工邊
    - 原始邊：`graph[a][] = [b, 1]`（需要翻轉）
    - 人工邊：`graph[b][] = [a, 0]`（不需要翻轉）
  - 選擇 `DFS` 或 `BFS` 進行圖遍歷：
    - 在每次訪問一個節點時：
      - 將鄰居節點標記為已訪問。
      - 遍歷其所有鄰居。
      - 如果鄰居未被訪問，檢查當前邊的標誌：
        - 如果是原始邊（sign = 1），則需要翻轉，將 count 增加 1。
        - 如果是人工邊（sign = 0），則不需要翻轉。
      - 繼續遞迴或加入隊列。
  - 返回結果

## 1.2. 程式碼實作

### DFS

```php
class Solution {

    /**
     * @param Integer $n
     * @param Integer[][] $connections
     * @return Integer
     */
    function minReorder($n, $connections) {
        $graph = [];
        // generate adjacency list
        foreach ($connections as [$a, $b]) {
            // Original Edge
            $graph[$a][] = [$b, 1];
            // Artificial Edge
            $graph[$b][] = [$a, 0];
        }

        $visited = array_fill(0, $n, false);
        $count = 0;
        $this->dfs(0, $graph, $visited, $count);
        return $count;
    }

    private function dfs($node, $graph, &$visited, &$count) {
        $visited[$node] = true;

        foreach ($graph[$node] as [$neighbor, $needToChanged]) {
            if (!$visited[$neighbor]) {
                // Encounter Original Edge
                $count += $needToChanged;
                $this->dfs($neighbor, $graph, $visited, $count);
            }
        }
    }
}
```

### BFS

```php
class Solution {

    /**
     * @param Integer $n
     * @param Integer[][] $connections
     * @return Integer
     */
    function minReorder($n, $connections) {
        $graph = [];
        // generate adjacency list
        foreach ($connections as [$a, $b]) {
            // Original Edge
            $graph[$a][] = [$b, 1];
            // Artificial Edge
            $graph[$b][] = [$a, 0];
        }

        $visited = array_fill(0, $n, false);
        $count = 0;
        $queue = new SplQueue();
        $queue->enqueue(0);
        $visited[0] = true;

        while (!$queue->isEmpty()) {
            $node = $queue->dequeue();

            foreach ($graph[$node] as [$neighbor, $needToChange]) {
                if (!$visited[$neighbor]) {
                    $visited[$node] = true;
                    $count += $needToChange;
                    $queue->enqueue($neighbor);
                }
            }
        }

        return $count;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(n)

## 1.3. 解法優化
