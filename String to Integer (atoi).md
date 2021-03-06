# [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)

## Description

Implement atoi which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

__Note:__

- Only the space character ' ' is considered as whitespace character.
- Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31,  2^31 − 1]. If the numerical value is out of the range of representable values, INT_MAX (2^31 − 1) or INT_MIN (−2^31) is returned.

Example 1:

    Input: "42"
    Output: 42
Example 2:

    Input: "   -42"
    Output: -42
    Explanation: The first non-whitespace character is '-', which is the minus sign. Then take as many numerical digits as possible, which gets 42.
Example 3:

    Input: "4193 with words"
    Output: 4193
    Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
Example 4:

    Input: "words and 987"
    Output: 0
    Explanation: The first non-whitespace character is 'w', which is not a numerical digit or a +/- sign. Therefore no valid conversion could be performed.
Example 5:

    Input: "-91283472332"
    Output: -2147483648
    Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer. Thefore INT_MIN (−2^31) is returned.

## Solution

### #1

```c++
    class Solution {
    public:
        int myAtoi(string str) {
            int pos = 0;
            while(1) {
                if(str[pos] == ' ')
                    ++ pos;
                else if(str[pos] == '+' || str[pos] == '-' || (str[pos] >= '0' && str[pos] <= '9')) {
                    long long ans = read(pos, str);
                    if(ans > INT_MAX) 
                        return INT_MAX;
                    else if(ans < INT_MIN)
                        return INT_MIN;
                    else
                        return ans;
                }
                else 
                    return 0;
            }
            return 0;
        }
        
        long long read(int pos, string& str) {
            long long ans = 0;
            int f = 1;
            if(str[pos] == '-') {
                f = -1;
                ++ pos;
                if(str[pos] == '+')
                    return 0;
            }
            if(str[pos] == '+') {
                ++ pos;
                if(str[pos] == '-')
                    return 0;
            }
            
            while(str[pos] >= '0' && str[pos] <= '9') {
                ans = ans * 10 + str[pos] - '0';
                if(f * ans < INT_MIN)
                    return INT_MIN;
                if(f * ans > INT_MAX)
                    return INT_MAX;
                ++ pos;
            }
            return f * ans;
        }
    };
```
Key Points

1. Be aware that current char will be processed or not.
2. There will be wrong `char` arithmetically, such as '+-' and '-+'.
3. The result is returned in type `int`, so consider using type `long long int` to store the intermediate results. But they may greater than `INT_MAX` or less than `INT_MIN`, thus comparing them with `INT_MAX` and `INT_MIN` to determine return the final result or the boundary value.
4. About read() function, `ans = ans * 10 + str[pos] - '0'` is the core of the problem.

### #2

``` c++
int myAtoi(string str) {
    long result = 0;
    int indicator = 1;
    for(int i = 0; i < str.length();)
    {
        i = str.find_first_not_of(' '); // familiar with STL
        if(str[i] == '-' || str[i] == '+')
            indicator = (str[i++] == '-')? -1 : 1;
        while('0'<= str[i] && str[i] <= '9')
        {
            result = result*10 + (str[i++]-'0');
            if(result*indicator >= INT_MAX) return INT_MAX;
            if(result*indicator <= INT_MIN) return INT_MIN;
        }
        return result*indicator;
    }
}
```