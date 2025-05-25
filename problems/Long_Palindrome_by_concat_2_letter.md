# Longest Palindrome by Concatenating Two Letter Words

You are given an array of strings words. Each element of words consists of two lowercase English letters.

Create the longest possible palindrome by selecting some elements from words and concatenating them in any order. Each element can be selected at most once.

Return the length of the longest palindrome that you can create. If it is impossible to create any palindrome, return 0.

A palindrome is a string that reads the same forward and backward.

 

Example 1:

Input: words = ["lc","cl","gg"]

Output: 6

Explanation: One longest palindrome is "lc" + "gg" + "cl" = "lcggcl", of length 6.

Note that "clgglc" is another longest palindrome that can be created.

Example 2:

Input: words = ["ab","ty","yt","lc","cl","ab"]

Output: 8

Explanation: One longest palindrome is "ty" + "lc" + "cl" + "yt" = "tylcclyt", of length 8.

Note that "lcyttycl" is another longest palindrome that can be created.

Example 3:

Input: words = ["cc","ll","xx"]

Output: 2

Explanation: One longest palindrome is "cc", of length 2.

Note that "ll" is another longest palindrome that can be created, and so is "xx".
 

Constraints:

1 <= words.length <= 105

words[i].length == 2

words[i] consists of lowercase English letters.


# Solution

```JavaScript

/**
 * @param {string[]} words
 * @return {number}
 */
var longestPalindrome = function(words) {
    const count = new Map();
    for (const word of words) {
        count.set(word, (count.get(word) || 0) + 1);
    }

    let result = 0;
    let hasCenter = false;

    for (const [word, freq] of count.entries()) {
        const reversed = word[1] + word[0];

        if (word === reversed) {
            // Words like 'cc', 'aa'
            const pairs = Math.floor(freq / 2);
            result += pairs * 4;
            if (freq % 2 === 1) hasCenter = true;
        } else if (count.has(reversed)) {
            const pairs = Math.min(freq, count.get(reversed));
            result += pairs * 4;
            // Set both counts to 0 to avoid reprocessing
            count.set(word, 0);
            count.set(reversed, 0);
        }
    }

    // One central palindromic word like 'cc' can be placed in the middle
    if (hasCenter) result += 2;

    return result;
};
```
