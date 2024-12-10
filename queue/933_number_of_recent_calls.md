# 933. Number of Recent Calls

## 1.1. 解題思路

- **目標**
  - 實現一個計數器 `RecentCounter`，能夠計算最近 3000 毫秒內的請求數量。
    每次調用 `ping(t)` 時，返回 `[t−3000,t]` 時間範圍內的請求總數。
    確保能夠高效處理多次請求，並移除過期的請求。
- **限制**：
  - 無

- **問題拆分**
  - 如何保持請求數據的有序性並高效處理過期數據？
  - 如何提高範圍檢查的效率？
- **做法**：
  - 使用陣列模擬隊列
  - 每次調用 `pint(t)` 時：
    - 檢查隊首的請求是否已超過有效範圍。若超過，從隊列中移除。
    - 返回當前隊列中剩餘有效的請求總數。

## 1.2. 程式碼實作

```php
class RecentCounter {
    /**
     */
    function __construct() {
        $queue = [];
    }

    /**
     * @param Integer $t
     * @return Integer
     */
    function ping($t) {
        $this->queue[] = $t;

        while(!empty($this->queue) && $this->queue[0] < $t - 3000) {
            array_shift($this->queue);
        }

        return count($this->queue);
    }
}

/**
 * Your RecentCounter object will be instantiated and called as such:
 * $obj = RecentCounter();
 * $ret_1 = $obj->ping($t);
 */
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(n)
  - 儘管邏輯上在陣列中過期的請求會被移除，但實際上內存未釋放，佔用大小取決於所有歷史請求數量 n。

## 1.3. 解法優化

- 由於使用 `array_shift` 時，需要重新排列陣列的順序。
  - 當陣列很小時，重新排列所需成本可忽略不計
  - 但當陣列很大時，重新排列的成本代價就會變大
  - 所以可以使用 PHP 內建 `SplQueue` 數據結構來實現

```php
class RecentCounter {
    private $queue;

    /**
     */
    function __construct() {
        $this->queue = new SplQueue();
    }

    /**
     * @param Integer $t
     * @return Integer
     */
    function ping($t) {
        $this->queue->enqueue($t);

        while(!$this->queue->isEmpty() && $this->queue->bottom() < $t - 3000) {
            $this->queue->dequeue();
        }

        return $this->queue->count();
    }
}

/**
 * Your RecentCounter object will be instantiated and called as such:
 * $obj = RecentCounter();
 * $ret_1 = $obj->ping($t);
 */
```

- **Time complexity:** O(1)
- **Space complexity:** O(k)

## 1.4. 補充

使用 Linked List 實現 Queue

```php
class Node {
    public $data;
    public $next;

    public function __construct($data) {
        $this->data = $data;
        $this->next = null;
    }
}

class LinkedListQueue {
    private $head;
    private $tail;
    private $size;

    public function __construct() {
        $this->head = null;
        $this->tail = null;
        $this->size = 0;
    }

    // 入隊操作
    public function enqueue($data) {
        $newNode = new Node($data);

        if ($this->tail !== null) {
            $this->tail->next = $newNode;
        }
        $this->tail = $newNode;

        if ($this->head === null) {
            $this->head = $newNode;
        }

        $this->size++;
    }

    // 出隊操作
    public function dequeue() {
        if ($this->head === null) {
            throw new Exception("Queue is empty");
        }

        $data = $this->head->data;
        $this->head = $this->head->next;

        if ($this->head === null) {
            $this->tail = null;
        }

        $this->size--;
        return $data;
    }

    // 查看隊首元素
    public function peek() {
        if ($this->head === null) {
            throw new Exception("Queue is empty");
        }
        return $this->head->data;
    }

    // 獲取隊列大小
    public function getSize() {
        return $this->size;
    }

    // 判斷隊列是否為空
    public function isEmpty() {
        return $this->size === 0;
    }
}

class RecentCounter {
    private $queue;

    public function __construct() {
        $this->queue = new LinkedListQueue();
    }

    public function ping($t) {
        // 新請求加入隊列
        $this->queue->enqueue($t);

        // 移除所有過期請求
        while (!$this->queue->isEmpty() && $this->queue->peek() < $t - 3000) {
            $this->queue->dequeue();
        }

        // 返回有效請求數量
        return $this->queue->getSize();
    }
}
```
