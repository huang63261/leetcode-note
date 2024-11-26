# 1456. Maximum Number of Vowels in a Substring of Given Length

## 1.1. 解題思路

- **目標**
  - 給定一整數陣列 `s` 以及 一整數 `k`，返回在 `s` 中長度為 `k` 的連續子列中包含的最大的母音數量。
- **限制**：
  - 無
- **問題拆分**：
  - 是否使用 sliding window 解法？
  - 如何檢查母音？
    - 使用內回圈太過耗時

- **做法**
  - 使用 sliding window ，於初始化 sliding window 時先計算母音數量 `count`，同時記錄於`max`。
  - for 迴圈移動 sliding window
    - 檢查去除的元素若為母音，`count` 減一
    - 檢查新增的元素若為母音，`count` 加一
    - 比較最大母音數 `max`
  - 返回最大母音數量 `max`

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param String $s
     * @param Integer $k
     * @return Integer
     */
    function maxVowels($s, $k) {
        $count = 0;
        $vowels = ['a', 'e', 'i', 'o', 'u'];
        $s = str_split($s);
        $window = array_slice($s, 0, $k);

        foreach ($window as $letter) {
            if (in_array($letter, $vowels)) {
                $count++;
            }
        }
        $max = $count;

        for ($i = $k; $i < count($s); $i++) {
            if (in_array($s[$i], $vowels)) {
                $count++;
            }
            if (in_array($s[$i - $k], $vowels)) {
                $count--;
            }
            $max = max($count, $max);
        }

        return $max;
    }
}
```

## 1.3. 解法優化
