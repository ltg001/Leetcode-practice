# [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

## Description

Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

Example 1:

    Input:
    [
    [ 1, 2, 3 ],
    [ 4, 5, 6 ],
    [ 7, 8, 9 ]
    ]
    Output: [1,2,3,6,9,8,7,4,5]
Example 2:

    Input:
    [
    [1, 2, 3, 4],
    [5, 6, 7, 8],
    [9,10,11,12]
    ]
    Output: [1,2,3,4,8,12,11,10,9,5,6,7]

## Solution

There is no data structure or algorithm in this problem.
Just care about the boundary conditions.

```cpp
class Solution {
    void print(int index, vector<int>& ans, vector<vector<int>>& matrix) {
        int endX = matrix[0].size() - 1 - index;
        int endY = matrix.size() - 1 - index;
        
        for(int i = index; i <= endX; i++) {
            ans.push_back(matrix[index][i]);
        }
        
        if(index < endY) {
            for(int i = index + 1; i <= endY; i++) {
                ans.push_back(matrix[i][endX]);
            }
        }
        
        if(index < endX && index < endY) {
            for(int i = endX - 1; i >= index; i--) {
                ans.push_back(matrix[endY][i]);
            }
        }
        
        if(index < endY - 1 && index < endX) {
            for(int i = endY - 1; i >= index + 1; i--) {
                ans.push_back(matrix[i][index]);
            }
        }
    }
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if(!matrix.size() || !matrix[0].size()) return {};
        vector<int> ans;
        int index = 0;
        while(index * 2 < matrix.size() && index * 2 < matrix[0].size()) {
            print(index, ans, matrix);
            index ++;
        }
        return ans;
    }
};
```