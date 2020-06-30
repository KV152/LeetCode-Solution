# [1]Next Permutation (Medium)

## Contents
- [Solution Explanation](#solution-explanation)
  - [Problem description](#problem-description)
  - [Problem Analysis](#problem-analysis)
  - [(1)Two Pointer Approach](#1-two-pointer-approach) 
    - [Apporach Analysis](#apporach-analysis) 
- [C++ knowledge](#c-knowledge)


## Solution Explanation

### Problem description
Tag: [Array]\
Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be **in-place** and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```
### Problem Analysis
Lexicographically order is kind of dictionary order. Comparing each character one by one from the fornt to the end. eg: "abc"< "acb". The permutation of "abc" can be described as 
```
→ abc → acb → bac → bca → cab → cba →
```
Start from the ascending order and end in descending order. We can take "1, 2, 3, 4" as example:

```
→ 1,2,3,4 → 1,2,4,3 → 1,3,2,4 → 1,3,4,2 → 1,4,2,3 → 1,4,3,2 
→ 2,1,3,4 → 2,1,4,3 → 2,3,1,4 → 2,3,4,1 → 2,4,1,3 → 2,4,3,1 
→ 3,1,2,4 → 3,1,4,2 → 3,2,1,4 → 3,2,4,1 → 3,4,1,2 → 3,4,2,1 
→ 4,1,2,3 → 4,1,3,2 → 4,2,1,3 → 4,2,3,1 → 4,3,1,2 → 4,3,2,1 →
```

### (1) Two Pointer Approach
[Directly from leetcode solution](https://leetcode.com/problems/next-permutation/solution/)
#### Apporach Analysis
First, we observe that for any given sequence that is in descending order, no next larger permutation is possible. For example, no next permutation is possible for the following array:
```
[9, 5, 4, 3, 1]
```
We need to find the first pair of two successive numbers a[i] and a[i−1], from the right, which satisfy a[i]>a[i−1]. Now, no rearrangements to the right of a[i−1] can create a larger permutation since that subarray consists of numbers in descending order. Thus, we need to rearrange the numbers to the right of a[i−1] including itself.

Now, what kind of rearrangement will produce the next larger number? We want to create the permutation just larger than the current one. Therefore, we need to replace the number a[i−1] with the number which is just larger than itself among the numbers lying to its right section, say a[j].
![Image of nums_graph](https://github.com/KV152/LeetCode-Solution/blob/master/figures/31_nums_graph.png)
We swap the numbers a[i−1] and a[j]. We now have the correct number at index i−1i-1i−1. But still the current permutation isn't the permutation that we are looking for. We need the smallest permutation that can be formed by using the numbers only to the right of a[i−1]. Therefore, we need to place those numbers in ascending order to get their smallest permutation.

But, recall that while scanning the numbers from the right, we simply kept decrementing the index until we found the pair a[i] and a[i−1] where, a[i]>a[i−1]. Thus, all numbers to the right of a[i−1] were already sorted in descending order. Furthermore, swapping a[i−1] and a[j] didn't change that order. Therefore, we simply need to reverse the numbers following a[i−1] to get the next smallest lexicographic permutation.

The following animation will make things clearer:
![Image of nums_gif](https://github.com/KV152/LeetCode-Solution/blob/master/figures/31_Next_Permutation.gif)
## C++ knowledge

