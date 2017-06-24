---
layout: page
tags: ['Leetcode']
title: Longest Substring Without Repeating Characters
---

May 31st, 2017

My fourth coding challenge was ["Longest Substring Without Repeating Characters"](https://leetcode.com/problems/longest-substring-without-repeating-characters)


According to the site, 

"Given a string, find the length of the longest substring without repeating characters."

The first approach I thought of was the brute one to just go through each index and loop until no repeating character appears by using a hash. This would take O(N^2) where N is the length of the string. However, I experienced timeout. 

Would it be possible to bring it down to O(N)? This is so because s can be really long and to have N^2 hurts the time a lot. Since it has to be consecutive, we can keep updating the start value to the index after the last repeated character. And looking up the last repeated character would be O(1) as we store the last seen index of that character in the hash. 

<!-- ```python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        bestlen = 0
        hashed = {}
        start = 0
        for i in range(len(s)):
            temp = 0
            if s[i] in hashed and start <= hashed[s[i]]: # update the start only if hash has the character and that index is closer to i than the start index. 
               start = hashed[s[i]] + 1 # start from where it does not repeat
            temp = i - start + 1
            if temp > bestlen:
                bestlen = temp
            hashed[s[i]] = i # always update the hash with the recently seen 
        return bestlen
``` -->
