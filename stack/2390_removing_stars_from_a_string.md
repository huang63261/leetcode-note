# 2390. Removing Stars From a String

## 1.1. 解題思路

- **目標**
  - 給定一個包含星號 `*` 的字符串 s，進行以下操作：
    - 每次選擇一個星號 `*`，刪除它左邊最近的非星號字符以及該星號本身
    - 直到刪除所有的星號後，返回剩餘的字符
- **限制**：
  - 星號的數量不會超過可刪除字符的數量
- **問題拆分**
  - 如何處理星號？
  - 如何刪除字符？
  - 如何保證順序？
  - 如何返回剩餘字符？
- **做法**：
  - 初始化一個空棧 stack。
  - 遍歷字符串：
    - 如果當前字符是星號 *，則從棧中彈出一個元素（模擬刪除最近的非星號字符）。
    - 否則，將該字符壓入棧中。
  - 將棧中剩餘的字符連接起來返回。

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param String $s
     * @return String
     */
    function removeStars($s) {
        $stack = [];

        for ($i = 0; $i < strlen($s); $i++) {
            if ($s[$i] === '*') {
                array_pop($stack);
            } else {
                array_push($stack, $s[$i]);
            }
        }

        return implode('', $stack);
    }
}
```

### 演算法複雜度

- **Time complexity:**
- **Space complexity:**

## 1.3. 解法優化
