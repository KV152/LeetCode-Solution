# [27]Remove Element (Easy)
Title is misleading. Not removing target value, just moving the elements without target value in the fornt of the array. In the other words, moving target value out of the front of array.
## Contents
- [Solution Explanation](#solution-explanation)
  - [Problem description](#problem-description)
  - [(1) Two Pointers Approach](#1-two-pointers-approach)
  - [(2) Two Pointers Simplified](#2-two-pointers-simplified)
  - [(3) Two Pointers Same Side](#3-two-pointers-same-side)
- [C++ knowledge](#c-knowledge)

## Solution Explanation

### Problem description
Tag: [Array]</br>
Given an array ```num```s and a value ```val```, remove all instances of that value in-place and return the new length.
Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.
The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Example 1:
```
Given nums = [3,2,2,3], val = 3,
Your function should return length = 2, with the first two elements of nums being 2.
It doesn't matter what you leave beyond the returned length.
```

Example 2:
```
Given nums = [0,1,2,2,3,0,4,2], val = 2,
Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.
Note that the order of those five elements can be arbitrary.
It doesn't matter what values are set beyond the returned length.
```
Clarification:
Confused why the returned value is an integer but your answer is an array?
Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.
Internally you can think of this:
```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeElement(nums, val);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```
 

### (1) Two Pointers Approach
 Aim: moving elements with target value out of front of the vector. 
 - One pointer(x) srat from the begin of vector and the anoter pointer(y) from the last element of vector. 
 - Moving x inward.
 -  if x pointed to target value, exchange the value of x and y then move both x and y inward
 - stopped when x>y
``` C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        std::vector<int>::size_type x, y;
        if (nums.size()<1)
            return 0;

        x = 0; 
        y = nums.size()-1;
        while (x < y ){
            if (nums[x] == val){
                if (nums[y] != val)
                    nums[x++] = nums[y--];
                else
                {
                    y--;
                    continue;
                }
            }
            else
                x++;
        }
        if (x == y && nums[x] != val){
            // two pointers point to the same element, iteration miss this element
            return x+1;
        }
        return x;
        
    }
};
```
- Time complexity : O(n)\
  We traverse the list containing n elements only once. 
- Space complexity : O(1)\
- Performance: runtime  62.72 % 4 ms, memory usage 83.68 %  8.9 MB.
### (2) Two Pointers Simplified
In this version, the process is simplifid. Copy Y to X without checking value of Y.
```C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        std::vector<int>::size_type x, y;
        x = 0; 
        y = nums.size(); // check all elements
        while (x < y ){
            if (nums[x] == val){
                nums[x] = nums[y-1];
                y--;
            }
            else
                x++;
        }
        return x;
    }
};
```
- Time complexity : O(n)\
  We traverse the list containing n elements only once. 
- Space complexity : O(1)\
- Performance: runtime 100 % 0 ms, memory usage 48.67 %  9 MB.

### (3) Two Pointers Same Side
It's variant of two pointers that two pointer start from the same side (left side).
## C++ knowledge
