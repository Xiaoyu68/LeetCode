## Quick Sort And Merge Sort

-  快速排序

快排中，等于标杆数的数，既可以在左边，又可以在右边，这是为了避免一种极端情况，即所有的数都相同，分布均匀。好坏都是O(nlogn)

```java
public class Solution {
  /**
  * @param A an integer array
  * @return void
  */
  public void sortIntegers(int[] A) {
    if (A == null || A.length == 0) {
      return;
    }
    
    quickSort(A, 0, A.length - 1);
  }
  
  private void quickSort(int[] A, int start, int end) {
    if (start >= end) {
      return;
    }
    
    int left = start, right = end;
    // 1. pivot的选取，不能是首， 不能是尾，不要选index
    int pivot = A[(start + end) / 2];
    //更理想的状态是取一个random的index
    
    // 2. left <= right not left < right
    while (left <= right) {
      // 3. A[left] < pivot not <=
      while (left <= right && A[left] < pivot) {
        left++;
      }
      while (left <= right && A[right] > pivot) {
        right--;
      }
      if (left <= right) {
        int temp = A[left];
        A[left] = A[right];
        A[right] = temp;
        left++;
        right--;
      }
    }
    
    quickSort(A, start, right);
    quickSort(A, left, end);
  }
}
```

- 归并排序

开辟出一块空间再回收一块空间很耗费时间

```java 
public void sortIntegers(int[] A) {
  if (A == null || A.length == 0) {
    return;
  }
  
  int[] temp = new int[A.length];
  mergeSort(A, 0, A.length - 1, temp);
}

private void mergeSort(int[] A, int start, int end, int[] temp) {
  if (start >= end) {
    return;
  }
  
  mergeSort(A, start, (start + end) / 2, temp);
  mergeSort(A, (start + end) / 2, end, temp);
  merge(A, start, (start + end) / 2, end, temp);
}

private void merge(int[] A, int start, int end, int[] temp) {
  int left = start;
  int right = (start + end) / 2 + 1;
  int index = left;
  
  while (left <= (start + end) / 2 && right <= end) {
    if(A[left] < A[right]) {
      temp[index++] = A[left++];
    } else {
      temp[index++] = A[right++];
    }
    
    while (left <= (start + end) / 2) {
      temp[index] = A[left++];
    }
    
    while (right <= end) {
      temp[index++] = A[right++];
    }
    
    for (int i = start; i <= end; i++) {
      A[i] = temp[i];
    }
  }
}
```

- 快排和归并的比较

1. 时间复杂度都是O(nlogn)， 快排的平均复杂度是O(nlogn)