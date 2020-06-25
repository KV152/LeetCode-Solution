# [26]Remove Duplicates from Sorted Array (Easy)
Title is misleading. Not removing duplicates, just moving the unqiue element in the fornt of the array.
## Contents
- [Solution Explanation](#solution-explanation)
  - [Problem description](#problem-description)
  - [(1) Two Pointers Approach](#1-tow-pointers-approach)
- [C++ knowledge](#c-knowledge)

## Solution Explanation

### Problem description
Tag: [Array]</br>
Given a sorted array ```nums```, remove the duplicates in-place such that each element appear only once and return the new length.
Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

Example 1:
```
Given nums = [1,1,2],
Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
It doesn't matter what you leave beyond the returned length.
```

Example 2:
```
Given nums = [0,0,1,1,1,2,2,3,3,4],
Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.
It doesn't matter what values are set beyond the returned length.
```
Clarification:
Confused why the returned value is an integer but your answer is an array?
Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.
Internally you can think of this:
```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```
 

### (1) Two Pointers Approach
One pointer (x) pointed to the index where last unique integer, and the another pointer (y) is moving to the end for finding the next unique value.
When the next unique value has found, exchanging the two values pointed by two pointer (x+1 and y)
``` C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        // x, y
        std::vector<int>::size_type x, y;
        if (nums.size()<2)
            return nums.size();
        for (x=0, y=1; y<nums.size(); y++){
            if (nums[x] != nums[y])
                nums[++x] = nums[y];
        }
        return x+1;
    }
};
```
- Time complexity : O(n)\
  We traverse the list containing n elements only once. 
- Space complexity : O(1)\
- Performance: runtime 99.15 % 12 ms, memory usage 79.37 %  13.7 MB.

## C++ knowledge
```std::vector<int>::size_type``` size of vector or other data type is defined by ```size_type```, unsigned may be acceptable and int is only used when size of vector is alresad known.
