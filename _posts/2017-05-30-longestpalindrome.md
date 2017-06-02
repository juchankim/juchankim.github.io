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

It was to just go through each index and keep looping the next indices until it is no longer a palindrome. Then you save the best number and the substring until you find a better one in future indices. This would be O(N^3) for time complexity since you go through the indices twice and one more time to check if the string is a palindrome. 

How can we remove the for loop where you compare if the string is a palindrome. We can do it by instead, starting from the center. If so, then the only thing that we need to worry about is leftmost and rightmost indices and see if they equal. This will eliminate the third loop of checking if the entire substring is a palindrome and the solution will be O(N^2). 
