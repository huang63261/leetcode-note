# 283. Move Zeroes

## 1.1. 解題思路

- **目標**：
  - 在不將除了0以外的數值順序打亂之下，將輸入的陣列中的所有0移去陣列的尾端
- **限制**：
  - in-space
- **問題拆分**：
  - 要如何將0移到陣列後方？
  - 如何保持順序？
- **做法**：
  1. 由於有`in-space`限制，不可複製至新的陣列當中，採用`雙指標`技術。
  2. 遍歷陣列，將不為0的數值依序覆蓋至`input`陣列中，並同時計數不為零的數組有幾個，即可得出有多少個0。
  3. 最終將0使用`for loop`覆蓋至`input`陣列中。

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @return NULL
     */
    function moveZeroes(&$nums) {
        $length = count($nums);
        $read = 0;
        $write = 0;
        $non_zero_count = 0;

        while ($read < $length) {
            if ($nums[$read] !== 0) {
                $nums[$write++] = $nums[$read];
                $non_zero_count += 1;
                $read += 1;
            } else {
                $read += 1;
            }
        }

        for ($i = $length - 1; $i > $non_zero_count - 1; $i--) {
            $nums[$i] = 0;
        }
    }
}
```

## 1.3. 解法優化

1. 已使用了雙指針，無需再使用 `none_zero_count`
2. for loop 可使用 while loop 替代。無需使用原本的 for loop 倒序填充0方法，可直接使用 `write` 指針的位置往後填充0即可。
3. 合併 while loop 中的 `read` 指針自增邏輯

優化後程式碼：

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @return NULL
     */
    function moveZeroes(&$nums) {
        $length = count($nums);
        $read = 0;
        $write = 0;

        // 將非0數字依序從陣列首填入
        while ($read < $length) {
            if ($nums[$read] !== 0) {
                $nums[$write++] = $nums[$read];
            }
            $read += 1;
        }

        // 於陣列後方補0
        while ($write < $length) {
            $nums[$write++] = 0;
        }
    }
}
```
