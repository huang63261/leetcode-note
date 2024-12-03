# 735. Asteroid Collision

## 1.1. 解題思路

- **目標**
  - 給定一個整數陣列 asteroids，模擬小行星在一行中的運動和碰撞，返回最終剩餘的小行星。
  - 每個小行星的絕對值代表其大小，正負號代表其方向：
    - 正數表示向右運動；負數表示向左運動。
  - 同方向的小行星永遠不會相遇
  - 當兩個相向而行的小行星碰撞時：
    - 較小的小行星爆炸；若兩個小行星大小相同，則兩者都爆炸。

- **限制**：
  - 無

- **問題拆分**
  - 如何判斷小行星會碰撞？
  - 如何處理相向的小行星？

- **做法**：
  - 由左向右遍歷陣列，僅需考慮若棧頂是向右運動，且下一行星是向左運動時的情況。
  - 碰撞條件：
    - 棧中有小行星存在（即棧不為空）
    - 棧頂小行星是正數（向右運動）
    - 當前小行星是負數（向左運動）
  - 若碰撞發生時：
    - 若向左運動的小行星較大，則繼續向左運動
    - 若向右運動的小行星較大，則繼續遍歷
    - 若兩小行星相同，則發生爆炸（移除棧頂並標示向左運動小行星為0）
  - 完成遍歷，返回棧

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param Integer[] $asteroids
     * @return Integer[]
     */
    function asteroidCollision($asteroids) {
        $stack = [];

        foreach ($asteroids as $asteroid) {
            // 處理碰撞過程
            while (!empty($stack) && $stack[count($stack) - 1] > 0 && $asteroid < 0) {
                $top = $stack[count($stack) - 1];

                $top_size = abs($top);
                $asteroid_size = abs($asteroid);

                // 若向左小行星較大
                if ($top_size < $asteroid_size) {
                    array_pop($stack);
                } else if ($top_size == $asteroid_size){
                // 若兩小行星一樣大
                    array_pop($stack);
                    $asteroid = 0;
                    break;
                } else {
                // 若向右小行星比較大
                    $asteroid = 0;
                    break;
                }
            }

            if ($asteroid != 0) {
                array_push($stack, $asteroid);
            }
        }

        return $stack;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(n)

## 1.3. 解法優化
