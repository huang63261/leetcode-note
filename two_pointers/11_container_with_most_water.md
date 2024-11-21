# 11. Container With Most Water

## 1.1. 解題思路

- **目標**：
  - 給定一個整數陣列 height，其中 height[i] 表示第 𝑖 條垂直線的高度。找出兩條線，它們與 𝑥 軸 形成的容器可以存儲最多的水，並返回該最大面積。
- **限制**：
  - 容器的兩條邊必須是陣列中的兩條垂直線。
  - 容器不能傾斜。
- **問題拆分**：
  - 初始位置如何選擇
  - 如何計算可容納面積？
  - 如何更新可容納最大面積？
  - 如何調整指針來遍歷所有可能的組合？
- **做法**：
  - 使用 `雙指針` 技術解題
  - 由於要找出可容納"最大"面積，從陣列的左右兩邊開始找起。
  - 面積 = min(height[left], height[right]) * (right - left)
  - 每次計算出面積後，將其和目前最大面積比較，取較大的值。
  - 根據較短的線移動

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param Integer[] $height
     * @return Integer
     */
    function maxArea($height) {
        $left = 0;
        $right = count($height) - 1;
        $max = 0;

        while ($left < $right) {
            $area = ($right - $left) * min($height[$left], $height[$right]);
            $max = max($area, $max);

            if ($height[$left] < $height[$right]) {
                $left += 1;
            } else {
                $right -= 1;
            }
        }

        return $max;
    }
}
```

## 1.3. 解法優化
