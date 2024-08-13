# Combination-Sum-II

Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target.

Each number in candidates may only be used once in the combination.

Note: The solution set must not contain duplicate combinations.

Example 1:

Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
Example 2:

Input: candidates = [2,5,2,1,2], target = 5
Output: 
[
[1,2,2],
[5]
]
 
Constraints:

1 <= candidates.length <= 100
1 <= candidates[i] <= 50
1 <= target <= 30

# SOLUTION 

Intuition
The goal is to find all unique combinations of numbers in the given candidate array (cand) that sum up to a specified target. Since the same number can appear multiple times in the candidate list, we need to ensure that our combinations are unique. To achieve this, we will use a depth-first search (DFS) approach combined with backtracking to explore all possible combinations while avoiding duplicates.

Approach
Sorting: Start by sorting the candidate array. This allows us to easily skip duplicates and helps in managing the combinations.

Depth-First Search (DFS): Use a recursive function to explore combinations:

If the current target becomes zero, we have found a valid combination, and we add it to the results.
If the current target becomes negative, we stop exploring that path.
Iterate through the candidates starting from the current index. For each candidate:
Skip duplicates to ensure uniqueness.
Include the candidate in the current path and recursively call the DFS function with the updated target and the next index.
Backtrack by removing the last added candidate from the path to explore other possibilities.
Backtracking: After exploring a path, remove the last element added to the path to allow for new combinations.

Time and Space Complexity
Time Complexity: The time complexity is O(2^N) in the worst case, where N is the number of candidates. This is because each candidate can either be included or excluded from a combination. However, the actual number of valid combinations will be much less due to the constraints of the problem and the skipping of duplicates.

Space Complexity: The space complexity is O(N) for the recursion stack and the space used to store the results. The maximum depth of the recursion can go up to N in the worst case.

Step-by-Step Explanation
Let's consider the input:

cand = [10, 1, 2, 7, 6, 1, 5]
target = 8
Step 1: Sort the Candidates
First, we sort the candidate array:

Sorted cand = [1, 1, 2, 5, 6, 7, 10]
Step 2: Start DFS
We initiate the DFS with an empty path and the target of 8.

Step 3: Explore Combinations
Start with 1 (index 0):

Current path: ``, Remaining target: 8 - 1 = 7
Next, again 1 (index 1):

Current path: [1, 1], Remaining target: 7 - 1 = 6
Next, 2 (index 2):

Current path: [1, 1, 2], Remaining target: 6 - 2 = 4
Next, 5 (index 3):

Current path: [1, 1, 2, 5], Remaining target: 4 - 5 = -1 (invalid, backtrack)
Backtrack to [1, 1, 2] and try 6 (index 4):

Current path: [1, 1, 2, 6], Remaining target: 4 - 6 = -2 (invalid, backtrack)
Backtrack to [1, 1] and try 5 (index 3):

Current path: [1, 1, 5], Remaining target: 6 - 5 = 1
Next, 6 (index 4):

Current path: [1, 1, 5, 6], Remaining target: 1 - 6 = -5 (invalid, backtrack)
Backtrack to [1, 1] and try 6 (index 4):

Current path: [1, 1, 6], Remaining target: 6 - 6 = 0 (valid combination, add to results)
Backtrack to `` and try 2 (index 2):

Current path: [1, 2], Remaining target: 7 - 2 = 5
Next, 5 (index 3):

Current path: [1, 2, 5], Remaining target: 5 - 5 = 0 (valid combination, add to results)
Backtrack to `` and try 6 (index 4):

Current path: [1, 6], Remaining target: 7 - 6 = 1
Next, 7 (index 5):

Current path: [1, 7], Remaining target: 1 - 7 = -6 (invalid, backtrack)
Backtrack to [] and try 2 (index 2):

Current path: ``, Remaining target: 8 - 2 = 6
Next, 5 (index 3):

Current path: [2, 5], Remaining target: 6 - 5 = 1
Next, 6 (index 4):

Current path: [2, 6], Remaining target: 1 - 6 = -5 (invalid, backtrack)
Backtrack to [] and try 5 (index 3):

Current path: ``, Remaining target: 8 - 5 = 3
Next, 6 (index 4):

Current path: [5, 6], Remaining target: 3 - 6 = -3 (invalid, backtrack)
Backtrack to [] and try 6 (index 4):

Current path: ``, Remaining target: 8 - 6 = 2
Next, 7 (index 5):

Current path: [6, 7], Remaining target: 2 - 7 = -5 (invalid, backtrack)
Finally, backtrack to [] and try 7 (index 5):

Current path: ``, Remaining target: 8 - 7 = 1
Next, 10 (index 6):

Current path: [7, 10], Remaining target: 1 - 10 = -9 (invalid, backtrack)
Step 4: Collect Results
After exploring all possibilities, we find the valid combinations:

[[1, 1, 6], [1, 2, 5], [1, 7], [2, 6]]

# JAVA CODE 

class Solution {

     public List<List<Integer>> combinationSum2(int[] cand, int target) {
     
    Arrays.sort(cand);
    
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    
    List<Integer> path = new ArrayList<Integer>();
    
    dfs(cand, 0, target, path, res);
    
    return res;
}

void dfs(int[] cand, int cur, int target, List<Integer> path, List<List<Integer>> res) {

    if (target == 0) {
    
        res.add(new ArrayList(path));
        
        return ;
    }
    
    if (target < 0) return;
    
    for (int i = cur; i < cand.length; i++){
    
        if (i > cur && cand[i] == cand[i-1]) continue;
        
        path.add(path.size(), cand[i]);
        
        dfs(cand, i+1, target - cand[i], path, res);
        
        path.remove(path.size()-1);
    }
}
}
