---
layout: post
title: "[Algorithm] Sliding window"
author: "Zhengyu Wu"
---


I met with a typical problem when doing exercise on Leetcode, an online programming learning platform. 

I came across the similar problem four months ago in a test of **Wish China**. 

The two questions are both about string. The question introduced today is as the following.

```
Given a string, find the length of the longest substring without repeating characters.
```

```
Example 1:

Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", which the length is 3.
Example 2:

Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
Example 3:

Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
```
```
Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

Obviously, bruce force is the most conceivable method, but of course, not feasible. Bruce force method got its time complexity as high as O(![公式名](http://latex.codecogs.com/png.latex?n^3)). Someone may reckon the time complexity of bruce force method here is O(![公式名](http://latex.codecogs.com/png.latex?n^2)). Note that all the characters of the O(![公式名](http://latex.codecogs.com/png.latex?n^2)) index ranges should be scanned, so the final time complexity is O(![公式名](http://latex.codecogs.com/png.latex?n^3)).

Now I'll introduce an awesome method called **sliding window**, which is really powerful for resolving such problems.

Look at the java implementation using sliding window.

```
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        Set<Character> set = new HashSet<>();
        int ans = 0, i = 0, j = 0;
        while (i < n && j < n) {
            // try to extend the range [i, j]
            if (!set.contains(s.charAt(j))){
                set.add(s.charAt(j++));
                ans = Math.max(ans, j - i);
            }
            else {
                set.remove(s.charAt(i++));
            }
        }
        return ans;
    }
}
```

We record the beginning and the end of the sliding window as i and j. The only thing we should do is to ensure all charaters in the current slide window are not dupicate so that the string in the slide window is exactly a substring. Now j-i+1 is the length of the substring. 

If s[j+1] appears in the set, then move s[i] and add s with 1 until  s[j+1] doesn't appear in the set. Else, add s[j+1] to the set and add j with 1. 

![](https://github.com/GEORGE5961/markdown_photos/blob/master/Sliding_window.png?raw=true)

Notice that we add i or j with 1 at each step so the process is like sliding the window smoothly along the whole originla string. Update the globle max length after each time comparing j-i+1 and the current globle max length. 

The solution based on sliding window gets O(2n)=O(n) time complexity. In the worst case each character will be visited twice by i and j.

Reference:

[https://leetcode.com/problems/longest-substring-without-repeating-characters/solution/](https://leetcode.com/problems/longest-substring-without-repeating-characters/solution/)



