# [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

## description

Given a 2D board containing 'X' and 'O' (the letter O), capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

Example:

    X X X X
    X O O X
    X X O X
    X O X X
After running your function, the board should be:

    X X X X
    X X X X
    X X X X
    X O X X
Explanation:

Surrounded regions shouldn’t be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.

### analysis

If a 'O' region contains a block on the edge of the matrix, the region remains 'O'. Otherwise, the region should be replaced by 'X'.

## Solutions and Thoughts

    class Solution {
    public:
        int dx[4] = {0, 1, 0, -1};
        int dy[4] = {1, 0, -1, 0};
        void solve(vector<vector<char>>& board) {
            if(board.size() == 0 || board[0].size() == 0)
                return;
            int m = board.size();
            int n = board[0].size();
            
            for(int i = 0; i < m; i++)
                for(int j = 0; j < n ; j++) {
                    if(i == 0 || i == m - 1 || j == 0 || j == n - 1)
                        if(board[i][j] == 'O')
                            dfs(i, j, board);
                }
            
            for(int i = 0; i < m; i ++) 
                for(int j = 0; j < n; j++) {
                    if(board[i][j] == 'O')
                        board[i][j] = 'X';
                    if(board[i][j] == 'S')
                        board[i][j] = 'O';
                }
        }
        void dfs(int x, int y, vector<vector<char>>& board) {
            board[x][y] = 'S';
            for(int i = 0; i < 4; i++) {
                int X = x + dx[i];
                int Y = y + dy[i];
                if(X >= 0 && X < board.size() && Y > 0 && Y < board[0].size() && board[X][Y] == 'O')
                    dfs(X, Y, board);
            }
        }
    };
**THOUGHTS:** Search from the boundaries. If one block on the edge is 'O', go DFS to visit all blocks connected and replace them with 'S'. When all work is done, the 'O' blocks is not reached and mark them with 'X'. The 'O' blocks are what we truly want so move them back to 'O'.

- Check any dimension of the matrix is 0 ===> may lead to the error below (memory out of bounds)
> reference binding to null pointer of type 'struct value_type' (stl_vector.h)