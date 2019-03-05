# [Minimum Falling Path Sum](https://leetcode.com/problems/minimum-falling-path-sum/)]

## Description

Given a square array of integers A, we want the minimum sum of a falling path through A.

A falling path starts at any element in the first row, and chooses one element from each row.  The next row's choice must be in a column that is different from the previous row's column by at most one.

Example 1:

    Input: [[1,2,3],[4,5,6],[7,8,9]]
    Output: 12
    Explanation: 
    The possible falling paths are:

- `[1,4,7], [1,4,8], [1,5,7], [1,5,8], [1,5,9]`
- `[2,4,7], [2,4,8], [2,5,7], [2,5,8], [2,5,9], [2,6,8], [2,6,9]`
- `[3,5,7], [3,5,8], [3,5,9], [3,6,8], [3,6,9]`

The falling path with the smallest sum is `[1,4,7]`, so the answer is `12`.

## Solution

### #1 Naive Solution

Add an extra function which searches the search tree and compare the sum of its path and `curMin` to get the minimum value of all paths.

Of course, it gets TLE.

```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& A) {
        if(!A.size()) return 0;
        if(A.size() == 1) return A[0][0];
        int curMin = 0x3f3f3f3f;
        for(int i = 0; i < A.size(); ++i) {
            minFallingPathSum(A, curMin, i, A[0][i], 1);
        }
        return curMin;
    }
    
    void minFallingPathSum(vector<vector<int>>& A, int& curMin, int privCol, int curSum, int depth) {
        if(depth == A.size() - 1) {
            int stepMin = 0x3f3f3f3f;
            for(int i = privCol - 1; i <= privCol + 1; i++)
                if(i >= 0 && i < A.size())
                    stepMin = min(stepMin, A[depth][i]);
            else continue;
            curMin = min(curMin, stepMin + curSum);
        }
        else {
            for(int i = privCol - 1; i <= privCol + 1; i++) {
                if(i >= 0 && i < A.size())
                    minFallingPathSum(A, curMin, i, curSum + A[depth][i], depth + 1);
                else continue;
            }
        }
    }
};
```

### #2 DP Solution

The minimum path to get to element `A[i][j]` is the minimum of `A[i - 1][j - 1]`, `A[i - 1][j]` and `A[i - 1][j + 1]`.
Starting from row 1, we add the minumum path to each element. The smallest number in the last row is the miminum path sum.

```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& A) {
        for(auto i = 1; i < A.size(); i++) {
            for(auto j = 0; j < A.size(); j++) {
                A[i][j] += min({ A[i-1][j], A[i-1][max(0,j-1)], A[i-1][min((int)A.size()-1,j+1)] });
            }
        }
        return *min_element(begin(A[A.size() - 1]), end(A[A.size() - 1]));
    }
};
```