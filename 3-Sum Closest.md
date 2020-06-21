# [1]3 Sum Closest(Medium)

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

	
