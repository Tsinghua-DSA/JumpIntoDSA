# 工具使用

- [100个gdb小技巧](https://wizardforcel.gitbooks.io/100-gdb-tips/content/index.html)
- [GDB Cheatsheet](https://gabriellesc.github.io/teaching/resources/GDB-cheat-sheet.pdf)
- [CS107 GDB and Debugging(stanford.edu)](https://web.stanford.edu/class/cs107/resources/gdb.html)

## 练习

用gdb单步追踪下面的三份代码。

尝试在这三份代码中改出一些bug，然后再追踪，观察代码的执行过程发生了什么变化。

### 二分查找
```c++
#include <iostream>
#include <vector>

// 二分查找函数
int binarySearch(const std::vector<int>& arr, int left, int right, int x) {
    while (left <= right) {
        int mid = left + (right - left) / 2;

        // 检查x是否在中间
        if (arr[mid] == x) {
            return mid;
        }

        // 如果x更大，则只能在右侧查找
        if (arr[mid] < x) {
            left = mid + 1;
        }
        // 如果x更小，则只能在左侧查找
        else {
            right = mid - 1;
        }
    }

    // 如果元素不存在，则返回-1
    return -1;
}

// 打印数组函数
void printArray(const std::vector<int>& arr) {
    for (int num : arr) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
}

// 主函数
int main() {
    std::vector<int> arr = {2, 3, 4, 10, 40};
    int x = 10; // 要查找的元素
    int n = arr.size();
    int result = binarySearch(arr, 0, n - 1, x);
    if (result == -1) {
        std::cout << "Element is not present in array" << std::endl;
    } else {
        std::cout << "Element is present at index " << result << std::endl;
    }
    return 0;
}

```

### 冒泡排序
```c++
#include <iostream>
#include <vector>

// 冒泡排序函数
void bubbleSort(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // 交换 arr[j] 和 arr[j + 1]
                std::swap(arr[j], arr[j + 1]);
            }
        }
    }
}

// 打印数组函数
void printArray(const std::vector<int>& arr) {
    for (int num : arr) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
}

// 主函数
int main() {
    std::vector<int> arr = {64, 34, 25, 12, 22, 11, 90};
    std::cout << "Given array is \n";
    printArray(arr);

    bubbleSort(arr);

    std::cout << "Sorted array is \n";
    printArray(arr);
    return 0;
}

```

### 归并排序
```c++
#include <iostream>
#include <vector>

// 合并两个子数组
void merge(std::vector<int>& arr, int left, int middle, int right) {
    int n1 = middle - left + 1; // 左子数组的长度
    int n2 = right - middle;    // 右子数组的长度

    // 创建临时数组
    std::vector<int> L(n1), R(n2);

    // 复制数据到临时数组
    for (int i = 0; i < n1; i++)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[middle + 1 + j];

    // 合并临时数组回到原数组
    int i = 0; // 左子数组的起始索引
    int j = 0; // 右子数组的起始索引
    int k = left; // 合并后数组的起始索引

    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    // 复制剩余的元素
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

// 归并排序函数
void mergeSort(std::vector<int>& arr, int left, int right) {
    if (left < right) {
        // 找到中间索引
        int middle = left + (right - left) / 2;

        // 分别对左右子数组进行排序
        mergeSort(arr, left, middle);
        mergeSort(arr, middle + 1, right);

        // 合并两个子数组
        merge(arr, left, middle, right);
    }
}

// 打印数组函数
void printArray(const std::vector<int>& arr) {
    for (int num : arr) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
}

// 主函数
int main() {
    std::vector<int> arr = {12, 11, 13, 5, 6, 7};
    std::cout << "Given array is \n";
    printArray(arr);

    mergeSort(arr, 0, arr.size() - 1);

    std::cout << "Sorted array is \n";
    printArray(arr);
    return 0;
}

```