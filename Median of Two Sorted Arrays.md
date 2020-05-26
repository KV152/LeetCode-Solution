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
In order to solve the complex problem with complexity requirement, it's better to implement analyze with the aid of mathmatic modelling and flowchart of the whole process.\
This solution is based on leetcode solution. Complexity required (log(m+n)) which is the same to binary search, and the median is the middle of the array. Therefore, we could build an approach based on idea of binary search. Before building the binary search, we have to build a model to describe two array.
#### Model
Assuming there are two array A and B, and sizes of two array are m and n respectively.
Let i be the index for A and j for the index for B, assuming the following index i and j are valid. When i and j pointed to the median of the combined sorted array.
#### Constrians
If i and j pointed to the median of combined sorted array, then they must satisfied following constrians:
```
1. i+j = (m-i) + (n-j)
    (if n ≥ m,  we just need to set: i=0∼m, j = (m+n+1)/2 - i)
2. B[j-1] ≤ A[i] and A[i-1] ≤ B[j]
```
With the first constrians and the goal that i and j point to the median of the combined sorted array, j can be expressed by i with aid of size of both array.  

#### Searching Steps
Assuming the i and j in the both array, if we find the median, i and j satisfy follwoing two stop conditions:
```
Scanning i in [0, m] to find an object i such that: 
    1. B[j-1] ≤ A[i], 
    2. A[i-1] ≤ B[j], 
    (where j = (m+n+1) / 2 - i)
```
The binary search steps for median can be expressed as:
1. Set i_min = 0, i_max = m, then start searching in [i<sub>min</sub>, i<sub>max</sub>]
2. Set i = (i_min + i_max)/2, j = (m+n+1)/2 - i
3. Now we have len(left_part)=len(right_part). And there are only 3 situations that we may encounter:
    - B[j−1]≤A[i] and A[i−1]≤B[j]\
        (The i and j have found)
    - B[j−1]>A[i]\
       Increasing i and decrasing j. Searching in the range: [i+1, i<sub>max</sub>]
    - A[i−1]>B[j]\
        Decaseing i and increasing j. Searching in the range: [i<sub>min</sub>, i-1]

When i is found, the median is max(A[i−1],B[j−1]) if (m+n) is odd. Otherwise median = (A[i−1]+B[j−1])/2
#### Edge Conditions
When we facing i=0,i=m,j=0,j=n, then A[i−1], B[j−1], A[i] or B[j] may not valid. Then, we can find the median in one array directly. Therefore, the previos stop conditions can be expressed as
```
Scanning i in [0, m] to find an object i such that: 
    1. j=0 or i=m or B[j-1] ≤ A[i], 
    2. i=0 or j=n or A[i-1] ≤ B[j], 
    (where j = (m+n+1) / 2 - i)
```
In searching loop, we will encounter only three situations:
```
1. Meet Stop conditions:
    * j=0 or i=m or B[j-1] ≤ A[i], 
    * i=0 or j=n or A[i-1] ≤ B[j], 
    (where j = (m+n+1) / 2 - i)
2. i<m and B[j−1]>A[i]:
    i is small, increasing i.
3. i>0 and A[i−1]>B[j]:
     i is big, decreasing it.
```


## C++ Knowledge
