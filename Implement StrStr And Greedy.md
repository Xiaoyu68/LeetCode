## Implement StrStr And Greedy

- Robin Karp

```java
// 计算出hashcode，与target的hashcode比较
// 计算后要取模，模出来的数越大，冲突越小
// 若出现负数，再加模的值
// 减完之后再模，和模了之后再减一样
// [(x * 31 + d) % 10^6 - a * 31^k] % 10^6   

public class Solution {
  
  public int BASE = 1000000;
  /**
  * @param source a source string
  * @param target a target string
  * @return an integer as index
  */
  public int strStr2(String source, String target) {
    if (source == null || target == null) {
      return -1;
    }
    
    int m = target.length();
    if (m == 0) {
      return 0;
    }
    
    //31^k
    int power = 1;
    for (int i = 0; i < m; i++) {
      power = (power * 31) % BASE;
    }
    
    int targetCode = 0;
    for (int i = 0; i < source.length(); i++) {
      // abc + d
      hashCode = (hashCode * 31 + source.charAt(i)) % BASE;
      if (i < m - 1) {
        continue;
      }
      
      // abcd - a
      if (i >= m) {
        hashCode = hashCode - power * source.charAt(i - m) % BASE;
        if(hashCode < 0) {
          hashCode += BASE;
        }
      }
      
      if (hashCode == targetCode) {
        // 不能写成==
        source.substring(i - m + 1, i + 1).equals(target);
      }
    }
  }
}
```

- JAVA语言特性

  Java中String类型具有一个equals的方法可以用于判断两周字符串是否相等，但是这种相等又与运算符“==”所判断的“相等”有所不同。
  
  - 使用“==”判断的相等时指相同的内存地址，也就是同一个对象实例
  - 使用equals方法判断的相等在不同的对象中实现不同，意义也不同
  
  Java的所有对象都继承自Object类，在Object类中实现的equals()方法如下：
  
  ```java
  public boolean equals(Object obj) {
    return (this == obj);
  }
  ```
  
  也就是等同于“==”，只有在内存一样的时候才返回true
  
  - String类重写了这个方法，重写后的方法首先判断地址是否一致，如果一致返回true，否则比较字符串的内容是否一致，如果内容一致返回true。因此，使用String类的equals方法是比较内容是否一致，而使用“==”是比较实例是否是同一个实例。
  - StringBuilder类并没有重写equals方法，因此使用equals比较时，需要时同一个实例才会返回true。否则返回false。
  
  用“=”赋值时，如果内存中已经有这个字符串，就会直接将其地址给这个字符串，不会产生新的字符串。当我们使用“+”或者“concat“方法拼接字符串的时候，会创建一个新的字符串，占用新的内存空间，因此使用”==“判断时返回false。
  
  String不支持下标索引的方式访问，所以需要使用charAt(i)的方式访问对应位置的字符。同时也就没有办法使用下标的方式对String进行修改。String是一种不可变类，字符串一但生成就不能被改变。例如我们使用**‘+’进行字符串连接，会产生新的字符串，原串不会发生任何变化；使用replace()**进行替换某些字符的时候也是产生新的字符串，不会更改原有字符串。
  
  但是c++中是可变的
  
  Java中一切皆对象，null用来表示空对象，我们不能对空对象做任何操作，除了‘=’和‘==’；""不是空对象，是指向实实在在的堆内存的。只是这段内存中没有数据而已，可以对“”做任何对字符串的操作。例如取长度、拼接、替换、查找字符等。
  
  

错误的贪心策略：

> 从A出发，选择里A最近的点X，走到X，然后选择离X最近的点Y，走到Y...

正确的贪心策略（Dijkstra算法）：

> 使用hashmap distance = {} 记录所有点到起点A的最短距离。一开始 distance = {A: 0}，代表目前只有A离起点的最短距离我们是确定知道的。然后在Distance中的点和非distance中的点中找到最小的一对X,Y, 使得 distance[X] + (X到Y的直接连接距离) 最小。其中X在distance里（已经被确认找到了最短距离），Y不在distance里（还没有被确认找到了最短距离）。然后将Y加入distance，并把距离设为 distance[X] + (X到Y的直接连接距离）

## 贪心算法：

- 加油站问题：

```java
public class Solution {
  public int canCompleteCircuit(int[] gas, int[] cost) {
    if (gas == null || cost == null || gas.length == 0 || cost.length == 0) {
      return -1;
    }
    
    int sum = 0;
    int total = 0;
    int index = -1;
    
    for (int i = 0; i < gas.length(); i++) {
      sum += gas[i] - cost[i];
      total += gas[i] - cost[i];
      if(sum < 0) {
        sum = 0;
        index = i;
      }
    }
    
    return total < 0 ? -1 : index + 1;
  }
}
```

- 创建最大数:

  ```java
  // 给出两个长度分别是m和n的数组来表示两个大整数，数组的每个元素都是数字0-9.
  // 从这两个数组当中选出k个数字来创建一个最大数，其中k满足k <= m + n.
  // 选出来的数字在创建的最大数里面的位置必须和在原数组内的相对位置一致。返回k个数的数组
  // 你应该尽可能的去优化时间和空间复杂度
  /**
  样例
  样例 1:
  
  输入：nums1 = [3, 4, 6, 5]， nums2 = [9, 1, 2, 5, 8, 3]，k = 5
  输出：[9, 8, 6, 5, 3]
  解释：
  从第一个数组选择[6, 5]，从第二个数组选择[9, 8, 3]
  样例 2:
  
  输入：nums1 = [6, 7]， nums2 = [6, 0, 4]，k = 5
  输出：[6, 7, 6, 0, 4]
  解释：
  从第一个数组选择[6, 7]，从第二个数组选择[6, 0, 4]
  样例 3:
  
  输入：nums1 = [3, 9]， nums2 = [8, 9]，k = 3
  输出：[9, 8, 9]
  解释：
  从第一个数组选择[9]，从第二个数组选择[8, 9]
  **/
  public class Solution {
    /**
    * @param nums1 an integer array of length m with digits 0-9
    * @param nums2 an integer array of length n with digits 0-9
    * @param k an integer and k <= m + n
    * @return integer array
    */
    public int[] maxNumber(int[] nums1, int[] nums2, int k) {
      if (k == 0) {
        return new int[0];
      }
      
      int m = nums1.length, n = nums2.length;
      if (m + n < k) return null;
      if (m + n == k) {
        int[] results = merge(nums1, nums2, k);
        return results;
      } else {
        int max = m >= k ? k : m;
        int min = n >= k ? 0 : k - n;
        
        int[] results = new int[k];
        for (int i = 0; i < k; i++) {
          results[i] = -0x7ffffff;
        }
        for (int i = min; i <= max; i++) {
          int[] temp = merge(getMax(nums1, i), getMax(nums2, k - i), k);
          results = isGreater(results, 0, temp, 0) ? results : temp;
        }
        return results;
      }
      
      int[] merge(int[] arr1, int[] arr2, int k) {
        int[] results = new int[k];
        if (k == 0) {
          return results;
        }
        int i = 0, j = 0;
        for (int l = 0; l < k; l++) {
          results[l] = isGreater(arr1, i, arr2, j)? arr1[i++] : arr2[j++];
        }
        
        return results;
      }
      
      boolean isGreater(int[] arr1, int i, int[] arr2, int j) {
        for (; i < arr1.length && j < arr2.length; i++, j++) {
          if (arr1[i] > arr2[j]) {
            return true;
          }
          if (arr1[i] < arr2[i]) {
            return false;
          }
        }
        
        return i != arr1.length 
      }
      
      int[] getMax(int[] arr1, int k) {
        if (k == 0) {
          return new int[0];
        }
        int[] results = new int[k];
        
        int i = 0;
        for (int j = 0; j < arr1.length; j++){
         while(arr1.length - j + i > k && i > 0 && results[i] < arr1[j]) {
           i--;
         }
         if (i < k) {
           results[i++] = arr1[j];
         }
        }
        
        return results;
      }
    }
  }
  ```

  

  