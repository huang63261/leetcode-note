# 1971. Find if Path Exists in Graph

## 1.1. 解題思路

- **目標**：
  - 確定從圖中的節點 `source` 是否可以到達節點 `destination`，如果可以，返回 `true`；否則返回 `false`。
- **限制**
  - 圖為雙向圖，且每對節點最多僅有一條邊。
  - 無重複邊，且無自循環（節點連接到自己）。
- **問題拆分**
  - 如何構建圖的結構？
  - 如何進行搜索？
- **做法**：
  - 根據 `edge` 產出 `Adjacency List` 鄰接表
  - 使用 `DFS` 或 `BFS` 進行搜索找出是否存在通往 `destination` 的路徑

## 1.2. 程式碼實作

### BFS 廣度優先搜尋

```php
class Solution {

    /**
     * @param Integer $n
     * @param Integer[][] $edges
     * @param Integer $source
     * @param Integer $destination
     * @return Boolean
     */
    function validPath($n, $edges, $source, $destination) {
        if ($source === $destination) {
            return true;
        }

        $adj = array_fill(0, $n, []);
        foreach ($edges as $edge) {
            $u = $edge[0];
            $v = $edge[1];
            $adj[$u][] = $v;
            $adj[$v][] = $u;
        }

        $queue = new SplQueue();
        $queue->enqueue($source);
        $visited = array_fill(0, $n, false);

        while (!$queue->isEmpty()) {
            $vertex = $queue->dequeue();

            foreach ($adj[$vertex] as $neighbor) {
                if ($neighbor === $destination) {
                    return true;
                }

                if (!$visited[$neighbor]) {
                    $visited[$neighbor] = true;
                    $queue->enqueue($neighbor);
                }
            }
        }

        return false;
    }
}
```

### DFS 深度優先搜尋

```php
class Solution {

    /**
     * @param Integer $n
     * @param Integer[][] $edges
     * @param Integer $source
     * @param Integer $destination
     * @return Boolean
     */
    function validPath($n, $edges, $source, $destination) {
        if ($source === $destination) {
            return true;
        }

        $graph = array_fill(0, $n, []);
        foreach ($edges as $edge) {
            $u = $edge[0];
            $v = $edge[1];
            $graph[$u][] = $v;
            $graph[$v][] = $u;
        }

        $visited = array_fill(0, $n, false);

        return $this->dfs($graph, $visited, $source, $destination);
    }

    private function dfs($graph, &$visited, $node, $destination) {
        if ($node === $destination) return true;
        $visited[$node] = true;

        foreach ($graph[$node] as $neighbor) {
            if (!$visited[$neighbor] && $this->dfs($graph, $visited, $neighbor, $destination)) {
                return true;
            }
        }

        return false;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(V+E)
- **Space complexity:** O(V+E)
- `V` 是節點數量； `E` 是邊的數量。

## 1.3. 解法優化
