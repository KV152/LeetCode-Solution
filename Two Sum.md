# [1]Two Sum (Easy)

##Contents

- Solution description



## Solution Explanation

###Problem description

Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

'''
Given nums = [2, 7, 11, 15], target = 9,
Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
'''

 
###  (1) Brute Force 
  The most stright forward solution is to iterate the array and find the pair 
   by testing value in array one by one. 
   '''
   class Solution {
    public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int>::size_type sz = nums.size();
        for (int m = 0; m < sz - 1; m++){
            for (int n = m + 1; n < sz; n++){
                if (nums[m] + nums[n] == target) {
                    vector<int> result = {m, n};
                    return result;
                }
            }
        }
        vector<int> result = {};
        return result;        
        }
    };
   '''

1 arithmetical operation = 1, 1 access = 1, let size of nums = n
runtime (356 ms), memory usage 7 MB

Time complexity : O(n^2). 
For each element, we try to find its complement 
by looping through the rest of array which takes O(n) time. 
Therefore, the time complexity is O(n^2).
Space complexity : O(1). 

### (2) Tow-Pass Hash Table
First create a hash table based on the corresponding difference between 
   the sum and individual integer provided in the array. 
  Then search each integer in the hash table to find the complement.

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int>::size_type sz = nums.size();
        unordered_map<int, int> hashTable;
        //Create hash table
        for (int i = 0; i < sz; i++){
            hashTable.insert({target-nums[i], i});
        }
        unordered_map<int, int>::const_iterator it;
        int index;
        for (int i = 0; i < sz; i++){
            if ((it = hashTable.find(nums[i])) != hashTable.end() 
                && (index = it->second) != i){
                    vector<int> result = {i, index};
                    return result;
                }
                
        }
        vector<int> result = {};
        return result;        
    }
};



Time complexity : O(n). 
We traverse the list containing n elements exactly twice.
 Since the hash table reduces the look up time to O(1), the time complexity is O(n).

Space complexity : O(n). 
The extra space required depends on the number of items stored in the hash table, 
which stores exactly n elements. 

    Your runtime (16 ms)
    Your memory usage (10.6 MB)

### (3) One-pass Hash Table
   Assuming the number Y is the complement of number X to make up the sum, 
     and array index of Y is behind of X. Moverover, the order of inserting 
     array elements into hash table is according to the array index.
     Then, we can only find the pair of number after the Y is inserted into the hash table. 
     In other words, we can search the complement and insert the difference simultaneously,
     and we don't miss the any pair.

     In this situation, we can insert difference and search complement at the same time.
     We only need to iterate the array one time.

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int>::size_type sz = nums.size();
        unordered_map<int, int> hashTable;

        unordered_map<int, int>::const_iterator it;
        for (int i = 0; i < sz; i++){
            if ((it = hashTable.find(nums[i])) != hashTable.end()){
                    // The return order
                    vector<int> result = {it->second, i}; 
                    return result;
                }
            else 
                hashTable.insert({target-nums[i], i});
                
        }
        vector<int> result = {};
        return result;        
    }
};



Time complexity : O(n). 
We traverse the list containing n elements only once. 
Each look up in the table costs only O(1) time.

Space complexity : O(n). The extra space required depends on 
the number of items stored in the hash table, 
which stores at most n elements.

runtime 12 ms
memory usage (10.1 MB)

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int>::size_type sz = nums.size();
        unordered_map<int, int> hashTable;

        unordered_map<int, int>::const_iterator it;
        for (int i = 0; i < sz; i++){
            if ((it = hashTable.find(nums[i])) != hashTable.end()){
                    // The return order
                    vector<int> result = {it->second, i}; 
                    return result;
                }
            else 
                hashTable.insert({target-nums[i], i});
                
        }
        vector<int> result = {};
        return result;        
    }
};

## C++ knowledge