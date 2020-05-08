# [7] Reverse Integer (Easy)
## Contents
  - [Solution Explanation](#solution-explanation)
    - [Problem Describtion](#problem-describtion)
    - [(1) Build-in Function](#1-build-in-function)
    - [(2) Modulus and Addation](#2-modulus-and-addation)
   - [C++ Knowledge](#c-knowledge)

## Solution Explanation

### Problem Describtion
Tag: [Math]
Given a 32-bit signed integer, reverse digits of an integer.

Example 1:
```
Input: 123
Output: 321
```
Example 2:
```
Input: -123
Output: -321
```
Example 3:
```
Input: 120
Output: 21
```
Note:\
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2<sup>31</sup>,  2<sup>31</sup> − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

### (1) Build-in Function 

`to_string` function to get the string of absolute value of `int`.\
`reverse` function to get the reverse of `string`.\
`stoi` to tansfer the string back to int, and it will throw `execption` about overflow. 
    
``` C++
  class Solution {
  public:
    int reverse(int x) {
      std::string xString;
      bool isNegative = 0;
      // Get the string represent absolute value of x
      if (x < 0){
          isNegative = 1;
          xString = std::to_string((long)-1 * x); //overflow negative int to positive int
      }
      else
        xString = std::to_string(x);
      // Get the reverse string of abs(x).
      std::reverse(xString.begin(), xString.end());
      // Insert negative sign when x is a negative
      if (isNegative){
        xString.insert(0, "-");
      }     
      // Get the reverse int or an overflow 
      try{
        return std::stoi(xString);
      }
      catch(const std::out_of_range & e){
        return 0;
      }
    }
};
```
Performance: runtime (0 ms) beats 100 % of cpp submissions, memory usage beats 100 % of cpp submissions (6.4 MB) 


### (2) Modulus and Addation
Extracting each digit by modulus function, and additing it back with correct multiply coefficient.  
        
``` C++
class Solution {
    public:
      int reverse(int x) {
          int limit;
          int lastDigtLimit;
          int digit;
          int result = 0;

          //INT_MIN and INT_MAX defined in <climits>

          if (x < 0){
            limit = INT_MIN / 10;
            lastDigtLimit = INT_MIN % 10;
            while(x != 0){
              digit = x % 10;
              if ( result < limit ||
                (result == limit && digit < lastDigtLimit))
                  return 0;
              else
                  result = result * 10 + digit; 
              x /= 10;
            }
          }
          else{
            limit = INT_MAX / 10; 
            lastDigtLimit = INT_MAX % 10;
            while(x != 0){
              digit = x % 10;
              if ( result > limit || 
              (result == limit && digit > lastDigtLimit))
                  return 0;
              else
                  result = result * 10 + digit; 
              x /= 10;
            }
          }
          return result;   
      }
};
```
- Perormance: runtime: (0 ms), memory usage: (6.1 MB)
- Time Complexity: O(log⁡(x)). There are roughly log⁡10(x) digits in x.
- Space Complexity: O(1).

    
## C++ Knowledge
