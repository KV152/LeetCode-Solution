# [1]Container With Most Water (Medium)

## Contents
- [Solution Explanation](#solution-explanation)
  - [Problem description](#problem-description)
  - [(1)Two Pointer](#1-two-pointer) 
    - [Problem and Apporach Analysis](#problem-and-apporach-analysis) 
    - [Programming](#programming) 
- [C++ knowledge](#c-knowledge)


## Solution Explanation

### Problem description
Tag: [Array]\
Given n non-negative integers a <sub>1 </sub>, a <sub>2 </sub>, ..., a <sub>n </sub> , where each represents a point at coordinate (i, a <sub>i </sub>). n vertical lines are drawn such that the two endpoints of line i is at (i, a <sub>i </sub>) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.
Example:

```
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```

 
###  (1) Two Pointer 
 #### Problem and Apporach Analysis
 #### Programming
 1. One pointer starts from the rightmost and another starts from the leftmost.
 2. Calculated the area based on the width and height from the two pointer and compated to the stored maximum value
 3. Moving the pointer with lowest height towatds central. 
 4. Stopping the process once two pointers meet each other.
   ``` C++
  class Solution {
  public:
      int maxArea(vector<int>& height) {
          int maxWater = 0;
          int area = 0;
          int left, right;
          left = 0;
          right = height.size()-1;
          while(left != right){
              area = std::min(height[left], height[right]) * (right -left);
              if (area > maxWater)
                  maxWater = area;
              if (height[left] > height[right])
                  right--;
              else
                  left++;
          }
          return maxWater;
      }
  };
   ```
- Time complexity : O(n)\
  Single Pass.
- Space complexity : O(1) 
- Performance: runtime (24 ms) 85.29 % of cpp submissions, memory usage (14.2 MB) 65.85 % of cpp submissions.


## C++ knowledge
The ```min``` and ```max``` are defined in the ```<algorithm>```library. Theses functions return the minimum or maximum value based on two input values. If multiple input values are required by the task, we can use the following small tricks: ```min(n1, min(n2, min(n3, n4)));```
	
