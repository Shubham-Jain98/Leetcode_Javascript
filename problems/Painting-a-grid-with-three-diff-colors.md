# Painting a Grid With Three Different Colors

You are given two integers m and n. Consider an m x n grid where each cell is initially white. You can paint each cell red, green, or blue. All cells must be painted.

Return the number of ways to color the grid with no two adjacent cells having the same color. Since the answer can be very large, return it modulo 109 + 7.

 

Example 1:


Input: m = 1, n = 1
Output: 3
Explanation: The three possible colorings are shown in the image above.
Example 2:


Input: m = 1, n = 2
Output: 6
Explanation: The six possible colorings are shown in the image above.
Example 3:

Input: m = 5, n = 5
Output: 580986
 

Constraints:

1 <= m <= 5
1 <= n <= 1000

# Solution

```JavaScript
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var colorTheGrid = function (m, n) {
  const MOD = 1_000_000_007;
  const colors = [0, 1, 2];            // red, green, blue

  // ---------- 1. generate all valid column patterns ----------
  const patterns = [];
  const makePatterns = (row = 0, cur = []) => {
    if (row === m) {
      patterns.push([...cur]);
      return;
    }
    for (const c of colors) {
      if (row > 0 && c === cur[row - 1]) continue; // vertical rule
      cur[row] = c;
      makePatterns(row + 1, cur);
    }
  };
  makePatterns();

  const S = patterns.length;

  // ---------- 2. pre-compute compatibility ----------
  const comp = Array.from({ length: S }, () => []);
  for (let i = 0; i < S; ++i) {
    for (let j = 0; j < S; ++j) {
      let ok = true;
      for (let r = 0; r < m; ++r) {
        if (patterns[i][r] === patterns[j][r]) {
          ok = false;
          break;
        }
      }
      if (ok) comp[i].push(j);   // j can follow i
    }
  }

  // ---------- 3. DP along columns ----------
  let dp = new Array(S).fill(1);       // first column: 1 way each
  for (let col = 1; col < n; ++col) {
    const next = new Array(S).fill(0);
    for (let p = 0; p < S; ++p) {
      const val = dp[p];
      if (val === 0) continue;
      for (const q of comp[p]) {
        next[q] = (next[q] + val) % MOD;
      }
    }
    dp = next;
  }

  // ---------- 4. sum up ----------
  return dp.reduce((a, b) => (a + b) % MOD, 0);
};
