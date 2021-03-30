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

  

  - 跳跃游戏

  ```java
  // 给出一个非负整数数组，你最初定位在数组的第一个位置
  // 数组中的每一个元素代表你在那个位置可以跳跃的最大长度
  // 判断你是否能够到达数组的最后一个位置
  /**
  样例
  样例 1
  
  输入 : [2,3,1,1,4]
  输出 : true
  样例 2
  
  输入 : [3,2,1,0,4]
  输出 : false
  挑战
  这个问题有两个方法，一个是贪心和 动态规划。
  
  贪心方法时间复杂度为O（N）。
  
  动态规划方法的时间复杂度为为O（n^2）。
  
  我们手动设置小型数据集，使大家可以通过测试的两种方式。这仅仅是为了让大家学会如何使用动态规划的方式解决此问题。如果您用动态规划的方式完成它，你可以尝试贪心法，以使其再次通过一次。
  **/
  public class Solution {
    public boolean canJump(int[] A) {
      if (A == null || A.length == 0) {
        return false;
      }
      int farthest = A[0];
      for (int i = 0; i < A.length; i++) {
        if (i <= farthest && i + A[i] > farthest) {
          farthest = i + A[i];
        }
      }
      return farthest >= A.length - 1;
    }
  }
  ```

  - 跳跃游戏2

  ```java
  // 给出一个非负整数数组，你最初定位在数组的第一个位置。
  // 数组中的每个元素代表你在那个位置可以跳跃的最大长度。　　　
  // 你的目标是使用最少的跳跃次数到达数组的最后一个位置。
  /**
  输入 : [2,3,1,1,4]
  输出 : 2
  解释 : 到达最后位置的最小跳跃次数是2(从下标0到1跳跃1个距离长度，然后跳跃3个距离长度到最后位置)
  **/
  public class Solution {
    public int jump(int[] A) {
      if (A == null || A.length == 0) {
        return -1;
      }
      
      int start = 0, end = 0, count = 0;
      while(end < A.length - 1) {
        count++;
        int farthest = end;
        for (int i = start; i < end; i++) {
          if (i + A[i] > farthest) {
            farthest = A[i] + i; 
          }
        }
        start = end + 1;
        end = farthest;
      }
      return count;
    }
  }
  ```

  - 删除数字

  ```java
  // 给出一个字符串A，表示一个n位正整数，删除其中k位数字，使得剩余的数字仍然按照原来的顺序排列产生一个新的正整数
  // 找到删除k个数字之后的最小正整数
  /**
  样例
  样例 1：
  
  输入: A = "178542", k = 4
  输出:"12"
  样例 2：
  
  输入: A = "568431", k = 3
  输出:"431"
  **/
  public class Solution {
    /**
    * @param A: A positive integer which has N digits, A is a string
    * @param k: Remove k digits
    * @return: A string
    */
    public String DeleteDigits(String A, int k) {
      StringBuffer sb = new StringBuffer(A);
      int i, j;
      for (i = 0; i < k; i++) {
        for(j = 0; j < A.length() - 1 && A.charAt(i) < A.charAt(i + 1); j++) {}
        sb.delete(j, j + 1);
      }
      while (sb.length() > 1 && sb.charAt(0) == '0') {
        sb.delete(0, 1);
      }
      
      return sb.toString;
    }
  }
  ```

  - 任务计划

  ```java
  //给定一个字符串，表示CPU需要执行的任务。 这个字符串由大写字母A到Z构成，不同的字母代表不同的任务。完成任务不需要按照给定的顺序。 每项任务都可以在一个单位时间内被完成。 在每个单位时间，CPU可以选择完成一个任务或是不工作。
  
  //但是，题目会给定一个非负的冷却时间“n”，表示在执行两个“相同的任务”之间，必须至少有n个单位时间，此时CPU不能执行该任务，只能执行其他任务或者不工作。
  
  //您需要返回CPU完成所有给定任务所需的最少单位时间数。
  /**
  样例1
  
  输入: tasks = ['A','A','A','B','B','B'], n = 2
  输出: 8
  解释:
  A -> B -> idle -> A -> B -> idle -> A -> B.
  样例2
  
  输入: tasks = ['A','A','A','B','B','B'], n = 1
  输出: 6
  解释:
  A -> B -> A -> B -> A -> B.
  **/
  public class Solution {
    /**
    * @param tasks: the given char array representing tasks CPU need to do
    * @param n: the non-negative cooling interval
    * @return: the least number of intervals the CPU will take to finish all the given tasks
    */
    public int leastInterval(char[] tasks, int n) {
      int[]  c = new int[26];
      for (int i = 0; i < tasks.length; i++) {
        c[tasks[i] - 'A']++;
      }
      
      Arrays.sort(c);
      int i = 25;
      while (i >= 0 && c[i] == c[25]) {
        i--;
      }
      return Math.max((c[25] - 1) * (n + 1) - 25 + i, tasks.length);
    }
  }
  ```

  需要背诵的贪心：
  
  https://www.jiuzhang.com/qa/2099/

