# [1]3 Sum Closest(Medium)

## Contents
- [Solution Explanation](#solution-explanation)
  - [Problem description](#problem-description)
  - [(1) Brute Force](#1-brute-force) 
  - [(2) Two Pointers Approach](#3-two-pointers-approach)
- [C++ knowledge](#c-knowledge)

## Solution Explanation

### Problem description
Tag: [Array]
Given an array ```nums``` of n integers and an integer ```target```, find three integers in ```nums``` such that the sum is closest to target. 
Return the sum of the three integers. You may assume that each input would have exactly one solution.


Example 1:
```
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
]
```
Constraints:
```
1. 3 <= nums.length <= 10^3
2. -10^3 <= nums[i] <= 10^3
3. -10^4 <= target <= 10^4
```
 
###  (1) Brute Force 
  The most stright forward solution.
``` C++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        std::vector<int>::iterator itX, itY, itZ;
        if (nums.size()==3)
            return (nums[0]+nums[1]+nums[2]);

        int absDif, sum, temp;
        // abs defined in <cmath>
        sum = nums[0]+nums[1]+nums[2];
        absDif = std::abs(target - sum); //Initial value
        for (itX = nums.begin(); itX<nums.end()-2; itX++){
            for (itY = itX+1; itY<nums.end()-1; itY++){
                for (itZ = itY+1; itZ<nums.end(); itZ++){
                    temp = *itX + *itY + *itZ; 
                    if (std::abs(temp-target) < absDif){
                        absDif = std::abs(temp-target);
                        sum = temp;
                    }
                }
            }
        }
        return sum;
    }
};
```



- Time complexity : O(n^3)\
  For each element, we try to find its complement by looping through the rest of array which takes O(n) time. Therefore, the time complexity is O(n^3)
- Space complexity : O(1) 
- Performance: runtime 5.03 % 1656 (ms), memory usage 18.61 % 9.9 (MB). 


### (2) Two Pointers Approach
  By fixing X, we can use two pointer approach to find Y and Z.

``` C++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        std::vector<int>::iterator itX, itY, itZ;
        if (nums.size()==3)
            return (nums[0]+nums[1]+nums[2]);
        std::sort(nums.begin(), nums.end());
        int absDif, sum, temp;
        // abs defined in <cmath>
        sum = nums[0]+nums[1]+nums[2];
        absDif = std::abs(target - sum); //Initial value
        for (itX = nums.begin(); itX<nums.end()-2; itX++){
            itY = itX+1;
            itZ = nums.end()-1;
            while(itY != itZ){
                temp = *itX+*itY+*itZ;
                if (std::abs(temp-target) < absDif){
		    // Found the target
                    if (temp == target){
                        return temp;
                    }
                    sum = temp;
                    absDif = std::abs(temp-target);
                }
                else if (temp>target){
                    itZ--;
                }
                else
                {
                    itY++;
                }
            }
        }
        return sum;
    }
};
```


- Time complexity : O(n^2)\
- Space complexity : O(1)\
- Performance: runtime 88.41 % 12 ms, memory usage 5.14 % 10.1 MB.

## C++ knowledge
   By using properties of set (no duiplcates in set container), there is a fast method to remove duiplcates in a vector.
   
    ``` C++
        // by using properties of set, we can remove all dulplicates in a vector
        // nums is vector<int>
        std::set<int> temp;
        for(std::vector<int>::iterator it = nums.begin(); it != nums.end(); it++) 
            temp.insert(*it);
      ```

	
