# [Decode Ways](https://leetcode.com/problems/decode-ways/)

## Description

A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26
Given a non-empty string containing only digits, determine the total number of ways to decode it.

Example 1:

    Input: "12"
    Output: 2
    Explanation: It could be decoded as "AB" (1 2) or "L" (12).
Example 2:

    Input: "226"
    Output: 3
    Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).

## Solution

```cpp
class Solution {
public:
    int numDecodings(string s) {
        if(s.length() == 0) return 0;
        int n = s.length();
        vector<int> dp(s.length() + 1);
        dp[n] = 1;
        for(int i = n - 1; i >= 0; --i) {
            if(s[i] == '0') dp[i] = 0;
            else {
                dp[i] = dp[i + 1];
                if(i < n - 1 && read(s[i], s[i + 1]) <= 26)
                    dp[i] += dp[i + 2];
            }
        }
        return dp[0];
    }
    
    int read(char ch1, char ch2) {
        return (ch1 - '0') * 10 + ch2 - '0';
    }
};
```

This dynamic programming thought is quite simple. For example, `12258` can be divided into `12` and `258` or `1` and `2258`. For `2258`, it can be divided into `2` and `258`. Obviously, there is a subproblem.
So we can solve this problem reversely. We can think about `8` and `58` first.
One case is `0`, which has no corresponding letter. We can only see it as a number which has to be decoded with others.

The equation is simple.

```python
for i from s.length() to 0:
    if s[i] != '0':
        dp[i] = dp[i + 1]
    else:
        dp[i] = 0

    if int(s[i:i + 2]) <= 26:
        dp[i] += dp[i + 2]
```