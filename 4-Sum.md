# [1]4 Sum (Medium)
Providing general solution for K-sum problem.
## Contents
- [Solution Explanation](#solution-explanation)
  - [Problem description](#problem-description)
  - [(1) Two Pointers Approach](#1-two-pointers-approach)
  - [(2) Hashing](#2-hashing)
- [General Solution for K-sum problem](#general-solution-for-ksum-problem)
  - [(1) Two Pointers Approach](#1-two-pointers-approach-general)
  - [(2) Hashing](#2-hashing-general)
- [C++ knowledge](#c-knowledge)

## Solution Explanation

### Problem description
Tag: [Array]

Given an array ```nums``` of n integers and an integer ```target```, are there elements a, b, c, and d in ```nums``` such that a + b + c + d = ```target```? Find all unique quadruplets in the array which gives the sum of ```target```.

Note:

The solution set must not contain duplicate quadruplets.

Example:
```
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```
### (1) Two Pointers Approach

``` C++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> quadruplets;
        // I, J, K, M 
        //sorting
        std::sort(nums.begin(),nums.end());

        if (nums.size()<3)
            return quadruplets;

        std::vector<int>::iterator itI, itJ, itK, itM;
        int twoSum, temp;
        for (itI = nums.begin(); itI != nums.end()-3; itI++){
            // avoid duiplicates
           if (itI != nums.begin() && *itI==*(itI-1)){
                continue;
            }
            for (itJ = itI+1; itJ != nums.end()-2; itJ++){
                if (itJ != itI+1 && *itJ==*(itJ-1)){
                    continue;
                }
                twoSum = *itI + *itJ;
                itK = itJ+1;
                itM = nums.end()-1;

                while(itK != itM){
                    if ((temp = *itK + *itM + twoSum) == target){
                        quadruplets.push_back({*itI, *itJ, *itK, *itM});
                        itK++;
                        while (itK != itM && *itK==*(itK-1)){
                            itK++;
                        }
                    }
                    else if (temp > target){
                        itM--;
                    }
                    else{
                        itK++;
                    }
                }
            }
        }
        return quadruplets;
    }
};
```


- Time complexity : O(n^3)
- Space complexity : O(1)
- Performance: runtime 37.57 % 104 ms, memory usage 81.75 % 12.9 MB.


### (2) Hashing
By fixing X, the probloem is reduced to two sum. We can use two sum hashing solution.
  
``` C++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> quadruplets;
        std::unordered_map<int, int> M;
        // I, J, K, M 
        //sorting
        std::sort(nums.begin(),nums.end());

        if (nums.size()<3)
            return quadruplets;

        std::vector<int>::iterator itI, itJ, itK;
        std::unordered_map<int, int>::iterator itM;
        int twoSum, temp;
        for (itI = nums.begin(); itI != nums.end()-3; itI++){
            // avoid duiplicates
           if (itI != nums.begin() && *itI==*(itI-1)){
                continue;
            }
            for (itJ = itI+1; itJ != nums.end()-2; itJ++){
                // avoid duiplicates
                if (itJ != itI+1 && *itJ==*(itJ-1)){
                    continue;
                }
                twoSum = *itI + *itJ;
                M.clear();
                for (itK = itJ+1; itK != nums.end(); itK++){
                    if ( (itM=M.find(*itK)) != M.end()){
                        quadruplets.push_back({*itI, *itJ, itM->second, *itK});
                        //avoid duiplicates
                        while(itK != (nums.end()-1) && *itK == *(itK+1)){
                            itK++;
                        }
                    }
                    else{
                        M.insert({target-*itK-twoSum, *itK});
                    }
                }
            }
        }
        return quadruplets;
    }
};
```

- Time complexity : O(n^3)
- Space complexity : O(n)
- Performance: Time Limit Exceeded

## General Solution for K-Sum problem
### (1) Two Pointers Approach General
### (2) Hashing General
    
## C++ knowledge


	
