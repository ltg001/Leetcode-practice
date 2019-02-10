# [3Sum](https://leetcode.com/problems/3sum/)

## Description

Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:
The solution set must not contain duplicate triplets.

Example:

    Given array nums = [-1, 0, 1, 2, -1, -4],

    A solution set is:
    [
    [-1, 0, 1],
    [-1, -1, 2]
    ]

### Analysis

We want to find 3 numbers which add up to 0.
The solution have 3 steps

1. Sort the whole array given.
2. If the min value greater than 0 or max value lower than 0, there exists no solution.
3. Enumerate every elements in the array. Figure out the rest by approching from both sides.

The total time comlexity is O(n^2).

## Solutions

    class Solution {
    public:
        vector<vector<int>> threeSum(vector<int>& nums) {
            vector<vector<int>> ans;
            sort(nums.begin(), nums.end());
            if(nums.empty() || nums.front() > 0 || nums.back() < 0)
                return {};
            for(int i = 0; i < nums.size(); i++) {
                if(nums[i] > 0) break;
                if(i > 0 && nums[i] == nums[i - 1]) continue;
                int tar = -1 * nums[i];
                int left = i + 1;
                int right = nums.size() - 1;
                while(left < right) {
                    if(nums[left] + nums[right] == tar) {
                        ans.push_back({nums[i], nums[left], nums[right]});
                        while(left < right && nums[left] == nums[left + 1]) left ++;
                        while(left < right && nums[right] == nums[right - 1]) right --;
                        left ++; right --;
                    }
                    else if(nums[left] + nums[right] > tar) {
                        right --;
                    }
                    else {
                        left ++;
                    }
                }
            }
            return ans;
        }
    };