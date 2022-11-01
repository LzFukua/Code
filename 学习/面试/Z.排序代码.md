
### 各类排序思想
选择排序思想:一个数组内从头部开始找到最小数的与头部进行交换,然后缩短数组,再次循环

冒泡排序思想:一个数组内从头开始两两进行比较并判断是否交换位置,然后每循环完一次大的,就从后往前缩短数组. 因为会找到最大的数在最边上.

插入排序思想:拿起后一个数与前面的所有数进行比较交换位置,直到碰到比他小的才停下.(基本有序的时候效率很高)

希尔排序思想:跨格插入排序的思想,排完一遍再缩小跨格再排

归并排序思想:
将一个数组拆成若干组,拆到每个数字为一组,设立三个指针,两个指针指两个数组嘴开始的位置,一个指针指新数组的位置,于是数组邻近的两两开始比较,每被选出来的一个就往前动一个,然后新数组就添加一个,直到两边比完,多出来的直接赋到后面去
然后用到了递归的思想,不断的重复排序合并


快排思想:基于树形结构完成
选取一个值作为中心点,建立两个左右哨兵,左边动一下右边动一下吗,碰到比中心点大的就移动到右边,小的就移动到左边,直到两个哨兵碰面为止,然后继续将分好的两边区域递归重复同样的操作,直到被拆解得只有一个为止

堆排思想:把无序序列构造成二叉树,然后构造一个大顶堆,从下到上,从右到左的扫描方式依次把最大的数往上移到最顶上,然后取堆顶数字,再把末尾的数继续构造大顶堆,重复操作,直到取完所有的数.



### 二分查找
```java 
public class BinarySearch {

    // 二分查找，找到并返回下标
    public static int binarySearch(int[] arr, int n) {

        //定义一些变量
        int left = 0;
        int right = arr.length - 1;
        int mid = 0;

        while (left <= right) {
            // 求中间值
            mid = (left + right) / 2;
            if (arr[mid] == n) {
                return mid;
            } else if (arr[mid] <= n) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        // 表示未找到
        return -1;
    }
}

```

递归实现

```java

import java.util.Arrays;
/**
 * 二分查找算法
 */
public class BinarySearch {
    public int binarySearch2(int target, int[] nums, int left, int right) {
        int mid = (left + right)/2;
        if (nums.length == 0 || nums == null || left > right) {
            return -1;
        }
        while (left <= right) {
            if (target == nums[mid]) {
                return mid;
            } else if (target < nums[mid]) {
                return binarySearch2(target, nums, left, mid - 1);
            } else if (target > nums[mid]) {
                return binarySearch2(target, nums, mid + 1, right);
            }
        }
        return -1;
    }
 
    public static void main(String[] args) {
        BinarySearch bSearch = new BinarySearch();
        int target = 8;
        int[] nums = new int[]{9,3,4,6,8};
        Arrays.sort(nums);
        System.out.println("排序后的nums为：" + Arrays.toString(nums));
        int result1 = bSearch.binarySearch2(target, nums, 0, nums.length-1 );
        System.out.println("递归算法，目标" + target + "的下标是：" + result1);
    }
}
```


### 冒泡排序
每轮循环确定最值；

```java
public void bubbleSort(int[] nums){
    int temp;
    boolean isSort = false; //优化，发现排序好就退出
    for (int i = 0; i < nums.length-1; i++) {
        for (int j = 0; j < nums.length-1-i; j++) {  //每次排序后能确定较大值
            if(nums[j] > nums[j+1]){
                isSort = true;
                temp = nums[j];
                nums[j] = nums[j+1];
                nums[j+1] = temp;
            }
        }
        if(!isSort){
            return;
        } else {
            isSort = false;
        }
    }
}
``` 

### 选择排序
``` java 
每次选出最值，再交换到边上；
public void selectSort(int[] nums){
    for (int i = 0; i < nums.length-1; i++) {
        int index = i;
        int minNum = nums[i];
        for (int j = i+1; j < nums.length; j++) {
            if(nums[j] < minNum){
                minNum = nums[j];
                index = j;
            }
        }
        if(index != i){
            nums[index] = nums[i];
            nums[i] = minNum;
        }
    }
}
```


### 插入排序
```java
public void insertionSort(int[] nums){
    for (int i = 1; i < nums.length; i++) {
        int j = i;
        int insertNum = nums[i];
        while(j-1 >= 0 && nums[j-1] > insertNum){
            nums[j] = nums[j-1];
            j--;
        }
        nums[j] = insertNum;
    }
}

``` 

### 快速排序
选一个基本值，小于它的放一边，大于它的放另一边；
```java
public void quickSortDfs(int[] nums, int left, int right){
    if(left > right){
        return;
    }
    int l = left;
    int r = right;
    int baseNum = nums[left];
    while(l < r){
        //必须右边先走
        while(nums[r] >= baseNum && l < r){
            r--;
        }
        while(nums[l] <= baseNum && l < r){
            l++;
        }
        int temp = nums[l];
        nums[l] = nums[r];
        nums[r] = temp;
    }
    nums[left] = nums[l];
    nums[l] = baseNum;
    quickSortDfs(nums, left, r-1);
    quickSortDfs(nums, l+1, right);
}
``` 

### 归并排序
```java

//归
public void mergeSortDfs(int[] nums, int l, int r){
    if(l >= r){
        return;
    }
    int m = (l+r)/2;
    mergeSortDfs(nums, l, m);
    mergeSortDfs(nums, m+1, r);
    merge(nums, l, m, r);
}


//并
private void merge(int[] nums, int left, int mid, int right){
    int[] temp = new int[right-left+1];
    int l = left;
    int m = mid+1;
    int i = 0;
    while(l <= mid && m <= right){
        if(nums[l] < nums[m]){
            temp[i++] = nums[l++];
        } else {
            temp[i++] = nums[m++];
        }
    }
    while(l <= mid){
        temp[i++] = nums[l++];
    }
    while(m <= right){
        temp[i++] = nums[m++];
    }
}


```

### 堆排序
```java
public void heapSort2(int[] nums) {
    for(int i = nums.length/2-1; i >= 0; i--){
        sift(nums, i, nums.length);
    }
    for (int i = nums.length-1; i > 0; i--) {
        int temp = nums[0];
        nums[0] = nums[i];
        nums[i] = temp;
        sift(nums, 0, i);
    }
}

private void sift(int[] nums, int parent, int len) {
    int value = nums[parent];
    for (int child = 2*parent +1; child < len; child = child*2 +1) {
        if(child+1 < len && nums[child+1] > nums[child]){
            child++;
        }
        if(nums[child] > value){
            nums[parent] = nums[child];
            parent = child;
        } else {
            break;
        }
    }
    nums[parent] = value;
}
``` 