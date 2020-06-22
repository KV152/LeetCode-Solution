# [1]4 Sum (Medium)
General solution for K-sum problem, 
## Contents
- [Solution Explanation](#solution-explanation)
  - [Problem description](#problem-description)
  - [(1) Two Pointers Approach](#1-two-pointers-approach)
  - [(2) Hashing](#2-hashing)
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


- Time complexity : O(n^3)\
- Space complexity : O(1)\
- Performance: runtime 37.57 % 104 ms, memory usage 81.75 % 12.9 MB.


### (2) Hashing
By fixing X, the probloem is reduced to two sum. We can use two sum hashing solution.
  
``` C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        std::vector<std::vector<int>> triplets = {};
        if (nums.size()<3){
            return triplets;
        }
        
       // Sorting elemets for skipping the duiplicate elements to avoid duiplcate result
        std::sort(nums.begin(), nums.end()); //sort defined in <algorithm> 

        // iteratre the nums by iterator
        std::vector<int>::iterator itX, itY;
	std::unordered_map<int, int> targetZ;
        std::unordered_map<int, int>::iterator itZ;
        int target;
        int upperZ; // For skipping Z
        for (itX = nums.begin(); itX != nums.end()-2; itX++){
            if (itX != nums.begin() && *itX == *(itX-1))
                continue;
            //Skip impossible elements. ([X+Y+Z = 0, X<=Y<=Z] => X<=0)
            if (*itX > 0)
                break;
            target = 0 - *itX;
	    upperZ = target - *(itX+1);
            targetZ.clear();
            for (itY = itX+1; itY != nums.end(); itY++){
	        // Skip impossible elements. ([Y+Z = 0-X, X<=Y<=Y+1<=Z] => Y<=-X-(Y+1))
	    	if (*itY > upperZ)
		    break;
                // To find Z
                if ((itZ = targetZ.find(*itY)) != targetZ.end()){ 
                    triplets.push_back({*itX, itZ->second, *itY});  
                    // avoid duplicate
                    while ((itY+1) != nums.end() && *(itY+1) == *itY){
                        itY++;
                        continue;  
                    }
                                        
                }
                targetZ.insert({target - *itY, *itY});
            }
        }
        return triplets;
    }
};
```

- Time complexity : O(n^2)\
- Space complexity : O(n)\
  The extra space required depends on the number of items stored in the hash table, which stores n elements. 
- Performance: runtime 16.69%  1280 ms, memory usage 7.4 % 149.5 MB.
    
## C++ knowledge
   By using properties of set (no duiplcates in set container), there is a fast method to remove duiplcates in a vector.
   
    ``` C++
        // by using properties of set, we can remove all dulplicates in a vector
        // nums is vector<int>
        std::set<int> temp;
        for(std::vector<int>::iterator it = nums.begin(); it != nums.end(); it++) 
            temp.insert(*it);
      ```

	
