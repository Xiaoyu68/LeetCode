## 组合类DFS

求所有具体方案，可以想DFS

- subsets

```java
// method 1
public List<List<Integer>> subsets(int[] nums) {
  List<List<Integer>> results = new ArrayList<>();
  
  if (nums == null) {
    return results;
  }
  
  Arrays.sort(nums);
  dfs(nums, 0, new ArrayList<Integer>(), results);
}

private void dfs(int[] nums, int index, List<Integer> subset, List<List<Integer>> results) {
  if (index == nums.length) {
    results.add(new ArrayList<Integer>(subset));
    return;
  }
  
  subset.add(nums[index]);
  dfs(nums, index + 1, subset, results);
  
  subset.remove(subset.size() - 1);
  dfs(nums, index + 1, subset, results);
}

// method 2
public List<List<Integer>> subsets(int[] nums) {
  
List<List<Integer>> results = new ArrayList<>();
  if (nums == null) {
    return results;
  }
  
  Arrays.sort(nums);
  dfs(nums, 0, new ArrayList<Integer>(), results);
}

private void dfs(int[] nums, int index, List<Integer> subset, List<List<Integer>> results) {
   results.add(new ArrayList<Integer>(subset));
  
  for (int i = index; i < nums.length; i++) {
    subset.add(nums[i]);
    dfs(nums, i + 1, subset, results);
    subset.remove(subset.size() - 1);
  }
  
}
```

```java
// 有带重复的情况
public List<List<Integer>> subsets(int[] nums) {
  
List<List<Integer>> results = new ArrayList<>();
  if (nums == null) {
    return results;
  }
  
  Arrays.sort(nums);
  dfs(nums, 0, new ArrayList<Integer>(), results);
}

private void dfs(int[] nums, int index, List<Integer> subset, List<List<Integer>> results) {
   results.add(new ArrayList<Integer>(subset));
  
  for (int i = index; i < nums.length; i++) {
    if (i != 0 && nums[i] == nums[i - 1] && i > startIndex) {
      continue;
    }
    subset.add(nums[i]);
    dfs(nums, i + 1, subset, results);
    subset.remove(subset.size() - 1);
  }
  
}
```

