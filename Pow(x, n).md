# [Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Description

Implement pow(x, n), which calculates x raised to the power n (x^n).

Example 1:

    Input: 2.00000, 10
    Output: 1024.00000
Example 2:

    Input: 2.10000, 3
    Output: 9.26100
Example 3:

    Input: 2.00000, -2
    Output: 0.25000
    Explanation: 2-2 = 1/22 = 1/4 = 0.25
**Note:**

-100.0 < x < 100.0
n is a 32-bit signed integer, within the range [−2^31, 2^31 − 1]

## Solution

```cpp
class Solution {
public:
    double myPow(double x, int n) {
        double ans = 1;
        long long n_ = n;
        if(n_ < 0) {
            x = 1 / x;
            n_ = -n_;
        }
        while(n_) {
            if(n_ & 1) ans *= x;
            x *= x;
            n_ >>= 1;
        }
        return ans;
    }
};
```

The main problem is overflow ===> INT_MIN * -1 = INT_MIN. So consider using `long long int` to store `n`.
<br>
Another highlight is using `if(n_ & 1)` to replace `if(n_ % 2)`, which significantly reduce the time cost.