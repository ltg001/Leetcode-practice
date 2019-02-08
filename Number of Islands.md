# [Number of Islands](https://leetcode.com/problems/number-of-islands/)

## Description

Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Example 1:

    Input:
    11110
    11010
    11000
    00000

    Output: 1

Example 2:

    Input:
    11000
    11000
    00100
    00011

    Output: 3

### Analysis

Go DFS from any point and search the sourrounding blocks. If the block is '1', replace it with other signs, such as '2', to show that this block has been reached. If the search queue is empty (or no new block for recursion), it shows that every block in this island has been visited then the counter adds.

## Solutions and Thoughts

1. **DFS**

        class Solution {
        public:
            int numIslands(vector<vector<char>>& grid) {
                int counter = 0;
                for(int i = 0; i < grid.size(); i++)
                    for(int j = 0; j < grid[0].size(); j++) {
                        if(dfs(i, j, grid))
                            counter ++;
                    }
                return counter;
            }
            bool dfs(int x, int y, vector<vector<char>>& grid) {
                int dx[] = {0, -1, 0, 1};
                int dy[] = {1, 0, -1, 0};
                if(grid[x][y] == '0' || grid[x][y] == '2')
                    return false;
                grid[x][y] = '2';
                for(int i = 0; i < 4; i++) {
                    int X = x + dx[i];
                    int Y = y + dy[i];
                    if(X >= 0 && X < grid.size() && Y >= 0 && Y < grid[0].size()) {
                        bool s = dfs(X, Y, grid);
                    }
                }
                return true;
            }
        };
    **THOUGHTS:** For all blocks to be checked, examine whether the block is a new island. The return value of function dfs is only relevent to the original call by function nunIsland. Other recursion processes only deal with the neighbor blocks.

2. **BFS**

        class Solution {
        public:
            int numIslands(vector<vector<char>>& grid) {
                int islands = 0;
                for (int i = 0; i < grid.size(); i++) {
                    for (int j = 0; j < grid[0].size(); j++) {
                        if (grid[i][j] == '1') {
                            islands++;
                            queue<pair<int, int>> todo;
                            todo.push(make_pair(i, j));
                            grid[i][j] = '0';
                            while (!todo.empty()) {
                                pair<int, int> p = todo.front();
                                todo.pop();
                                int r = p.first, c = p.second;
                                if (r && grid[r - 1][c] == '1') {
                                    todo.push(make_pair(r - 1, c));
                                    grid[r - 1][c] = '0';
                                }
                                if (r + 1 < grid.size() && grid[r + 1][c] == '1') {
                                    todo.push(make_pair(r + 1, c));
                                    grid[r + 1][c] = '0';
                                }
                                if (c && grid[r][c - 1] == '1') {
                                    todo.push(make_pair(r, c - 1));
                                    grid[r][c - 1] = '0';
                                }
                                if (c + 1 < grid[0].size() && grid[r][c + 1] == '1') {
                                    todo.push(make_pair(r, c + 1));
                                    grid[r][c + 1] = '0';
                                }
                            }
                        }
                    }
                }
                return islands;
            }
        };
    **ATTENTION:** Pay attention to memory use. May lead to MLE.