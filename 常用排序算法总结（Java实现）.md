排序算法比较：

![在这里插入图片描述](https://raw.githubusercontent.com/chuerat/MarkDownImages/master/SortingAlgorithmComparison.png)

##### 1. 冒泡排序

```java
/**
 * 冒泡排序
 * 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
 * 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。
 * 针对所有的元素重复以上的步骤，除了最后一个。
 * 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。
 *
 * @param array 需要排序的整型数组
 */
public void bubbleSort(int[] array) {
    int temp = 0;
    int len = array.length;

    for (int j = 0; j < len - 1; j++) {
        for (int i = 0; i < len - 1 - j; i++) {
            if (array[i] > array[i + 1]) {
                temp = array[i];
                array[i] = array[i + 1];
                array[i + 1] = temp;
            }
        }
    }
}
```

###### 冒泡排序的改进：鸡尾酒排序

```java
/**
 * 鸡尾酒排序（冒泡排序的改进）
 * 先找到最小的数字，把他放到第一位，然后找到最大的数字放到最后一位。
 * 再找到第二小的数字放到第二位，再找到第二大的数字放到倒数第二位。
 * 以此类推，直到完成排序。
 *
 * @param array 需要排序的整型数组
 */
public void cocktailSort(int[] array) {
    int temp = 0;
    int len = array.length;

    for (int j = 0; j < len / 2; j++) {
        //数组中最大的数向右冒泡
        for (int i = j; i < len - 1 - j; i++) {
            if (array[i] > array[i + 1]) {
                temp = array[i];
                array[i] = array[i + 1];
                array[i + 1] = temp;
            }
        }
        //数组中最小的数向左冒泡
        for (int i = len - 1 - (j + 1); i > j; i--) {
            if (array[i] > array[i + 1]) {
                temp = array[i];
                array[i] = array[i + 1];
                array[i + 1] = temp;
            }
        }
    }
}
```

##### 2. 快速排序

```java
/**
 * 查找出中轴（默认是最低位start）的在数组指定区间排序后所在位置
 *
 * @param array 待查找数组
 * @param start 开始位置
 * @param end 结束位置
 * @return 中轴所在位置
 */
public int partition(int[] array, int start, int end) {
    int key = array[start]; //中轴通常设置为序列的第一项

    while (start < end) {
        while (start < end && array[end] >= key)
            end--;
        array[start] = array[end];
        while (start < end && array[start] <= key)
            start++;
        array[end] = array[start];
    }
    array[start] = key;

    return start;
}

/**
 * 递归实现的快速排序
 * 典型的分而治之思想
 *
 * @param array 待排序数组
 * @param start 开始位置
 * @param end 结束位置
 */
public void quickSort(int[] array, int start, int end) {
    if (start < end) {
        int index = partition(array, start, end);
        quickSort(array, start, index - 1);
        quickSort(array, index + 1, end);
    }
}
```

##### 3. 选择排序

```java
/**
 * 选择排序算法
 * 在未排序序列中找到最小元素，存放到排序序列的起始位置
 * 再从剩余未排序元素中继续寻找最小元素，然后放到排序序列末尾。
 * 以此类推，直到所有元素均排序完毕。
 *
 * @param array 需要排序的整型数组
 */
public void selectSort(int array[]) {
    int len = array.length;
    int temp = 0;

    for (int i = 0; i < len - 1; i++) { //i为已排序序列的末尾
        int min = i;
        //找出未排序序列最小值所在的位置
        for (int j = i + 1; j < len; j++) {
            if (array[j] < array[min])
                min = j;
        }
        temp = array[i];
        array[i] = array[min];
        array[min] = temp;
    }
}
```

##### 4. 插入排序

```java
/**
 * 插入排序
 *
 * 类似抓扑克牌排序
 * 从第一个元素开始，该元素可以认为已经被排序
 * 取出下一个元素，在已经排序的元素序列中从后向前扫描
 * 如果该元素（已排序）大于新元素，将该元素移到下一位置
 * 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置
 * 将新元素插入到该位置中
 * 重复步骤2
 *
 * @param array 待排序数组
 */
public void insertSort(int[] array) {
    int len = array.length;
    int j, temp = 0;

    for (int i = 1; i < len; i++) {
        temp = array[i];
        for (j = i; j > 0 && temp < array[j - 1]; j--) {
            array[j] = array[j - 1];
        }
        array[j] = temp;
    }
}
```

###### 插入排序的改进：希尔排序

```java
/**
 * 希尔排序（插入排序的高效改进）
 *
 * 将整个有序序列分割成若干小的子序列分别进行插入排序。
 *
 * @param array 待排序数组
 */
public void shellSort(int[] array) {
    int len = array.length;
    int j, temp = 0;

    int increment = len; //设置初始步长
    while (true) {
        increment /= 2; //每次将步长缩短为原来的一半
        for (int x = 0; x < increment; x++) {
            for (int i = x + increment; i < len; i += increment) {
                temp = array[i];
                for (j = i - increment; j >= 0 && array[j] > temp; j -= increment) {
                    array[j + increment] = array[j];
                }
                array[j + increment] = temp;
            }
        }
        if (increment == 1)
            break;
    }
}
```



##### 5.归并排序

```java
/**
 * 归并排序
 *
 * 将两个（或两个以上）有序表合并成一个新的有序表，即把待排序序列分为若干个子序列，每个子序列是有序的。
 * 然后再把有序子序列合并为整体有序序列。
 *
 * 时间复杂度为O(nlogn)
 * 稳定排序方式
 *
 * @param array 待排序数组
 */
public void mergeSort(int[] array, int start, int end) {
    int mid = (start + end) / 2;

    if (start < end) {
        mergeSort(array, start, mid);
        mergeSort(array, mid + 1, end);
        merge(array, start, mid, end);
    }
}

/**
 * 将数组中low到high位置的数进行排序
 *
 * @param array 待排序数组
 * @param start 待排的开始位置
 * @param mid   待排中间位置
 * @param end   待排结束位置
 */
public void merge(int[] array, int start, int mid, int end) {
    int[] temp = new int[end - start + 1]; //辅助数组
    int i = start; //前一数组的起始元素
    int j = mid + 1; //后一数组的起始元素
    int k = 0;

    //把较小的数先移到新数组中
    while (i <= mid && j <= end) {
        if (array[i] <= array[j]) { //带等号保证归并排序的稳定性
            temp[k++] = array[i++];
        } else {
            temp[k++] = array[j++];
        }
    }

    //把左边剩余的数移入数组
    while (i <= mid) {
        temp[k++] = array[i++];
    }

    //把右边边剩余的数移入数组
    while (j <= end) {
        temp[k++] = array[j++];
    }

    //把新数组中的数覆盖array数组
    for (k = 0; k < temp.length; k++) {
        array[start + k] = temp[k];
    }
}
```

##### 6. 堆排序

```java
/**
 * 堆排序
 *
 * 由输入的无序数组构造一个最大堆，作为初始的无序区
 * 把堆顶元素（最大值）和堆尾元素互换
 * 把堆（无序区）的尺寸缩小1，并从新的堆顶元素开始进行堆调整
 * 重复步骤2，直到堆的尺寸为1
 *
 * 时间复杂度为O(nlogn)
 * 不稳定排序方式
 *
 * @param array 待排序数组
 */
public void heapSort(int[] array) {
    //最后一个非叶子节点
    int firstNonLeaf = array.length / 2 - 1;

    //构建大顶堆
    for (int i = firstNonLeaf; i >= 0; i--) {
        heapify(array, i, array.length);
    }

    //交换堆顶元素与末尾元素，并重新调整堆结构
    for (int j = array.length - 1; j > 0; j--) {
        swap(array, 0, j);
        heapify(array, 0, j);
    }
}

/**
  * 调整大顶堆
  *
  * @param array
  * @param i
  * @param size
  */
public void heapify(int[] array, int i, int size) {
    int left = i * 2 + 1;
    int right = i * 2 + 2;
    int largest = i;

    while (left < size) {
        if (array[left] > array[i]) {
            largest = left;
        }
        if (right < size && array[right] > array[largest]) {
            largest = right;
        }
        if (largest != i) {
            swap(array, largest, i);
        } else {
            break;
        }

        i = largest;
        left = i * 2 + 1;
        right = i * 2 + 2;
    }

}

public void swap(int[] array, int index1, int index2) {
    int tmp = array[index1];
    array[index1] = array[index2];
    array[index2] = tmp;
}
```

##### 思考：

> Java系统提供的Arrays.sort函数。对于基础类型，底层使用快速排序。对于非基础类型，底层使用归并排序。请问是为什么？

　　答：这是考虑到排序算法的稳定性。对于基础类型，相同值是无差别的，排序前后相同值的相对位置并不重要，所以选择更为高效的快速排序，尽管它是不稳定的排序算法；而对于非基础类型，排序前后相等实例的相对位置不宜改变，所以选择稳定的归并排序。
