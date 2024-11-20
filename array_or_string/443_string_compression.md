# 443. String Compression

## 1.1. 解題思路

- **目標**：壓縮字符陣列，並返回壓縮後的長度。
- **限制**：
  - 僅能使用常數額外空間(constant extra space)
  - 僅能原地壓縮
  - 重複字符使用字符和數字表示，如：`aa` -> `a2`
- **問題拆分**：
  - 怎麼找到重複的字符？
  - 怎麼原地更新字符？
  - 怎麼處理多位數的情況？
- **做法**：
  - 需原地操作的情形可使用`雙指針`技術，`read pointer` 以及 `write pointer`。使用`read pointer`來遍歷重複字符，並使用`write pointer`於原`$chars`中覆蓋上字符以及重複次數。而多位數可使用`str_split`方法分次寫入。

## 1.2. 程式碼實作

``` php
  class Solution {

    /**
     * @param String[] $chars
     * @return Integer
     */
    function compress(&$chars) {
        $read = 0;
        $write = 0;
        $length = count($chars);

        while ($read < $length) {
            $char = $chars[$read];
            $count = 0;

            // 統計字符數出現次數
            while ($read < $length && $chars[$read] == $char) {
                $count += 1;
                $read += 1;
            }

            // 寫入字符
            $chars[$write++] = $char;
            // 上方寫法等同於:
            // $chars[$write] = $char;
            // $write += 1;
            if ($count > 1) {
                foreach (str_split((string) $count) as $digit) {
                    $chars[$write++] = $digit;
                }
            }
        }

        return $write;
    }
}
```
