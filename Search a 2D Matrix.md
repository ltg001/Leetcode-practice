# [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

## Description

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.
Example 1:

    Input:
    matrix = [
    [1,   3,  5,  7],
    [10, 11, 16, 20],
    [23, 30, 34, 50]
    ]
    target = 3
    Output: true
Example 2:

    Input:
    matrix = [
    [1,   3,  5,  7],
    [10, 11, 16, 20],
    [23, 30, 34, 50]
    ]
    target = 13
    Output: false

## Solution

The same code

# [Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)

## Description

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted in ascending from left to right.
Integers in each column are sorted in ascending from top to bottom.
Example:

Consider the following matrix:

    [
    [1,   4,  7, 11, 15],
    [2,   5,  8, 12, 19],
    [3,   6,  9, 16, 22],
    [10, 13, 14, 17, 24],
    [18, 21, 23, 26, 30]
    ]
Given target = `5`, return `true`.

Given target = `20`, return `false`.

## Solution

Think about keeping checking the element in the corner, such as the one on the right top corner. If it is larger than what we are looking for, we do not think about the row. If it is less than target, we don't care the column.

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.size() == 0 || matrix[0].size() == 0)
            return 0;
        int curRow = 0;
        int curCol = matrix[0].size() - 1;
        while(curRow < matrix.size() && curCol >= 0) {
            if(matrix[curRow][curCol] == target)
                return true;
            else if(matrix[curRow][curCol] > target) {
                curCol -= 1;
            } else {
                curRow += 1;
            }
        }
        return false;
    }
};
```