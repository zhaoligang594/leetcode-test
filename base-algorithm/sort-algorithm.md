### 排序算法

> 排序，意义上就是在一个乱序的数组上，经过一系列的操作，让这个数组按照定义的“大小”升序或者降序的排列。这个过程就是排序的过程。

[TOC]



#### 1. 快速排序

```java
    /**
     * 快速排序算法
     *
     * @param arr 待排序的属猪
     * @param l   左边位置
     * @param r   右边位置
     */
    private static void qSort(int[] arr, int l, int r) {
        if (l < r) {
            int partition = getPartition(arr, l, r);
            qSort(arr, l, partition - 1);
            qSort(arr, partition + 1, r);
        }
    }

    /**
     * 荷兰国旗的问题
     *
     * @param arr 排序数组
     * @param l   左边位置
     * @param r   右边位置
     * @return 返回分割好后的中间位置
     */
    private static int getPartition(int[] arr, int l, int r) {
        int temp = arr[l];
        while (l < r) {
            while (l < r && arr[r] >= temp) r--;
            arr[l] = arr[r];
            while (l < r && arr[l] <= temp) l++;
            arr[r] = arr[l];
        }
        arr[r] = temp;
        return l;
    }
```

> 快速排序算法他的时间复杂度是O(logN)，空间复杂度是O(1)。
>
> 存在的问题：有的时候，在快速排序的过程中，有可能会发生分配不均的问题，导致时间复杂度升高。这个时候，我们可以使用随机快速排序来解决这个问题。

#### 2. 归并排序

```java
    /**
     * 进行归并排序
     *
     * @param nums  带排序数组
     * @param left  左边位置
     * @param right 右边位置
     */
    private static void mergeSort(int[] nums, int left, int right) {
        if (left != right) {
            int mid = (left + right) / 2;
            mergeSort(nums, left, mid);
            mergeSort(nums, mid + 1, right);
            // 随后合并两个排序数组
            int[] sorted = new int[right - left + 1];
            int p1 = left, p2 = mid + 1;
            int p = 0;
            // 合并操作
            while (p1 <= mid || p2 <= right) {
                if (p1 > mid) {
                    sorted[p++] = nums[p2++];
                } else if (p2 > right) {
                    sorted[p++] = nums[p1++];
                } else {
                    if (nums[p1] < nums[p2]) {
                        sorted[p++] = nums[p1++];
                    } else {
                        sorted[p++] = nums[p2++];
                    }
                }
            }
            // 设置新值
            for (int k = 0; k < sorted.length; k++) {
                nums[left + k] = sorted[k];
            }
        }
    }
```

> 归并排序采用的也是分治的思想，时间复杂度也是O(logN). 