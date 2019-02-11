# [Combination Sum](https://leetcode.com/problems/combination-sum/)

## Description

Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

The same repeated number may be chosen from candidates unlimited number of times.

Note:

All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
Example 1:

    Input: candidates = [2,3,6,7], target = 7,
    A solution set is:
    [
    [7],
    [2,2,3]
    ]
Example 2:

    Input: candidates = [2,3,5], target = 8,
    A solution set is:
    [
    [2,2,2,2],
    [2,3,3],
    [3,5]
    ]

### Analysis

It is easy to realize that what we need is a recursive process because the searching space is a tree.
Trying all possible combinations is obviously unacceptable. So we need go pruning.

- Sort the array given
- If the current element is larger than current targrt, abandon this trail
- If the current element equals to current target, record this solution and abandon the trail. Because any element behind is larger than 0

## Solutions

1. Naive Edition

        class Solution {
        public:
            vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
                sort(candidates.begin(), candidates.end());
                vector<vector<int>> ans;
                if(candidates.empty())
                    return ans;
                vector<int> cur_sol;
                solve(0, target, cur_sol, ans, candidates);
                return ans;
            }
            void solve(int cur, int target, vector<int> cur_sol, vector<vector<int>>& ans, vector<int>& candidates) {
                if(target < 0)
                    return;
                if(target == 0) {
                    ans.push_back(cur_sol);
                    return;
                }
                for(int i = cur; i < candidates.size(); i++) {
                    cur_sol.push_back(candidates[i]);
                    solve(i, target - candidates[i], cur_sol, ans, candidates);
                    cur_sol.pop_back();
                }
            }
        };
    **ATTENTION:** When finishing a trail, remember to pop the current element. Otherwise, the trail history will be stored in the answer vector ===> wrong answer.