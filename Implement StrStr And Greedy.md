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