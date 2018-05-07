# Array

### [House Robber III](https://leetcode.com/problems/house-robber-iii/description/)

__Description:__

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

__Note:__

The solution set must not contain duplicate triplets.

__Example:__

1. Given tree = [3,2,3,null,3,null,1],

Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.

2. Given tree = [3,4,5,1,3,null,1],

Maximum amount of money the thief can rob = 4 + 5 = 9.



__Solution:__

Strategy: Divide and Conquer

Sort: O(# tree nodes)

For each node: if the thief robs it, then he cannot rob its children; if he does not rob it, then he can choose to rob its children or not base on max amount of money. So for each node, return value is max amount of money he can rob in this subtree in the situation that he rob this node and in one situation that he not rob this node (2 cases).

__Coding:__

```Java
public int rob(TreeNode root) {
    int[] res = robHelper(root);
    return Math.max(res[0], res[1]);
}
private int[] robHelper(TreeNode root){
    //[0]: count; [1]: not count
    if(root == null){
        return new int[]{0, 0};
    }
    int[] left = robHelper(root.left);
    int[] right = robHelper(root.right);
    int[] res = new int[2];
    res[1] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]); // If not count this node, then can choose count or not count its children, so select max.
    res[0] = root.val + left[1] + right[1]; // If count this node, then cannot count its children.
    return res;
}
```
