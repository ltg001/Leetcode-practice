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