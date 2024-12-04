# 394. Decode String

## 1.1. 解題思路

- **目標**：
  - 給定一個編碼字符串，按照以下規則進行解碼，返回解碼後的字符串：
    - 編碼規則：
      - `k[encoded_string]`，表示將 `encoded_string` 重複 `k` 次。
    - 假設：
      - 輸入字符串是有效的，括號成對且格式正確。
      - 原始字符串不包含數字，數字只出現在重複次數中。
- **限制**：
- **問題拆分**：
  - 如何提取括號內的內容？
  - 如何處理嵌套括號？
- **做法**：
  - 遇到多層嵌套時，先處理最內層的括號，再處理外層：
    - 如 `"3[a2[c]]"`，應先解碼 `2[c]`，然後再處理外層的 `3[...]`。
  - 遇到 `[` 時，代表將開始一層解碼，將當前狀態放入棧中：
    - 當前字符串 `current_str`
    - 重複次數 `k`

    並重置當前次數 `times` 以其當前字符串 `current_str`
  - 遇到 `]` 時，將棧頂取出
    1. 前一次的字符串 `prev_str`
    2. 當前字符串應重複次數 `k`

    將當前字符串 `current_str` 重複 `k` 次，並加在 `prev_str` 後，且記錄於當前字符串 `current_str` 中

  - 遇到純字符串時，累加當前字符串至 `current_str`
  - 遇到數字時，將其紀錄於 `times`

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param String $s
     * @return String
     */
    function decodeString($s) {
        $stack = [];
        $number = 0;
        $current_str = '';

        for ($i = 0; $i < strlen($s); $i++) {
            $char = $s[$i];

            if (is_numeric($char)) {
                $number = $number * 10 + (int) $char;
            } else if ($char == '[') {
                array_push($stack, [$current_str, $number]);
                $current_str = '';
                $number = 0;
            } else if ($char == ']') {
                [$prev_str, $times] = array_pop($stack);
                $current_str = $prev_str . str_repeat($current_str, $times);
            } else {
                $current_str .= $char;
            }
        }

        return $current_str;
    }
}
```

### 演算法複雜度

- **Time complexity:** O(n)
- **Space complexity:** O(n)

## 1.3. 解法優化
