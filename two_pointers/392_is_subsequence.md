# 392. Is Subsequence

## 1.1. 解題思路

- **目標**：
  - 有兩字串 `s` 和 `t`，判斷 `s` 是否為 `t` 的子序列。若 `s` 中的字符皆存在於 `t` 中（無需完全相符），且順序相符即返回 true；反之，返回 false。
- **限制**：
  - 暫無
- **問題拆分**：
  - 如何遍歷比對 `s` 及 `t`
  - 遍歷比對達成條件怎麼設定
- **做法**：
  1. 使用雙指針 `s_pointer` 和 `t_pointer` 分別遍歷 `s` 以及 `t` 。每當於 `t` 中找到當前 `s_pointer` 指向之 value ，就向右繼續找。
  2. 在遍歷後，若 `s_pointer`與 `s` 長度相符，代表符合條件並返回 true；反之返回 false。

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param String $s
     * @param String $t
     * @return Boolean
     */
    function isSubsequence($s, $t) {
        $s_length = strlen($s);
        $s_pointer = 0;
        $t_length = strlen($t);
        $t_pointer = 0;

        if (!$s) {
            return true;
        }

        while ($s_pointer < $s_length) {
            while ($t_pointer < $t_length) {
                if ($s[$s_pointer] == $t[$t_pointer]) {
                    if ($s_pointer == $s_length - 1) {
                        return true;
                    }
                    $s_pointer += 1;
                }
                $t_pointer += 1;
            }
            return false;
        }

        return true;
    }
}
```

## 1.3. 解法優化

1. while loop 僅需一層且條件應該為：雙指針同時還小於該字串長度時。
2. 條件判斷可於遍歷字串後判斷 `s_pointer` 長度是否和 `s` 字串長度一樣。
3. 當 `s` 為空字串時不會進入while loop中，可直接於後方條件判斷中處理。

優化後程式碼：

```php
class Solution {

    /**
     * @param String $s
     * @param String $t
     * @return Boolean
     */
    function isSubsequence($s, $t) {
        $s_pointer = 0;
        $t_pointer = 0;

        while ($s_pointer < strlen($s) && $t_pointer < strlen($t)) {
            if ($s[$s_pointer] == $t[$t_pointer]) {
                $s_pointer += 1;
            }
            $t_pointer += 1;
        }
        return $s_pointer == strlen($s);
    }
}
```
