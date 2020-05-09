# [4] Median of Two Sorted Arrays (Hard)
## Contents

## Solution Explanation
### Problem Description 
There are two sorted arrays nums1 and nums2 of size m and n respectively. Find the median of the two sorted arrays. 
The overall run time complexity should be O(log (m+n)). 
You may assume nums1 and nums2 cannot be both empty.\

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


### (1) Iterating to find
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

## C++ Knowledge
