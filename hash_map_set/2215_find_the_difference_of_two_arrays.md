# 2215. Find the Difference of Two Arrays

## 1.1. 解題思路

- **目標**：
  - 找到兩個陣列 nums1 和 nums2 的區別，返回一個包含兩個子陣列的結果：
    - 第一個子陣列包含 nums1 中不在 nums2 中的所有不同元素。
    - 第二個子陣列包含 nums2 中不在 nums1 中的所有不同元素。

- **限制**
  - 返回的兩個子數組中的元素順序可以任意，但必須是唯一值。
- **問題拆分**：
  - 如何計算差集？
  - 如何去重？
- **做法**：
  - 使用 PHP function `array_diff` 算出差集。
  - 使用 PHP function `array_unique` 去重複。

## 1.2. 程式碼實作

```php
class Solution {

    /**
     * @param Integer[] $nums1
     * @param Integer[] $nums2
     * @return Integer[][]
     */
    function findDifference($nums1, $nums2) {
        return [array_unique(array_diff($nums1, $nums2)), array_unique(array_diff($nums2, $nums1))];
    }
}
```

## 1.3. 解法優化
