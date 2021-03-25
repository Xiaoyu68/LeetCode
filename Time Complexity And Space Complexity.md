## Time Complexity And Space Complexity

- 几个原则：
  - 只考虑最高项
  - 不考虑常数项和系数
- 时间复杂度为O(n): 双指针算法，打擂台算法，单调栈算法，单调队列算法等
- 三种双指针算法：相向双指针，背向双指针，同向双指针

- Valid Palindrome

  ```java
  public boolean isPaindrome(String s) {
    if (s == null) {
      return false;
    }
    
    int left = 0, right = s.length() - 1;
    while (left <= right) {
      while (left <= right && !isValid(s.charAt(left))) {
        left++;
      }
      while(left <= right && !isValid(charAt(right))) {
        right--;
      }
      if (left <= right && isEqual(s.charAt(right), s.charAt(left))) {
        return false;
      }
      left++;
      right--;
    }
    return true;
  }
  
  boolean isValid(char c) {
    return Character.isLetter(c) || Character.isDigit(c);
  }
  
  boolean isEqual(char c, char d) {
    return Character.toLowerCase(c) == Character.toLowerCase(d);
  }
  ```

  - Valid Palindrome II

  ```java
  Class Pair {
    int left, right;
    public Pair(int left, int right) {
      this. left = left;
      this.right = right;
    }
  }
  
  public boolean validPalindrome(String s) {
    if (s == null) {
      return false;
    }
    
    Pair pair = findDifference(0, s.length() - 1, String s);
    return isPalindrome(pair.left, pair.right - 1) || isPalindrome(pair.left + 1, pair.right);
  }
  
  private Pair findDifference(int left, int right, String s) {
    while (left < right && s.charAt(left) == s.charAt(right)) {
      left++;
      right--;
    }
    return new Pair(left, right);
  }
  
  private boolean isPalindrome(int left, int right) {
    Pair pair = findDifference(left, right);
    return pair.left >= pair.right;
  }
  ```

  - Two Sum

  ```java
  //hashtable O(n), O(n)
  // two pointer O(nlogn), O(1)
  public class Solution {
    class Pair implements Comparable<Pair> {
      int number, index;
      
      public Pair(int num, int index) {
        this.number = num;
        this.index = index;
      }
      
      public int compareTo(Pair other) {
        return number - other.number;
      }
    }
    /**
    * @param numbers: An array of Integer
    * @param target: target = numbers[index1] + numbers[index2]
    * @return: [index1, index2]
    */
    public int[] twoSum(int[] numbers, int target) {
      int[] result = {-1, -1};
      
      if (numbers = null) {
        return result;
      }
      
      Pair[] pairs = getSortedPairs(numbers);
      
      int left = 0, right = pairs.length - 1;
      while (left < right) {
        if (pairs[left].number + pair[right].number < target) {
          left++;
        } else if (pair[left].number + pairs[right].number > target) {
          right--;
        } else {
          result[0] = Math.min(pairs[left].index, pairs[right].index);
          result[1] = Math.max(pairs[left].index, pairs[right].index);
          return result;
        }
      }
      
      return result;
    }
    
    private Pair[] getSortedPairs(int[] numbers) {
      Pair[] pairs = new Pair[numbers.length];
      for (int i = 0; i < numbers.length; i++) {
        pairs[i] = new Pair(numbers[i], i);
      }
      Arrays.sort(pairs);
      return pairs;
    }
  }
  
  // hashmap
  public class Solution {
    public int[] twoSum(int[] numbers, int target) {
      HashMap<Integer, Integer> map = new HashMap<>();
      
      for (int i = 0; i < numbers.length; i++) {
        if(map.get(numbers[i]) != null) {
          int[] result = {map.get(numbers[i]), i};
          return result;
        }
        map.put(target - numbers[i], i);
      }
      
      int[] result = {};
      return result;
    }
  }
  ```

  