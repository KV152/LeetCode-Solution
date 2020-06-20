# [1]3 Sum (Medium)

The solution using hashing, principle of hashing can be found in [source](https://github.com/KV152/Data-Structures-and-Algorithm/blob/master/Hashing.md "hashing")
## Contents
- [Solution Explanation](#solution-explanation)
  - [Problem description](#problem-description)
  - [(1) Brute Force](#1-brute-force) 
  - [(2) Hashing](#2-hashing)
  - [(3) Two Pointers Approach](#3-two-pointers-approach)
- [C++ knowledge](#c-knowledge)

## Solution Explanation

### Problem description
Tag: [Array]
Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:

The solution set must not contain duplicate triplets.

Example:
```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

 
###  (1) Brute Force 
  The most stright forward solution.
   ``` C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        std::vector<std::vector<int>> triplets = {};
        if (nums.size()<3){
            return triplets;
        }
       // Sorting elemets for skipping the duiplicate elements to avoid duiplcate result
        std::vector<int> sortedNums(nums.begin(), nums.end());
        std::sort(sortedNums.begin(), sortedNums.end()); //sort defined in <algorithm> 

        // iteratre the nums by iterator
        std::vector<int>::iterator itX, itY, itZ;
        int target;

        for (itX = sortedNums.begin(); itX != sortedNums.end()-2; itX++){
            if (itX != sortedNums.begin() && *itX == *(itX-1))
                continue;
            for (itY = itX+1; itY != sortedNums.end()-1; itY++){
                if (itY != itX+1 && *itY == *(itY-1))
                    continue;
                target = 0 - *itX - *itY;
                for (itZ = itY+1; itZ != sortedNums.end(); itZ++){
                    if (itZ != itY+1 && *itZ == *(itZ-1))
                        continue;
                    if (*itZ == target){
                        std::vector<int> triplet = {*itX, *itY, *itZ};
                        triplets.push_back(triplet); 
                    }                     
                }
            }
        }
        return triplets;
    }
};
   ```



- Time complexity : O(n^3)\
  For each element, we try to find its complement by looping through the rest of array which takes O(n) time. Therefore, the time complexity is O(n^3)
- Space complexity : O(1) 
- Performance: Time LIMIT EXCEEDED.

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
    
    

### (3) Two Pointers Approach
  By fixing X, we can use two pointer approach to find Y and Z without introducing extra space.

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
        std::vector<int>::iterator itX, itY, itZ;

        int target;
        int upperZ; // For skipping Z
        for (itX = nums.begin(); itX != nums.end()-2; itX++){
            if (itX != nums.begin() && *itX == *(itX-1))
                continue;
            //Skip remaining elements. ([X+Y+Z = 0, X<=Y<=Z] => X<=0)
            if (*itX > 0)
                break;
            target = 0 - *itX;
            itZ = nums.end()-1;
            itY = itX + 1;
            while(itY != itZ){
                //Skip impossible remaining elements 
                if (2*(*itY)>target || 2*(*itZ)<target)
                    break;

                if (*itY+*itZ == target){
                    triplets.push_back({*itX, *itY, *itZ});     
                    itY++;
                    // avoid duiplicate
                    while(itY != itZ && *itY == *(itY-1)){
                        itY++;
                    } 
                }
                else if (*itY+*itZ > target){
                    itZ--;
                }
                else{
                    itY++;
                }
            }
        }
        return triplets;
    }
};
```


- Time complexity : O(n^2)\
  We traverse the list containing n elements only once. Each look up in the table costs only O(1) time.
- Space complexity : O(1)\
  The extra space required depends on the number of items stored in the hash table, which stores at most n elements.
- Performance: runtime 46.73% 128 ms, memory usage 66% 19.8 MB.

## C++ knowledge
   By using properties of set (no duiplcates in set container), there is a fast method to remove duiplcates in a vector.
   
    ``` C++
        // by using properties of set, we can remove all dulplicates in a vector
        // nums is vector<int>
        std::set<int> temp;
        for(std::vector<int>::iterator it = nums.begin(); it != nums.end(); it++) 
            temp.insert(*it);
      ```

	
