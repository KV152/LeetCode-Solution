# [4] Median of Two Sorted Arrays (Hard)
## Contents

## Solution Explanation
### Problem Description 
There are two sorted arrays nums1 and nums2 of size m and n respectively. Find the median of the two sorted arrays. 
The overall run time complexity should be O(log (m+n)). 
You may assume nums1 and nums2 cannot be both empty.

Example 1:
```
nums1 = [1, 3]
nums2 = [2]
```
The median is 2.0

Example 2:
```
nums1 = [1, 2]
nums2 = [3, 4]
```
The median is (2 + 3)/2 = 2.5


### (1) Iterating to find (wrong)
Go though half of total amount of elements to find median of both arrays.
``` C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        bool isOdd;
        int pre, current;
        std::vector<int>::iterator ptr1, ptr2;
        ptr1 = nums1.begin();
        ptr2 = nums2.begin();

        // only one element in nums1 and nums2
        if ( nums1.size() + nums2.size() == 1 ){
            if (ptr1 != nums1.end())
                return *ptr1;
            else
                return *ptr2;
        }

        int numberMedian = (nums1.size() + nums2.size())/2 + 1;
        if ( (nums1.size() + nums2.size())%2 == 1 ){
            isOdd = true;
        }
        else{
            isOdd = false;
        }

        //both ptrs are not pointing to end
        while (numberMedian > 0 && ptr1 != nums1.end() && ptr2 != nums2.end()){
            pre = current;
            if (*ptr1>*ptr2){
                current = *ptr2;
                ptr2++;
             }
            else{
                current = *ptr1;
                ptr1++;            
            }  
            numberMedian--;
        }
        // Not finding the median
        if (numberMedian > 0){
            std::vector<int>::iterator ptr;
            // nums1 reach the end
            if (ptr1 == nums1.end()){
                ptr = ptr2;
            }
            // nums1 reach the end
            else{
                ptr = ptr1;
            }      
            // iterate rest of array to find the median
            if (numberMedian > 1){
                current = *(ptr+numberMedian-1); 
                pre = *(ptr+numberMedian-2);
            }
            else{
                pre = current;
                current = *(ptr);
            }
            

        }
        if (isOdd){
            return current;
        }
        else
            return (double) (pre + current) / 2;
    }
};
```
- Performance: runtime beats 18.72 % of cpp submissions (44 ms), memory usage beats 5.16 % of cpp submissions (89.2 MB)

### (2) Recursive Approach (Binary Search)
This solution is based on leetcode solution.
Complexity required (log(m+n)) which is the same to binary search, and the median is the middle of the array. Therefore, we could build an approach based on idea of binary search. Before building the binary search, we have to build a model to describe two array.
Assuming there are two array A and B, and sizes of two array are m and n respectively.
Let i be the index for A and j for the index for B, assuming the following index i and j are valid. When i and j pointed to the median of the combined sorted array, i and j must satisfied following coditions:
```
1. i+j = (m-i) + (n-j)
    (if n>=m,  we just need to set: i=0∼m, j = (m+n+1)/2 - i)
2. B[j-1]<= A[i] and A[i-1]<= B[j]
```
Assuming the i and j in the both array, so we scanning follwing the below condition
```
Scanning i in [0, m] to find an object i such that: 
    B[j-1]<= A[i] and A[i-1]<= B[j], where j = (m+n+1)/2 - i
```
The binary search steps for median can be expressed as:
1. Set i_min = 0, i_max = m, then start searching in [i_min, i_max]
2. Set i = (i_min + i_max)/2, j = (m+n+1)/2 - i
3. Now we have len(left_part)=len(right_part). And there are only 3 situations that we may encounter:
    - $B[j−1]≤A[i] and A[i−1]≤B[j]\text{A}[i-1] \leq \text{B}[j]A[i−1]≤B[j]$



## C++ Knowledge
