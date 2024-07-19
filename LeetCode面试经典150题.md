# LeetCode面试经典150题

## 数组/字符串

### 88. 合并两个有序数组

#### 题目

给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。

请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。

**注意：**最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 `0` ，应忽略。`nums2` 的长度为 `n` 。

**示例 1：**

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
```

**示例 2：**

```
输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
解释：需要合并 [1] 和 [] 。
合并结果是 [1] 。
```

**示例 3：**

```
输入：nums1 = [0], m = 0, nums2 = [1], n = 1
输出：[1]
解释：需要合并的数组是 [] 和 [1] 。
合并结果是 [1] 。
注意，因为 m = 0 ，所以 nums1 中没有元素。nums1 中仅存的 0 仅仅是为了确保合并结果可以顺利存放到 nums1 中。
```

#### 解题思路

题目要求按非递减排列并存放到数组1中，两个数组原本就是按照非递减排列的，如果直接按照从小到大比较，每次插入数组2里面元素都会对数组1重新排列，可以考虑从大到小比较

1. 将一个指针记录当前数组1中需要插入的位置，从`m+n-1`的索引值开始，每插入一个元素，指针减1
2. 从后往前逐个比较两数组元素，将较大的插入指针指向位置
3. 两数组任一元素全部比较完时停止比较
   - 数组2先比较完成，直接结束
   - 数组1先比较完成，将数组2还没比较元素插入

#### 实现

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // p记录指针位置，p1,p2分别为当前数组比较位置
        int p = m + n - 1;
        int p1 = m - 1;
        int p2 = n - 1;

        //比较二者元素，直到任一数组所有元素都进行比较
        while (p2 >= 0 && p1 >= 0) {
            if(nums1[p1] < nums2[p2]) {
                nums1[p] = nums2[p2];
                p2--;
            }else {
                nums1[p] = nums1[p1];
                p1--;
            }
            p--;
        }

        //p1所有元素比较完成，将数组2元素全部插入
        if(p2 >= 0) {
            for (int i = 0; i <= p2; i++) {
                nums1[i] = nums2[i];
            }
        }
    }
}
```

#### 题解

略

---





### 27. 移除元素

#### 题目

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素。元素的顺序可能发生改变。然后返回 `nums` 中与 `val` 不同的元素的数量。

假设 `nums` 中不等于 `val` 的元素数量为 `k`，要通过此题，您需要执行以下操作：

- 更改 `nums` 数组，使 `nums` 的前 `k` 个元素包含不等于 `val` 的元素。`nums` 的其余元素和 `nums` 的大小并不重要。
- 返回 `k`。

**用户评测：**

评测机将使用以下代码测试您的解决方案：

```
int[] nums = [...]; // 输入数组
int val = ...; // 要移除的值
int[] expectedNums = [...]; // 长度正确的预期答案。
                            // 它以不等于 val 的值排序。

int k = removeElement(nums, val); // 调用你的实现

assert k == expectedNums.length;
sort(nums, 0, k); // 排序 nums 的前 k 个元素
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有的断言都通过，你的解决方案将会 **通过**。

**示例 1：**

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2,_,_]
解释：你的函数函数应该返回 k = 2, 并且 nums 中的前两个元素均为 2。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

**示例 2：**

```
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3,_,_,_]
解释：你的函数应该返回 k = 5，并且 nums 中的前五个元素为 0,0,1,3,4。
注意这五个元素可以任意顺序返回。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

**提示：**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

#### 解题思路

1. 使用双指针分别记录需要插入的位置，当前遍历的数
2. 遍历数组所有数，如果遍历到的数不等于`val`时，将该数插入该插入位置，插入值+1
3. 返回 插入值指针存的数

#### 实现

```java
class Solution {
    public int removeElement(int[] nums, int val) {
       // 使用双指针，指针1指向需要插入位置，指针2代表需要比较的值
        int index1 = 0;
        
        for (int index2 = 0; index2 < nums.length; index2++) {
            if(nums[index2] != val) {
                nums[index1] = nums[index2];
                index1++;
            }
        }

        return index1;
    }
}
```

#### 题解

在上述解题思路上进行优化，同样是使用双指针，不同的是，从后往前遍历数，这样避免对数组整体进行移动，直到两指针进行交汇。

1. 使用两个指针分别在指向插入位置，遍历的数
2. 判断左指针指向的数，如果等于`val`，则将该值换为遍历的数，遍历指针往左移
3. 左指针指向的数不等于`val`，则左指针往右移

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        //使用双指针，左指针指向需要插入的位置，右指针指向遍历的位置。
        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            if(nums[left] == val) {
                nums[left] = nums[right];
                right--;
            } else {
                left++;
            }
        }

        return left;
    }
}
```

---



### 26. 删除有序数组中的重复项

#### 题目

给你一个 **非严格递增排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：

- 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
- 返回 `k` 。

**判题标准:**

系统会用下面的代码来测试你的题解:

```
int[] nums = [...]; // 输入数组
int[] expectedNums = [...]; // 长度正确的期望答案

int k = removeDuplicates(nums); // 调用

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有断言都通过，那么您的题解将被 **通过**。

**示例 1：**

```
输入：nums = [1,1,2]
输出：2, nums = [1,2,_]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。
```

**示例 2：**

```
输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4]
解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。
```

**提示：**

- `1 <= nums.length <= 3 * 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按 **非严格递增** 排列

#### 解题思路

类似27题的解题思路，使用双指针分别表示比较位置，遍历位置

1. 指针1从0索引开始，指针2从1索引开始
2. 判断两指针指向的值，是否相等
3. 如果相等，表示出现重复项，则遍历位置右移
4. 如果不相等则表示没有重复，将当前遍历位置赋给比较位置的下一位置
5. 返回比较位置指针的值

#### 实现

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        //使用双指针分别表示比较位置，遍历位置
        int index1 = 0, index2 = 1;

        while (index2 < nums.length) {
            if(nums[index1] == nums[index2]) {
                index2++;
            }else {
                nums[++index1] = nums[index2];
            }
        }

        return index1 + 1;
    }
}
```

#### 题解

优化一下，如果两个指针值相差值为1时，则表示两个相邻，这时，如果二者指向值不相等也不需要复制

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        //使用双指针分别表示比较位置，遍历位置
        int index1 = 0, index2 = 1;

        while (index2 < nums.length) {
            if(nums[index1] != nums[index2]) {
                if(index2 - index1 > 1) {
                    nums[index1 + 1] = nums[index2];
                }
                index1++;
            }

            index2++;

        }

        return index1 + 1;
    }
}
```

---



### 80. 删除有序数组中的重复项 II

#### 题目

给你一个有序数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使得出现次数超过两次的元素**只出现两次** ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

**说明：**

为什么返回数值是整数，但输出的答案是数组呢？

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

**示例 1：**

```
输入：nums = [1,1,1,2,2,3]
输出：5, nums = [1,1,2,2,3]
解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3。 不需要考虑数组中超出新长度后面的元素。
```

**示例 2：**

```
输入：nums = [0,0,1,1,1,1,2,3,3]
输出：7, nums = [0,0,1,1,2,3,3]
解释：函数应返回新长度 length = 7, 并且原数组的前七个元素被修改为 0, 0, 1, 1, 2, 3, 3。不需要考虑数组中超出新长度后面的元素。
```

**提示：**

- `1 <= nums.length <= 3 * 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按升序排列

#### 解题思路

1. 使用双指针，一个用于记录插入位置，一个用于记录遍历到的元素

2. 每次遍历元素时，和前一个进行对比，相等则计数加1
3. 不等则把计数重置
4. 查看计数值，只要计数值不超过2，则把遍历值赋给插入位置，插入位置后移

#### 实现

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        //记录每个数字出现次数
        int count = 1;
        //记录当前遍历数字位置
        int i = 1;
        //记录需要插入位置
        int index = 1;

        while (i < nums.length) {
            //判断遍历到的数是否相等，相等出现次数+1
            if(nums[i] == nums[i-1]) {
                count++;
            }else {
                count = 1;
            }

            //如果count次数小于2，将当前遍历数赋给插入位置，插入位置+1,
            if(count <= 2) {
                nums[index++] = nums[i];
            }

            i++;
        }
        
        return index;
    }
}
```

#### 题解

与上面用的方法类似，区别在于没有使用一个变量记录数字出现的次数，而是使用插入位置上一位和遍历到的数进行比较，相等说明重复数字超过2个不用处理，不等就把遍历到的数字复制过来。

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        //数组长度小于2，不用处理
        if(nums.length <= 2) {
            return nums.length;
        }

        int index1 = 2, index2 = 2;

        while (index2 < nums.length) {
            if(nums[index1 - 2] != nums[index2]) {
                nums[index1] = nums[index2];
                index1++;
            }

            index2++;
        }

        return index1;
    }
}
```



