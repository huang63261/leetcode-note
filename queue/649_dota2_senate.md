# 649. Dota2 Senate

## 1.1. 解題思路

- **目標**
  - 給定一陣列包含著 `R` 與 `D`。
  - 模擬 Dota2 的參議院投票程序，決定最後哪個黨派（`Radiant` 或 `Dire`）會宣布勝利。
    - 每位參議員根據給定順序參與投票。
    - 每位參議員可以選擇「剝奪另一方黨派參議員的權利」或「宣布勝利」。
    - 投票程序以回合制進行，直到某一方黨派的所有參議員失去權利。
- **限制**：
  - 每個字符只能是 `'R'（Radiant）`或 `'D'（Dire）`
  - 投票以回合制進行，直到滿足勝利條件，無其他提前退出的情況。
  - 每位參議員會選擇最佳策略，優先保護自己的黨派利益（剝奪對方的權利）。
- **問題拆分**：
  - 需考慮先後順序
  - 如何實現「剝奪另一方黨派參議員的權利」？
  - 如何判斷剩下的成員皆為同一黨派並宣佈勝利？
- **做法**：
  - 使用隊列可以解先後順序 `FIFO` 的題型
  - 將所有參議員入隊，同時紀錄兩黨派人員數 `r_count`, `d_count`
  - while 迴圈條件 `r_count > 0 && d_count > 0`：
    - dequeue 出參議員並利用 `r_banned`, `d_banned` 來查看是否有投票權&剝奪另一方黨派參議員的權利。
      - 若有，紀錄於 `r_banned` 或是 `d_banned`，並重新入隊
      - 若無，出隊後則不再入隊。另外黨派人數減一。
  - 返回剩餘人數大於 0 的黨派名稱

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param String $senate
     * @return String
     */
    function predictPartyVictory($senate) {
        $queue = new SplQueue();
        $r_count = 0;
        $d_count = 0;
        $r_banned = 0;
        $d_banned = 0;

        for ($i = 0; $i < strlen($senate); $i++) {
            if ($senate[$i] === 'R') {
                $r_count++;
            } else {
                $d_count++;
            }
            $queue->enqueue($senate[$i]);
        }

        while ($r_count > 0 && $d_count > 0) {
            $member = $queue->dequeue();

            if ($member === 'R') {
                if ($r_banned > 0) {
                    $r_count--;
                    $r_banned--;
                } else {
                    $d_banned++;
                    $queue->enqueue($member);
                }
            } else {
                if ($d_banned > 0) {
                    $d_count--;
                    $d_banned--;
                } else {
                    $r_banned++;
                    $queue->enqueue($member);
                }
            }
        }

        return $r_count > 0 ? "Radiant" : "Dire";
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(n)

## 1.3. 解法優化
