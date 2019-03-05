# [Integer Replacement](https://leetcode.com/problems/integer-replacement/)

## Description

Given a positive integer n and you can do operations as follow:

If n is even, replace n with n/2.
If n is odd, you can replace n with either n + 1 or n - 1.
What is the minimum number of replacements needed for n to become 1?

Example 1:

    Input:
    8

    Output:
    3

    Explanation:
    8 -> 4 -> 2 -> 1
Example 2:

    Input:
    7

    Output:
    4

    Explanation:
    7 -> 8 -> 4 -> 2 -> 1
    or
    7 -> 6 -> 3 -> 2 -> 1

## Solution

This question may seem to be simple enough. But when the input n is INT_MAX, you can do nothing about the overflow.

Just like the code below.

```cpp
class Solution {
public:
    int integerReplacement(int n) {
        if(n == 1) return 0;
        else if(n & 0x1) {
            return min(integerReplacement(n - 1), integerReplacement(n + 1)) + 1;
        } else {
            return integerReplacement(n / 2) + 1;
        }
    }
};
```
It is stupid, you can deal with it by `(n + 1) / 2`.

Then you can use another function, which has bigger range (long long) in storing a number to overcome the problem. The final result is obviously within int range, so we do not care about the return value.

```cpp
class Solution {
public:
    int integerReplacement(int n) {
        return integerReplacement2(n);
    }
    int integerReplacement2(long long n) {
        if(n == 1) return 0;
        else if(n & 0x1) {
            return min(integerReplacement2(n - 1), integerReplacement2(n + 1)) + 1;
        } else {
            return integerReplacement2(n / 2) + 1;
        }
    }
};
```

Then try with DP.
First from the vary beginning, if n == 1, return 0. If n is even, needless to say, the answer f(n) = 1 + f(n/2). Otherwise f(n) = 2 + min(f(n/2), f((n + 1)/2). ===> If it is odd, frist add or minus 1, then divide it by 2. So consider replace the procedure with directly find min between f(n/2) and f((n + 1)/2).
Another optimize is memorize. we store all results we have done to avoid redundant computing.

```cpp
class Solution {
private:
    unordered_map<int, int> visited;

public:
    int integerReplacement(int n) {
        if (n == 1) { return 0; }
        if (visited.count(n) == 0) {
            if (n & 1 == 1) {
                visited[n] = 2 + min(integerReplacement(n / 2), integerReplacement(n / 2 + 1));
            } else {
                visited[n] = 1 + integerReplacement(n / 2);
            }
        }
        return visited[n];
    }
};
```