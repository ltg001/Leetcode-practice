# [Recursion Tutorial](https://leetcode.com/explore/learn/card/recursion-i/250/principle-of-recursion/)

## Principle

> Recursion is an approach to solving problems using a function that calls itself as a subroutine.

You might wonder how we can implement a function that calls itself. The trick is that each time a recursive function calls itself, it reduces the given problem into subproblems. The recursion call continues until it reaches a point where the subproblem can be solved without further recursion.

A recursive function should have the following properties so that it does not result in an infinite loop:

1. A simple **base case** (or cases) — a terminating scenario that does not use recursion to produce an answer.
2. A set of rules, also known as **recurrence relation** that reduces all other cases towards the base case.

Note that there could be multiple places where the function may call itself.

### Recursion Function

For a problem, if there exists a recursive solution, we can follow the guidelines below to implement it.

For instance, we define the problem as the function F(X) to implement, where X is the input of the function which also defines the scope of the problem.

Then, in the function F(X), we will:

1. Break the problem down into smaller scopes.
2. Call functions F(x1), F(x2) ... F(xn) recursively to solve the subproblems of X.
3. Finally, process the results from the recursive function calls to solve the problem corresponding to X.

### Recurence Relation

There are two important things that one needs to figure out before implementing a recursive function:

- recurrence relation: the relationship between the result of a problem and the result of its subproblems.
- base case: the case where one can compute the answer directly without any further recursion calls. Sometimes, the base cases are also called bottom cases, since they are often the cases where the problem has been reduced to the minimal scale, i.e. the bottom, if we consider that dividing the problem into subproblems is in a top-down manner.

> Once we figure out the above two elements, to implement a recursive function we simply call the function itself according to the **recurrence relation** until we reach the **base case**.

## Examples

### [Reverse String](https://leetcode.com/explore/learn/card/recursion-i/250/principle-of-recursion/1440/)

Write a function that reverses a string. The input string is given as an array of characters char[].

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

You may assume all the characters consist of printable ascii characters.

Example 1:

    Input: ["h","e","l","l","o"]
    Output: ["o","l","l","e","h"]
Example 2:

    Input: ["H","a","n","n","a","h"]
    Output: ["h","a","n","n","a","H"]

#### Solution

    class Solution {
    public:
        void reverseString(vector<char>& s) {
            recursion(0, s);
        }
        void recursion(int cur, vector<char>& s) {
            if(cur == s.size())
                return;
            char ch = s[cur];
            recursion(cur + 1, s);
            s[s.size() - 1 - cur] = ch;
        }
    };
**Explanation:** The requirement are that no extra array is allowed and that the space used should be in O(1) complexity. For every letter in the string, I allocate one byte for it. Obviously, the complexity is O(1). What's more, the recursive depth is O(n), which brings invisible memory use --> stack space.

### [Swap Nodes in Pairs](https://leetcode.com/explore/learn/card/recursion-i/250/principle-of-recursion/1681/)

Given a linked list, swap every two adjacent nodes and return its head.

You may not modify the values in the list's nodes, only nodes itself may be changed.

Example:

    Given 1->2->3->4, you should return the list as 2->1->4->3.

#### Solution

    class Solution {
    public:
        ListNode* swapPairs(ListNode* head) {
            if(!head || !head -> next)
                return head;
            ListNode* temp = head -> next;
            head -> next = swapPairs(head -> next -> next);
            temp -> next = head;
            return temp;
        }
    };
**DO IT RECURSIVELY** ===> try thinking how to do it from the tail of the list.
Call the function recursively ===> the last pair goes first and reverse the remaining ones.
Maybe drawing a graph can help find the relations among the nodes.

### [Pascal's Triangle](https://leetcode.com/explore/learn/card/recursion-i/251/scenario-i-recurrence-relation/1659/)

Given a non-negative integer numRows, generate the first numRows of Pascal's triangle.
In Pascal's triangle, each number is the sum of the two numbers directly above it.

Example:

    Input: 5
    Output:
    [
        [1],
       [1,1],
      [1,2,1],
     [1,3,3,1],
    [1,4,6,4,1]
    ]

#### Solution

    class Solution {
    public:
        vector<vector<int>> generate(int numRows) {
            vector<vector<int>> ans(numRows, vector<int>());
            for(int i = 0; i < numRows; i++) {
                ans[i].resize(i + 1, 1);
                for(int j = 1; j < i; j++) {
                    ans[i][j] = ans[i - 1][j - 1] + ans[i - 1][j];
                }
            }
            return ans;
        }
    };

### [Pascal's Triangle II](https://leetcode.com/explore/learn/card/recursion-i/251/scenario-i-recurrence-relation/1660/)

Given a non-negative index k where k ≤ 33, return the kth index row of the Pascal's triangle.

Note that the row index starts from 0.

Example:

    Input: 3
    Output: [1,3,3,1]
Follow up: Could you optimize your algorithm to use only O(k) extra space?

#### Solution

1. **Naive Edition**

        class Solution {
        public:
            vector<int> getRow(int rowIndex) {
                vector<vector<int>> ans;
                ans.resize(rowIndex + 1, vector<int>());
                for(int i = 0; i <= rowIndex; i++) {
                    ans[i].resize(i + 1, 1);
                    for(int j = 1; j < i; j++)
                        ans[i][j] = ans[i - 1][j - 1] + ans[i - 1][j];
                }
                return ans[rowIndex];
            }
        };
    This solution calculates every line of the triangle. Obvously, the extra space used more than O(k).
2. **O(k) Space Complexity Edition**

        class Solution {
        public:
            vector<int> getRow(int rowIndex) {
                vector<int> res(rowIndex + 1);
                res[0] = 1;
                for (int i = 1; i <= rowIndex; ++i) {
                    for (int j = i; j >= 1; --j) {
                        res[j] += res[j - 1];
                    }
                }
                return res;
            }
        };
    Think about calculate the next line with the previous one iteratively.
    Use two *for* loop to complete the calculation.