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

### Memoization

Recursion is often an intuitive and powerful way to implement an algorithm. However, it might bring some undesired penalty to the performance, *e.g.* duplicate calculations, if we do not use it wisely. For instance, at the end of the previous chapter, we have encountered the duplicate calculations problem in Pascal's Triangle, **where some intermediate results are calculated multiple times.**
To eliminate the duplicate calculation, as many of you would have figured out, one of the ideas would be to store the intermediate results in the cache so that we could reuse them later without re-calculation.
This idea is also known as memoization, which is a technique that is frequently used together with recursion.

> Memoization is an optimization technique used primarily to speed up computer programs by storing the results of expensive function calls and returning the cached result when the same inputs occur again.

### Complexity Analysis

[Time Complexity - Recursion](https://leetcode.com/explore/learn/card/recursion-i/256/complexity-analysis/1669/)
[Space Complexity - Recursion](https://leetcode.com/explore/learn/card/recursion-i/256/complexity-analysis/1671/)
[Tail Recursion](https://leetcode.com/explore/learn/card/recursion-i/256/complexity-analysis/2374/)

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

```cpp
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
```

This solution calculates every line of the triangle. Obvously, the extra space used more than O(k).

2. **O(k) Space Complexity Edition**

```cpp
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
```

Think about calculate the next line with the previous one iteratively. Use two *for* loop to complete the calculation.

### [Reverse Linked List](https://leetcode.com/explore/learn/card/recursion-i/251/scenario-i-recurrence-relation/2378/)

Reverse a singly linked list.

Example:

    Input: 1->2->3->4->5->NULL
    Output: 5->4->3->2->1->NULL
Follow up:
A linked list can be reversed either iteratively or recursively. Could you implement both?

#### Solution

1. **iteratively**

```cpp
        /**
        * Definition for singly-linked list.
        * struct ListNode {
        *     int val;
        *     ListNode *next;
        *     ListNode(int x) : val(x), next(NULL) {}
        * };
        */
        class Solution {
        public:
            ListNode* reverseList(ListNode* head) {
                ListNode* newHead = NULL;
                while(head) {
                    ListNode* temp = head -> next;
                    head -> next = newHead;
                    newHead = head;
                    head = temp;
                }
                return newHead;
            }
        };
```

    Set a newHead(ListNode pointer), initialized with NULL. Make it a new head of the linked list and put the rest nodes behind the position.
<br>
    Time Complexity `O(n)`
    Space Complexity `O(1)`
<br>
        newHead = NULL                head = [1 -> 2 -> 3 -> 4 -> 5]
        newHead = [1]                 head = [2 -> 3 -> 4 -> 5]
        newHead = [2 -> 1]            head = [3 -> 4 -> 5]
        ......

2. **recursively**

```cpp
        /**
        * Definition for singly-linked list.
        * struct ListNode {
        *     int val;
        *     ListNode *next;
        *     ListNode(int x) : val(x), next(NULL) {}
        * };
        */
        class Solution {
        public:
            ListNode* reverseList(ListNode* head) {
                if(!head || ! head -> next)
                    return head;
                ListNode* newHead = reverseList(head -> next);
                head -> next -> next = head;
                head -> next = NULL;
                return newHead;
            }
        };
```

- find the tail node recursively
- move one privious node right after the tail node (head -> next -> next = head) ===> the relative position is consistent
    <br>
        Time Complexity `O(n)`
        Space Complexity `O(n)`
    <br>

### [Fibonacci Number](https://leetcode.com/explore/learn/card/recursion-i/255/recursion-memoization/1661/)

The Fibonacci numbers, commonly denoted F(n) form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is,

    F(0) = 0,   F(1) = 1
    F(N) = F(N - 1) + F(N - 2), for N > 1.
Given N, calculate F(N).

Example 1:

    Input: 2
    Output: 1
    Explanation: F(2) = F(1) + F(0) = 1 + 0 = 1.
Example 2:

    Input: 3
    Output: 2
    Explanation: F(3) = F(2) + F(1) = 1 + 1 = 2.
Example 3:

    Input: 4
    Output: 3
    Explanation: F(4) = F(3) + F(2) = 2 + 1 = 3.

**Note:** 0 ≤ N ≤ 30.

#### Solution

```cpp
class Solution {
public:
    int fib(int N) {
        vector<int> v;
        v.push_back(0);
        v.push_back(1);
        if(N == 0) return 0;
        if(N == 1) return 1;
        for(int i = 2; i <= N; ++i) {
            v.push_back(v[i - 1] + v[i - 2]);
        }
        return v[N];
    }
};
```

It is easy to figure out that F(0) and F(1) will be used many times. Thus we can store them rather than calculate them many times.

### [Climbing Stairs](https://leetcode.com/explore/learn/card/recursion-i/255/recursion-memoization/1662/)

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.

Example 1:

    Input: 2
    Output: 2
    Explanation: There are two ways to climb to the top.
    1. 1 step + 1 step
    2. 2 steps
Example 2:

    Input: 3
    Output: 3
    Explanation: There are three ways to climb to the top.
    1. 1 step + 1 step + 1 step
    2. 1 step + 2 steps
    3. 2 steps + 1 step

#### Solution

```cpp
class Solution {
public:
    int climbStairs(int n) {
        vector<int> v;
        v.push_back(1);
        v.push_back(2);
        if(n == 1) return 1;
        if(n == 2) return 2;
        for(int i = 2; i < n; ++ i) {
            v.push_back(v[i - 1] + v[i - 2]);
        }
        return v[n - 1];
    }
};
```

Thoughts:

1. Declear an array Array[n], and each element means how many ways can you get to this stair. For example, Array[x] == 10 means there are 10 ways to reach stair #10.
2. Figure out how to reach a new stair.
   1. From position x-2, climb 2 stairs to get to get position x.
   2. From position x-1, climb 1 stair to get to get position x.
===> Array[x] = Array[x - 1] + Array[x - 2]

### [Maximum Depth of Binary Tree](https://leetcode.com/explore/learn/card/recursion-i/256/complexity-analysis/2375/)

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Note: A leaf is a node with no children.

Example:

Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its depth = 3.

#### Solution

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root) return 0;
        return max(maxDepth(root -> left), maxDepth(root -> right)) + 1;
    }
};
```

simple question

### [K-th Symbol in Grammar](https://leetcode.com/explore/learn/card/recursion-i/253/conclusion/1675/)

On the first row, we write a 0. Now in every subsequent row, we look at the previous row and replace each occurrence of 0 with 01, and each occurrence of 1 with 10.

Given row N and index K, return the K-th indexed symbol in row N. (The values of K are 1-indexed.) (1 indexed).

Examples:
    Input: N = 1, K = 1
    Output: 0

    Input: N = 2, K = 1
    Output: 0

    Input: N = 2, K = 2
    Output: 1

    Input: N = 4, K = 5
    Output: 1

    Explanation:
    row 1: 0
    row 2: 01
    row 3: 0110
    row 4: 01101001

**Note:**

1. N will be an integer in the range [1, 30].
2. K will be an integer in the range [1, 2^(N-1)].

#### Solution

1. Naive Solution

    Pretty simple thoughts: store all intermediate results in a 2 dimentions vector.
    But get **MLE**.

```cpp
    class Solution {
    public:
        int kthGrammar(int N, int K) {
            vector<vector<int>> v;
            v.resize(N, vector<int>());
            v[0].push_back(0);
            
            if(N == 1) return 0;
            
            for(int i = 1; i < N; i++) {
                for(int j = 0; j < v[i - 1].size(); ++ j) {
                    if(v[i - 1][j] == 0) {
                        v[i].push_back(0);
                        v[i].push_back(1);
                    }
                    else {
                        v[i].push_back(1);
                        v[i].push_back(0);
                    }
                }
            }
            return v[N - 1][K - 1];
        }
    };
```

2. Tree Model Solution

    The problem require viewing `0` as `01` and `1` as `10`. Thus it forms a binary tree. It is obvious that using recursion is a great approach.
    *The biggest problem in `recursive relation` is finding the relation itself.*

```cpp
    class Solution {
    public:
        int kthGrammar(int N, int K) {
            if(N == 0) return 0;
            if(K & 1) return kthGrammar(N - 1, (K + 1) / 2) == 0 ? 0 : 1;
            else return kthGrammar(N - 1, K / 2) == 0 ? 1 : 0;
        }
    };
```

### [Unique Binary Search Trees II](https://leetcode.com/explore/learn/card/recursion-i/253/conclusion/2384/)

