# AlgorithmProblems

## [Array](https://github.com/yingyingzhuzhu/AlgorithmProblems/tree/master/array)
## [Tree / Graph](https://github.com/yingyingzhuzhu/AlgorithmProblems/tree/master/tree)


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


