# Find Building Where Alice and Bob Can Meet

You are given a 0-indexed array heights of positive integers, where heights[i] represents the height of the ith building.

If a person is in building i, they can move to any other building j if and only if i < j and heights[i] < heights[j].

You are also given another array queries where queries[i] = [ai, bi]. On the ith query, Alice is in building ai while Bob is in building bi.

Return an array ans where ans[i] is the index of the leftmost building where Alice and Bob can meet on the ith query. If Alice and Bob cannot move to a common building on query i, set ans[i] to -1.

 

Example 1:

Input: heights = [6,4,8,5,2,7], queries = [[0,1],[0,3],[2,4],[3,4],[2,2]]

Output: [2,5,-1,5,2]

Explanation: In the first query, Alice and Bob can move to building 2 since heights[0] < heights[2] and heights[1] < heights[2]. 

In the second query, Alice and Bob can move to building 5 since heights[0] < heights[5] and heights[3] < heights[5]. 

In the third query, Alice cannot meet Bob since Alice cannot move to any other building.

In the fourth query, Alice and Bob can move to building 5 since heights[3] < heights[5] and heights[4] < heights[5].

In the fifth query, Alice and Bob are already in the same building.  

For ans[i] != -1, It can be shown that ans[i] is the leftmost building where Alice and Bob can meet.

For ans[i] == -1, It can be shown that there is no building where Alice and Bob can meet.

Example 2:

Input: heights = [5,3,8,2,6,1,4,6], queries = [[0,7],[3,5],[5,2],[3,0],[1,6]]

Output: [7,6,-1,4,6]

Explanation: In the first query, Alice can directly move to Bob's building since heights[0] < heights[7].

In the second query, Alice and Bob can move to building 6 since heights[3] < heights[6] and heights[5] < heights[6].

In the third query, Alice cannot meet Bob since Bob cannot move to any other building.

In the fourth query, Alice and Bob can move to building 4 since heights[3] < heights[4] and heights[0] < heights[4].

In the fifth query, Alice can directly move to Bob's building since heights[1] < heights[6].

For ans[i] != -1, It can be shown that ans[i] is the leftmost building where Alice and Bob can meet.

For ans[i] == -1, It can be shown that there is no building where Alice and Bob can meet.

 

Constraints:

1 <= heights.length <= 5 * 104

1 <= heights[i] <= 109

1 <= queries.length <= 5 * 104

queries[i] = [ai, bi]

0 <= ai, bi <= heights.length - 1

# Solution

```JavaScript
/**
 * @param {number[]} heights
 * @param {number[][]} queries
 * @return {number[]}
 */
var leftmostBuildingQueries = function (heights, queries) {
  const n = heights.length;
  const m = queries.length;

  // answer array, default -1
  const res = Array(m).fill(-1);

  // For every building i we’ll store queries whose “right end” is i
  // Each element: [neededHeight, queryIndex]
  const bucket = Array.from({ length: n }, () => []);

  // --- 1.  Pre-classify queries ------------------------------------------
  for (let i = 0; i < m; ++i) {
    let a = queries[i][0];
    let b = queries[i][1];

    if (a > b) [a, b] = [b, a];          // ensure a < b like in C++ code

    // already meetable?
    if (heights[b] > heights[a] || a === b) {
      res[i] = b;
    } else {
      // otherwise we’ll solve later when scanning from right to left
      bucket[b].push([heights[a], i]);
    }
  }

  // --- 2.  Monotone stack (decreasing by height) while sweeping right→left
  /** @type {[number, number][]}  pair: [height, index] */
  const stk = [];

  // Helper: binary-search in stack for first element with height > h
  const upperGreaterPos = (h) => {
    let lo = 0, hi = stk.length - 1, ans = -1;
    while (lo <= hi) {
      const mid = (lo + hi) >> 1;
      if (stk[mid][0] > h) {      // taller ⇒ candidate
        ans = mid;
        lo = mid + 1;             // search further right (stack is decreasing)
      } else {
        hi = mid - 1;
      }
    }
    return ans;                   // -1 if not found
  };

  for (let i = n - 1; i >= 0; --i) {
    // Resolve queries whose right end is i
    for (const [needH, qi] of bucket[i]) {
      const pos = upperGreaterPos(needH);
      if (pos !== -1) res[qi] = stk[pos][1];
    }

    // Maintain decreasing stack
    while (stk.length && stk[stk.length - 1][0] <= heights[i]) stk.pop();
    stk.push([heights[i], i]);
  }

  return res;
};
```
