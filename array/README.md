# Array

### [3Sum](https://leetcode.com/problems/3sum/description)

__Description:__

Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

__Note:__

The solution set must not contain duplicate triplets.

__Example:__

Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]

__Solution:__

Strategy: two pointer

Sort: O(nlogn)

Two level for loop: O(n^2)

Outer loop: loop each element in input array with index i.

Inner loop: 2sum ---- use two pointer left and right to loop remaining elements. left should start from i + 1 instead of 0 to avoid duplicate, right should start from len - 1, left < right. Since there are duplicate elements in the input array, we should check if the next element is equal to current element when update i, left and right index.

Total time complexity: O(n^2)

__Coding:__

```Java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    // corner case
    if(nums == null || nums.length == 0){
        return res;
    }
    int len = nums.length;
    Arrays.sort(nums);
    for(int i = 0; i < len - 2; i ++){
        if(i != 0 && nums[i] == nums[i - 1]){ // avoid duplicate
            continue;
        }
        int left = i + 1;
        int right = len - 1;
        while(left < right){
            if(nums[i] + nums[left] + nums[right] == 0){
                // add to res
                List<Integer> list = new ArrayList<Integer>();
                list.add(nums[i]);
                list.add(nums[left]);
                list.add(nums[right]);
                res.add(list);
                // simpler version
                // res.add(Arrays.asList(num[i], num[lo], num[hi]));
                do{left ++;}while(left < right && nums[left] == nums[left - 1]); // avoid duplicate
                do{right --;}while(left < right && nums[right] == nums[right + 1]); // avoid duplicate
            }
            else if(nums[i] + nums[left] + nums[right] < 0){
                left ++;
            }
            else{
                right --;
            }
        }
    }
    return res;
}
```

### [Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/description/)

__Description:__

Given a string, find the length of the longest substring T that contains at most k distinct characters.

__Example:__ 

Given s = “eceba” and k = 2,

T is "ece" which its length is 3.

__Solution 1:__

Strategy: two pointer: left and right. Loop on right from 0 to the length of input string _len_. If the number of distinct characters in current substring is larger than k, left ++. The size of HashMap can tell us how many distinct characters are in current substring.

Data structure: HashMap (key: character; value: the number of this character in current substring).

Time Complexity: O(n), n is the length of input string _len_.

Space Complexity: O(n) for HashMap.

__Coding:__

```Java
public int lengthOfLongestSubstringKDistinct(String s, int k) {
    // corner case
    if(s == null || k <= 0){
        return 0;
    }
    int len = s.length();
    if(len <= k){
        return len;
    }
    int maxLen = 0; // result
    HashMap<Character, Integer> map = new HashMap<Character, Integer>();
    int left = 0;
    int right = 0;
    while(right < len){
        char chr = s.charAt(right);
        // duplicate character => valid
        if(map.containsKey(chr)){
            map.put(chr, map.get(chr) + 1);
        }
        else{
            // garuantee at most K distinct characters
            while(map.size() >= k){
                char chl = s.charAt(left ++);
                map.put(chl, map.get(chl) - 1);
                if(map.get(chl) == 0){
                    map.remove(chl);
                }
            }
            map.put(chr, 1);
        }
        // update result maxLen
        maxLen = Math.max(maxLen, right - left + 1);
        right ++;
    }
    return maxLen;
}
```

__Solution 2:__

Strategy: two pointer: left and right. Loop on right from 0 to the length of input string. If the number of distinct characters in current substring is larger than k, left ++. __Use a variable _cnt_ to count how many distinct characters are in current substring.__

Data structure: array of int type with size 256.

Time Complexity: O(n), n is the length of input string.

Space Complexity: O(n) for array.

```Java
public int lengthOfLongestSubstringKDistinct(String s, int k) {
    // corner case
    if(s == null || k <= 0){
        return 0;
    }
    int len = s.length();
    if(len <= k){
        return len;
    }
    int maxLen = 0; // result
    int[] count = new int[256]; // count how many times each characters exist in current substring
    int cnt = 0; // count how many distinct characters are in current substring
    int left = 0;
    int right = 0;
    while(right < len){
        char chr = s.charAt(right);
        count[chr] ++;
        if(count[chr] == 1){ // a new distinct character
            cnt ++;
        }
        while(cnt > k){ // garuantee at most K distinct characters
            char chl = s.charAt(left ++);
            count[chl] --;
            if(count[chl] == 0){
                cnt --;
            }
        }
        // update result maxLen
        maxLen = Math.max(maxLen, right - left + 1);
        right ++;
    }
    return maxLen;
}
```
