### 排序算法

> 排序，意义上就是在一个乱序的数组上，经过一系列的操作，让这个数组按照定义的“大小”升序或者降序的排列。这个过程就是排序的过程。

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