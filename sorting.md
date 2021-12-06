# Elementary sorts

### selection sort

O(n^2)

**it's called selection sort because it works by repeatedly selecting the smallest remaining element**每次选数组中的最小数，和尚未排好序的数组的第一个交换位置。

find the smallest element in array and exchange it with the first entry. then find the next smallest element and exchange it with the second entry. so on and so forth.

- **不稳定**（如2, 2, 1 排序后两个2相对位置就变了）
  - 解决方案：新建一个数组存放每次找到的最小值。
- **运行时间和数据分布无关**（最好、最坏时间都是O(n^2)）。running time is insensitive to input
  - it takes **as long** to run selection sort for an already-sorted array as it does for a randomly-ordered array
- **数据变动次数最少。**
  - 对于交换成本较高的任务，用选择排序（一共只要n次交换）

```java
class Solution {
    public int[] sortArray(int[] nums) {
        for(int i = 0; i<nums.length; i++){
            int min_idx= i;
            for(int j = i+1; j<nums.length; j++){
                if(nums[j]<nums[min_idx]){
                    min_idx = j;
                }
            }
            swap(nums, i, min_idx);
        }
        return nums;
    }
    private void swap(int[] nums, int i, int j){
        //不使用额外变量交换数组中两个值
        //避开交换同一个元素的情况，会让这个元素变成0
        if(i!= j){
            nums[i] = nums[i]^nums[j];
            nums[j] = nums[j]^nums[i];
            nums[i] = nums[i]^nums[j];
        }
    }
}
```



### insertion sort

- **在线排序算法**（在排序过程中还可以接受新数据）
- **稳定**
- 正序数组最好O(n)，逆序数组最差O(n^2)
- 适用于**基本有序**的数组

```java
class Solution {
    public int[] sortArray(int[] nums) {
        for(int i = 1; i<nums.length; i++){
            int j = i;
            int tmp = nums[j];
            while(j>=1 && nums[j-1]>tmp){
                nums[j] = nums[j-1];
                j--;
            }
            nums[j] = tmp;
        }
        return nums;
    }
}
```



### bubble sort

- **稳定**
- 正序数组最好O(n)，逆序数组最差O(n^2)
- 适用于**基本有序**的数组

用一个boolean保存一轮循环有没有交换过元素。最好情况O(n)

```java
class Solution {
    public int[] sortArray(int[] nums) {
        boolean switched = false;
        for(int i = 0; i<nums.length-1; i++){
            switched = false;
            for(int j = 0; j<nums.length-1-i; j++){
                //大于才交换。保证算法稳定性
                if(nums[j]>nums[j+1]){
                    swap(nums, j, j+1);
                    switched = true;
                }
            }
            if(switched == false) break;
        }
        return nums;
    }
    private void swap(int[] nums, int i, int j){
        //不适用额外变量交换数组中两个值
        nums[i] = nums[i]^nums[j];
        nums[j] = nums[i]^nums[j];
        nums[i] = nums[j]^nums[i];
    }
}
```



# Merge sort

- 稳定
- 需要额外空间
- 运行时间于数据分布无关（最好最差都是O(nlogn)）

```java
//mergesort初始版本，有大量的数组被创建和销毁
class Solution {
    public int[] sortArray(int[] nums) {
        return mergeSort(nums, 0, nums.length-1);

    }
    private int[] mergeSort(int[] nums, int left, int right){
        if(left>=right){
            return new int[]{nums[left]};
        }
        int mid = left+(right - left)/2;
        int[] leftArray = mergeSort(nums, left, mid);
        int[] rightArray = mergeSort(nums, mid+1, right);
        return merge(leftArray, rightArray);
    }
    private int[] merge(int[] leftArray, int[] rightArray){
        int i = 0, j = 0;
        int[] result = new int[leftArray.length+rightArray.length];
        while(i<leftArray.length&& j<rightArray.length){
            if(leftArray[i]<=rightArray[j]){
                result[i+j] = leftArray[i];
                i++;
            }
            else{
                result[i+j] = rightArray[j];
                j++;
            }
        }
        while(i<leftArray.length){
            result[i+j] = leftArray[i];
            i++;
        }
        while(j<rightArray.length){
            result[i+j] = rightArray[j];
            j++;
        }
        return result;
    }
}
//another merge function
private int[] merge(int[] leftArray, int[] rightArray){
        int i = 0, j = 0;
        int[] result = new int[leftArray.length+rightArray.length];
        //因为result的长度和左右数组的长度之和相等。所以当左数组空了，k还没走完时，右数组内必然还有数
        for(int k = 0; k<result.length; k++){
            if(i>=leftArray.length){
                result[k] = rightArray[j];
                j++; 
            }
            else if(j>=rightArray.length){
                result[k] = leftArray[i];
                i++;
            }
            else if(leftArray[i]<=rightArray[j]){
                result[k] = leftArray[i];
                i++;
            }
            else{
                result[k] = rightArray[j];
                j++;
            }
        }
        return result;
    }
```

```
//开辟一个和原数组一样大的辅助数组aux。避免频繁创建和删除数组。
class Solution {
    int[] aux;
    public int[] sortArray(int[] nums) {
        aux = new int[nums.length];
        mergeSort(nums, 0, nums.length-1);
        return nums;
    }
    private void mergeSort(int[] nums, int left, int right){
        if(left>=right){
            return;
        }
        int mid = left+(right -left)/2;
        mergeSort(nums, left, mid);
        mergeSort(nums, mid+1, right);
        merge(nums, left, mid, right);
    }
    private void merge(int[] nums, int left, int mid, int right){
        int i = left, j = mid+1;
        System.arraycopy(nums, left, aux, left, right-left+1);
        for(int k = left; k<=right; k++){
            if(i>mid){
                nums[k] = aux[j];
                j++;
            }
            else if(j>right){
                nums[k] = aux[i];
                i++;
            }
            else if(aux[i]<=aux[j]){
                nums[k] = aux[i];
                i++;
            }
            else{
                nums[k] = aux[j];
                j++;
            }
        }
    }
}
```



# Quick Sort

- 有序数组或全部相等最差 O(n^2)，其他情况O(nlogn)
- 不稳定

```java
class Solution {
    public int[] sortArray(int[] nums) {
        quickSort(nums, 0, nums.length-1);
        return nums;
    }
    private void quickSort(int[] nums, int left, int right){
        if(left>=right) return;
        int idx = partition(nums, left, right);
        quickSort(nums, left, idx-1);
        quickSort(nums, idx+1, right);
    }
    private int partition(int[] nums, int left, int right){
        //选取随机元素放置到正确位置
        int temp_idx = new Random().nextInt(right-left+1)+left;
        swap(nums, temp_idx, left);
        int i = left, j = right;
        while(i<j){
            while(i<j && nums[j]>=nums[left]){
                j--;
            }
            while(i<j && nums[i]<= nums[left]){
                i++;
            }
            swap(nums, i, j);
        }
        swap(nums, i, left);
        return i;
    }
    private void swap(int[] nums, int i, int j){
        if(i != j){
            nums[i] = nums[i]^nums[j];
            nums[j] = nums[i]^nums[j];
            nums[i] = nums[i]^nums[j];
        }
        
    }
}
```



# Heap Sort

- 不稳定
- 运行时间于数据分布无关（最好最差都是O(nlogn)）
- 0
- 1,2
- 3,4

```java
class Solution {
    public int[] sortArray(int[] nums) {
        heapify(nums);
        for(int i = nums.length-1; i>=1; i--){
            swap(nums, 0, i);
            siftDown(nums, 0, i-1);
        }
        return nums;
    }
    private void heapify(int[] nums){
        for(int i = (nums.length-2)/2; i>=0; i--){
            siftDown(nums, i, nums.length-1);
        }
    }
    //把大的换上来，小的换下去
    //维护大顶堆
    private void siftDown(int[] nums, int k, int end){
        while(k*2+1<=end){
            int child = k*2+1;
            if(child+1<=end && nums[child+1]>nums[child]){
                child++;
            }
            if(nums[k]<nums[child]){
                swap(nums, k, child);
            }
            else{
                break;
            }
            k = child;
        }
    }
    private void swap(int[] nums, int i, int j){
        if(i!= j){
            nums[i] = nums[i]^nums[j];
            nums[j] = nums[i]^nums[j];
            nums[i] = nums[i]^nums[j];
        }
    }
}
```



# 总结

- mergeSort归并排序O(nlogn)，heapSort堆排序O(nlogn)，selectionSort(O(n^2))选择排序运行时间都是固定的。
  - mergeSort相比于heapSort优势是稳定（保持相同元素的相对顺序）。
  - heapSort的优势：节省空间，原地排序O(1)空间复杂度

- 数组基本有序时，用bubbleSort冒泡排序和insertionSort插入排序，只需要O(n)复杂度



# Arrays.sort() 源码分析

当我们对基本数据类型数组排序时：

数据量小于47时，用的是insertion sort插入排序

数据量大于286且高度结构化时，采用类似Timsort排序

其他情况都用双轴快排。

当我们对引用数据类型排序时：

用TimSort
