---

layout: post
title: Quick Select的细节
date: 2017-06-22
tag: [algorithms]

---


目录

* TOC 
{:toc}


## 引言
**Quick Select**是发明**快速排序**的大师的另一杰作，Quick Select要解决的问题是在无序列表中找到第K小元素的选择算法，并且平均时间复杂度为$$O(n)$$.
与快速排序不同的是，Quick Select不是从两边递归进行查找，而是每次丢弃一边，递归进入另一边元素继续寻找。

## 原理
**Quick Select**的每一次parttion过程，将原问题的规模减半，所以$$T(n) = O(n) + T(\frac{n}{2})$$。
其中parttion过程的时间复杂度为$$O(n)$$.可以推导出$$T(n) = O(n)$$.


##  Quick Select用到的技巧
1.递归

2.双指针(对撞型)

## 细节漫谈
Quick Sort的细节基本上Quick Select都有，但是Quick Select有自己独特的细节。
在一次 parttion后，图为start==========right===========left======end.
那么在right指针和left指针之间至多有一个元素。
目标元素有三种情况：

1.start指针和right指针之间

2.left指针和end指针之间

3.刚好找到


## 例题

这里用找到无序列表的第K大元素和找到无序列表第k小元素分别来举例。

### 无序列表的第K大元素
*首先要审题，确实是从大到小，还是从小到大。显然这里是从大到小。*

Code:

```java
class Solution {
    /*
     * @param k : description of k
     * @param nums : array of nums
     * @return: description of return
     */
    public int kthLargestElement(int k, int[] nums) {
        // write your code here 
        if (nums == null || nums.length == 0) {
            return 0;
        }
        return quickSelect(nums, 0, nums.length - 1, k);
    }
    private int quickSelect(int[] nums, int start, int end, int k) {
        if (start == end) {
            return nums[start];
        }

        int left = start;
        int right = end;
        int pivot = nums[(start + end) / 2];
        
        while (left <= right) {
            while (left <= right && nums[left] > pivot) {
                left++;
            }
            while (left <= right && nums[right] < pivot) {
                right--;
            }
            if (left <= right) {
                int tmp = nums[left];
                nums[left] = nums[right];
                nums[right] = tmp;
                left++;
                right--;
            }
        }
        if (start + k - 1 <= right) {
            return quickSelect(nums, start, right, k);
        }
        if (start + k - 1 >= left) {
            return quickSelect(nums, left, end, k - (left - start));
        }
        return nums[right + 1];
    }
};
```
**细节**：注意最后的判断，因为right指针和 left指针中的元素个数未知，最坏情况是有一个元素，所以这里用的是偏移k - 1。

另外，如果元素落在了右边，那么只需再找第k - (left - start)大的元素。

### 无序数组第K小的元素

*首先要审题，确实是从大到小，还是从小到大。显然这里是从小到大。*

Code:
```java
class Solution {
    /*
     * @param k an integer
     * @param nums an integer array
     * @return kth smallest element
     */
    public int kthSmallest(int k, int[] nums) {
        // write your code here
        if (nums == null || nums.length == 0) {
            return -1;
        }
        return quickSelect(nums, 0, nums.length - 1, k);
    }
    private int quickSelect(int[] nums, int start, int end, int k) {
        if (start == end) {
            return nums[start];
        }
        int pivot = nums[(start + end) / 2];
        int left = start;
        int right = end;
        while (left <= right) {
            while (left <= right && nums[left] < pivot) {
                left++;
            }
            while (left <= right && nums[right] > pivot) {
                right--;
            }
            if (left <= right) {
                int tmp = nums[left];
                nums[left] = nums[right];
                nums[right] = tmp;
                left++;
                right--;
            }
        }
        if (start + k - 1 <= right) {
            return quickSelect(nums, start, right, k);
        }
        if (start + k - 1 >= left) {
            return quickSelect(nums, left, end, k - (left - start));
        }
        return nums[right + 1];
    }
};

```

## 彩蛋
Quick Select给的是一种解决问题的思路，这种题目可以借助**堆**这种数据结构，非常轻松的解决。


