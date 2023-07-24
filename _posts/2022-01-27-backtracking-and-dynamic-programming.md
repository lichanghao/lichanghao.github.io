---
title:             "Backtracking and dynamic programming - some random thinking from the permutation problem"
date:              2021-01-27
permalink:         /posts/2021/01/27/backtracking-and-dynamic-programming
tags:              Programming
---

This is a quick note recording my coding practice.

# The permutation problem

Given an array nums of **distinct** integers, return all the possible permutations. You can return the answer in any order.

Example:

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```


# My solution with dynamic programming

I first came up the solution using dynamic programming. Given any number array `num`, you can remove its beginning `num[0]` then recursively calculate the permutation of this one-element-shorter array. Inserting `num[0]` into this shorter permutation, traversing all possible position will give us the solution.

So the psuedocode would be:

```
def permutate(nums):
	result = []
	if length(nums) = 1
		return [nums]
	temp = permutate(nums[1:end])
	for p in temp:
		for i in 0:length(p):
			p.insert(i, nums[0])
			result.insert(p)
	return result
```

My C++ solution based on this method is:

```c++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> result;
        if (nums.size() == 1)
        {
            result.push_back(nums);
            return result;
        }
        if (nums.size() == 2)
        {
            result.push_back(nums);
            vector<int> tmp = {nums[1], nums[0]};
            result.push_back(tmp);
            return result;
        }

        vector<int> temp = nums;
        int a = temp[temp.size()-1];
        temp.pop_back();
        vector<vector<int>> small = permute(temp);

        for (int i = 0; i < small.size(); i++)
        {
            for (int j = 0; j < small[i].size(); j++)
            {
                vector<int> per = small[i];
                per.insert(per.begin() + j, a);
                result.push_back(per);
            }
            vector<int> per = small[i];
            per.push_back(a);
            result.push_back(per);
        }
        return result;
    }
};
```


# Solution with backtracking

After finishing this problem, I checked other people's solution and found that most of them were using backtracking. Backtracking is a method that "cleverly" exploring the result space that can avoid unnecessary search routes. The famous Traveling Salesman problem can be solved by backtracking.

Basically, here backtracking is somewhat similar with dynamic programming approach, since they are both constructing the final solution from bottom to top. However, the concept of constructing is different. For this problem, backtracking builds a tree, where the final leaf nodes are elements of the solution, and backtracking uses DFS to find all the final leaf nodes. Mathematically, it is equivalent to:

```
result = []
Traverse all possible candidates:
	p = []
	repeat length(nums) times:
		a = one unused element in nums
		p.insert(a)
	result.insert(p)
```

One example of C++ code is:

```c++
class Solution {
public:
    vector<vector<int>> v;
        void solve(vector<int> &nums,int left,int right){
            if (left==right){
                v.push_back(nums);
                return;
            }
            else{
                for(int i=left;i<=right;i++){
                    swap(nums[left],nums[i]);
                    solve(nums,left+1,right);
                    swap(nums[left],nums[i]);
                }
            }

        }


    vector<vector<int>> permute(vector<int>& nums) {
        solve(nums,0,nums.size()-1);
        return v;
    }
};
```

# Differences

Assume we have a solution tree, whose leaves are the solutions and other nodes are subsolutions of one part of the problem. **Dynamic programming is more like BFS**, since it must visit(calculate) all the nodes on the same level to calculate the next-level-node. In contrast, **backtracking is more like DFS**, since it aims at first finding one leaf node. During this in-depth search, if the problem constraint is not satisfied, the route is ended and new search starts.

For this specific problem, dynamic programming solves the problem by recursivly finding "the permutation of last n elements", then insert the remaining element. Backtracking solves the problem by constructing all possibilities, using the concept "repeat picking one unused element from nums until it forms a permutation". The difference between the two method is conceptually quite similar to the difference between BFS and DFS.

If we add additional constraints to this problem, then backtracking outperforms dynamic programming, because backtracking is more good at solving problem with various constraints, but dynamic programming need "optimal substructure" property which limits its application.

For example, consider this problem:

Given a collection of numbers, nums, that **might contain duplicates**, return all possible unique permutations in any order.

Backtracking can easily solve above problem by only using unique number constructing the solution candidates. However, dynamic programming will encounter difficuties because it is unclear that how to link the subproblems "the unrepeated permutation of last n elements".

Example code:

```c++
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        map<int, int> m;
        for(auto const &num: nums){
            if(m.find(num) != m.end()) m[num]++;
            else m[num] = 1;
        }
        vector<pair<int, int>> mt;
        for(auto const &[k, v]: m){
            mt.push_back(pair<int, int>{k, v});
        }
        vector<vector<int>> res;
        vector<int> r;
        dfs(nums, res, r, 0, mt);
        return res;
    }
    void dfs(vector<int> &nums, vector<vector<int>> &res, vector<int> &r, int level, vector<pair<int, int>> &mt){
        if(level == nums.size()){
            res.push_back(r);
        }
        else{
            for(int i = 0; i < mt.size(); i++){
                int num = mt[i].first;
                int freq = mt[i].second;
                if(freq <= 0) continue;
                r.push_back(num);
                mt[i].second--;
                dfs(nums, res, r, level + 1, mt);
                r.pop_back();
                mt[i].second++;
            }
        }
    }
};
```
