# [1]4 Sum (Medium)
Providing general solution for K-sum problem.
## Contents
- [Solution Explanation](#solution-explanation)
  - [Problem description](#problem-description)
  - [(1) Two Pointers Approach](#1-two-pointers-approach)
  - [(2) Hashing](#2-hashing)
- [General Solution for K-sum problem](#general-solution-for-k-sum-problem)
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
```C++
class Solution {
public:
    vector<vector<int>> KSum(int K, int target, 
    std::vector<int>::iterator start, std::vector<int>::iterator end,
    vector<int>& nums){
        // sorting the array at first place
        int currentNum;
        vector<vector<int>> result, temp;
        std::vector<int>::iterator it;
        vector<vector<int>>::iterator itResult;
        if (start == nums.begin()){
            std::sort(nums.begin(), nums.end());
        }
        // distance defined in <iterator>
        if (K>std::distance(start, end) || K*(*start)> target || K*(*(end-1))<target){
            return result;
        }

        if (K==2){
            return twoSum(target, start, end);
        }
        else{
            for (it = start; it != (end-K+2); it++){
                //avoid duplicates
                if (it != start && *it == *(it-1)){
                    continue;
                }
                temp = KSum(K-1, target - *it, it+1, end, nums);

                for (unsigned i = 0; i<temp.size(); i++){
                    result.push_back({*it});
                    result.back().insert(result.back().end(), temp[i].begin(), temp[i].end());
                }
                if (temp.size()>0){
                }
            }
            return result;
        }
    }

    vector<vector<int>> twoSum(int target, 
    std::vector<int>::iterator start, std::vector<int>::iterator end){
        // X, Y
        std::vector<int>::iterator itX, itY;
        itX = start;
        itY = end-1;
        int sum;
        std::vector<vector<int>> result;
        
        while(itX != itY){
            if ((sum=*itX+*itY) == target){
                result.push_back({*itX, *itY});
                itX++;
                while(itX != itY && *itX == *(itX-1)){
                    itX++;
                }
            }
            else if (sum > target){
                itY--;
            }
            else{
                itX++;
            }
        }
        
        return result;
    }

    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        return KSum(4, target, nums.begin(), nums.end(), nums);
    }
};
```
- Time complexity : O(n^3)
- Space complexity : O(1)
- Performance: runtime 88.5 % 20 ms, memory usage 29.01 %  13.5 MB.

### (2) Hashing General
There are two functions called twoSum and KSum. The twoSum function is used for sloving two sum problem and the KSum is an helper function. This helper function is used to reduced the K-sum problem to two sum and formate the result for return.

```C++
class Solution {
public:
    vector<vector<int>> KSum(int K, int target, 
    std::vector<int>::iterator start, std::vector<int>::iterator end,
    vector<int>& nums){
        // sorting the array at first place
        int currentNum;
        vector<vector<int>> result, temp;
        std::vector<int>::iterator it;
        vector<vector<int>>::iterator itResult;
        if (start == nums.begin()){
            std::sort(nums.begin(), nums.end());
        }
        // distance defined in <iterator>
        if (K>std::distance(start, end) || K*(*start)> target || K*(*(end-1))<target){
            return result;
        }

        if (K==2){
            return twoSum(target, start, end);
        }
        else{
            for (it = start; it != (end-K+2); it++){
                //avoid duplicates
                if (it != start && *it == *(it-1)){
                    continue;
                }
                temp = KSum(K-1, target - *it, it+1, end, nums);

                for (unsigned i = 0; i<temp.size(); i++){
                    result.push_back({*it});
                    result.back().insert(result.back().end(), temp[i].begin(), temp[i].end());
                }
                if (temp.size()>0){
                }
            }
            return result;
        }
    }

    vector<vector<int>> twoSum(int target, 
    std::vector<int>::iterator start, std::vector<int>::iterator end){
        // X, Y
        std::unordered_set<int> Yset;
        std::vector<int>::iterator it;
        vector<vector<int>> result;
        std::unordered_set<int>::iterator Y;
        for (it = start; it != end; it++){
            if ((Y = Yset.find(*it)) != Yset.end()){
                result.push_back({target-*it, *it});
                //avoid duplicates
                while(it != (end-1) && *it == *(it+1))
                    it++;
            }
            else{
                Yset.insert(target-*it);
            }
        }
        return result;
    }

    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        return KSum(4, target, nums.begin(), nums.end(), nums);
    }
};
```
- Time complexity : O(n^3)
- Space complexity : O(n)
- Performance: running time 33.2 % 120 ms, memory usage 15.02 % 28.6 MB.

## C++ knowledge
- ```distance(A, B)``` defined in ```<iterator>``` library which can be used to measure the distance between two iterator A and B.
	- Performance: Constant for random-access iterators. Otherwise, linear in n.
- ```unordered_set``` defined in ```<unordered_set>``` library. It's similar to data tpye```unordered_map``` without associated values. 
	- ```find(key)``` An iterator to the element, if the specified value is found, or unordered_set::end if it is not found in the container.
		- Performance: Average case: constant. Worst case: linear in container size.
	- ```count(key)``` 1 if an element with a value equivalent to k is found, or zero otherwise.
		- Performance: Average case: constant. Worst case: linear in container size.
- ```vector```. The last element in the vector can be found by calling method ```back()``` or by the iterator ```end()-1```. There is an ending element in the vector, which  can be accessed by the method ```end()``` which returns a iterator point to that element.

