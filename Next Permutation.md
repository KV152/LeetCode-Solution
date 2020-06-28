# [1]Next Permutation (Medium)

## Contents
- [Solution Explanation](#solution-explanation)
  - [Problem description](#problem-description)
  - [Problem Analysis](#problem-analysis)
  - [(1)Two Pointer](#1-two-pointer) 
    - [Problem and Apporach Analysis](#problem-and-apporach-analysis) 
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
1,2,3,4 → 1,3,2,4 → 1,3,4,2 → 1,4,2,3 → 1,4,3,2
2,1,3,4 → 2,3,1,4 → 2,3,4,1 → 2,4,1,3 → 2,4,3,1
3,1,2,4 → 3,2,1,4 → 3,2,4,1 → 3,4,1,2 → 3,4,2,1
4,1,2,3 → 4,2,1,3 → 4,2,3,1 → 4,3,1,2 → 4,3,2,1
```
## C++ knowledge

