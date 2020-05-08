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
The built in function used in first solution including: `to_string`, `stoi`, `string::insert`, `reverse`. The limit of numeric value type are deinfed in library `<climits>`.
  - `to_string`\
    Return a string with the representation of value. The data type including: `int`, `long`, `long long`, `float`, `double`, `long double`, `unsigned`, `unsigned long`, `unsigned long long`.
  - `stoi`\
    Parses str interpreting its content as an integral number of the specified base, which is returned as an int value.
    - `int stoi (const string&  str, size_t* idx = 0, int base = 10);`
      - str: String object with the representation of an integral number.
      - Pointer to an object of type size_t, whose value is set by the function to position of the next character in str after the numerical value. This parameter can also be a null pointer, in which case it is not used. (optional).
      - Numerical base (radix) that determines the valid characters and their interpretation. If this is 0, the base used is determined by the format in the sequence (see strtol for details). Notice that by default this argument is 10, not 0. (optional)
    - If no conversion could be performed, an `invalid_argument` exception is thrown. If the value read is out of the range of representable values by an int, an `out_of_range` exception is thrown. An invalid idx causes undefined behavior.
    - Example:
``` C++
      std::string str_dec = "2001, A Space Odyssey";
      std::string str_hex = "40c3";
      std::string str_bin = "-10010110001";
      std::string str_auto = "0x7f";

      std::string::size_type sz;   // alias of size_t

      int i_dec = std::stoi (str_dec,&sz);
      int i_hex = std::stoi (str_hex,nullptr,16);
      int i_bin = std::stoi (str_bin,nullptr,2);
      int i_auto = std::stoi (str_auto,nullptr,0);

      // can simply int i = std::stoi(str); 

      // Exception:
      #include <iostream>
      #include <string>

      int main() {

        std::string s = "10";

        try{
          int i = std::stoi(s);
          std::cout << i << '\n';
        }
        catch (std::invalid_argument const &e){
          std::cout << "Bad input: std::invalid_argument thrown" << '\n';
        }
        catch (std::out_of_range const &e){
          std::cout << "Integer overflow: std::out_of_range thrown" << '\n';
        }

        return 0;
       }
```
  - `string::insert`\
  Inserts additional characters into the string right before the character indicated by pos (or p)
    - Example:
``` C++
      std::string str="to be question";
      std::string str2="the ";
      std::string str3="or not to be";
      std::string::iterator it;

      // used in the same order as described above:
      str.insert(6,str2);                 // to be (the )question
      str.insert(6,str3,3,4);             // to be (not )the question
      str.insert(10,"that is cool",8);    // to be not (that is )the question
      str.insert(10,"to be ");            // to be not (to be )that is the question
      str.insert(15,1,':');               // to be not to be(:) that is the question
      it = str.insert(str.begin()+5,','); // to be(,) not to be: that is the question
      str.insert (str.end(),3,'.');       // to be, not to be: that is the question(...)
      str.insert (it+2,str3.begin(),str3.begin()+3); // (or )
 ```
 - `std::reverse`\
    Reverses the order of the elements in the range [first,last). `std::reverse(myvector.begin(),myvector.end());`
 - `climits`\
  This header defines constants with the limits of fundamental integral types for the specific system and compiler implementation used.
  The limits for fundamental floating-point types are defined in <cfloat> (<float.h>).
The limits for width-specific integral types and other typedef types are defined in <cstdint> (<stdint.h>).
