## Longest Palindromic Substring

1. Brute Force:

```java
O(n^3)的时间复杂度，解法略
```

2. 中心枚举:

```java
public class Solution {
  /**
  * @param s: input string
  * @return: the longest palindramic substring
  **/
  public String longestPalindrame(String s) {
    if (s == null) {
      return null;
    }
    
    String longest = ""
    for (int i = 0; i < s.length(); i++) {
      String oddPalindrome = getPalindromeFrom(s, i, i); //此处要注意用子函数
      if (longest.length() < oddPalindrome.length()) {
        longest = oddPalindrome;
      }
      
      String evenPalindrome = getPalindromeFrom(s, i, i + 1);
      if (longest.length() < evenPalindrome.length()) {
        longest = evenPalindrome;
      }
    }
    
    return longest;
  }
  
  private String getPalindromeFrom(String s, int left, int right) {
    while (left >= 0 && right < s.length()) {
      if (s.charAt(left) != s.charAt(right)) {
        break;      //这个逻辑可以及时跳出while循环，避免计数器的不必要增加
      }
      left--;
      right++;
    }
    
    return s.substring(left + 1, right);
  }
}
```

3. 动态规划

```java
public class Solution {
  /**
  * @param s: input string
  * @return: the longest palindramic substring
  */
  public String longestPalindrome(String s) {
    if (s == null || s.length() == 0) {
      return null;
    }
    
    int n = s.length();
    boolean[][] isPalindrome = new boolean[n][n];
    
    int longest = 1, start = 0;
    for (int i = 0; i <= n; i++) {
      isPalindrome[i][i] = true;
    }
    for (int i = 0; i < n; i++) {
      isPalindrome[i][i + 1] = s.charAt(i) == s.charAt(i + 1);
      if (isPalindrome[i][i + 1]) {
        start = i;
        longest = 2;
      }
    }
    
    for (int i = n - 1; i >= 0; i--) {    //排除了与上面重复的计算
      for (int j = i + 2; j < n; j++) {
        isPalindrome[i][j] = isPalindrome[i + 1][j - 1] && s.charAt(i) == s.charAt(j);
        if (j - i + 1 > longest) {
          longest = j - i + 1;
          start = i;
        }
      }
    }
    
    return s.substring(start, start + longest);
  }
}
```



## 代码质量：

- Bug Free:
  - 对入口函数的参数进行异常检测
  - 访问下标时，确保下标不越界
  - 访问对象的属性或者方法时，确保对象不空
  - 不用全局变量
- Coding Style:
  - 二元运算符两边加空格，单元运算符不加空格
  - 花括号和for, if之间要加空格，圆括号和if之间要加空格
  - 用空行分隔开不同的逻辑块
  - 逗号后面加空格
- Readability相关:
  - 函数名和变量名用1-2个单词作为名称
  - 确保一个函数内部不超过三层缩进
  - 多用子函数来减少入口函数的代码量
  - 多用continue少用if

## 错误点：

1. Java的string类型的长度要写做：String s = s.length()
2. s下标取index，s.charAt(1)
3. 重复用多次的逻辑块，可以写成子函数
4. 注意while进行逻辑判断时，计数器的变化
5. Java的substring要注意第二个参数是指index

