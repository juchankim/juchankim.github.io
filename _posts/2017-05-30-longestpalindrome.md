---
layout: page
tags: ['Leetcode']
title: Longest Palindromic Substring
---

May 30th, 2017

My third coding challenge was ["Longest Palindromic Substring"](https://leetcode.com/problems/longest-palindromic-substring/)


According to the site, 

"Given a string s, find the longest palindromic substring in s."

I actually remember this from numerous interviews and I remember giving a solution that was very basic and a brute way to do it. 

It was to just go through each index and keep looping through following indices until it is no longer a palindrome. Then you save the best number and the substring until you find a better one in future indices. This would be O(N^3) for time complexity (N = length of string) since you go through the indices twice and one more time to check if the string is a palindrome. 

One way to reduce the time complexity is to remove the innermost for loop in which we check if the string is a palindrome. This can be done simply through starting from the index and go left AND right. For each index, we will still go through whole string worst-case to see which substring can be the longest AND palindromic one. However, by going left and right, we eliminate the need to check for palindromicity of the entire substring over and over again since we check the reflectivity (if left and right have the equal character). Thus, this is how the entire process is reduced to O(N^2).

I believe that there is a way (a special algorithm) to make this a O(N). But unless you know the algorithm, I do not believe that it is possible to come up with it in a timed and pressured space. In my opionion, it is most logical to start with the most basic and brute way and keep optimizing. Maybe, we can come up with it during our optimization process.
<!-- ```python
def longestPalindrome(self, s):
    """
    :type s: str
    :rtype: str
    """
    if (len(s) == 0):
        return 0
    if (len(s) == 1):
        return s
    best = 1
    bestLeft = 0
    bestRight = 0
    for i in range(len(s)):
        left = i-1
        right = i+1
        tempBest = 1
        tempBestLeft = i
        tempBestRight = i
        while (right < len(s) and s[right] == s[i]):
            tempBest += 1
            tempBestRight = right
            right += 1
            
        while(left >= 0 and s[left] == s[i]):
            tempBest += 1
            tempBestLeft= left
            left -= 1
            
        while (left >= 0 and right < len(s)):
            if s[left]==s[right]:
                tempBest += 2
                tempBestLeft = left
                tempBestRight = right
            else:
                break
            left -= 1
            right += 1
        if (tempBest > best):
            best = tempBest
            bestLeft = tempBestLeft
            bestRight = tempBestRight
            print(best, bestLeft, bestRight)
    return s[bestLeft:bestRight+1]
``` -->