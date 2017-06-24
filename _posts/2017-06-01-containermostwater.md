---
layout: page
tags: ['Leetcode']
title: Container with Most Water
---

June 1st, 2017

Today's coding challenge was ["Container with Most Water"](https://leetcode.com/problems/container-with-most-water/)

According to the site, 

"GGiven n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water."

So you want to find the best area within any two vertical lines. 

This one I wanted to start from the left and have two indices that keep track of best area. I realized that would mean there are so many options that have to be considered every time index is incremented. Final check would be the widest container. So I could not try this method. 

Rather, I realized that it is better to start with the widest container and go on from there. 

You have the best area initially with the widest (leftIndex = 0, rightIndex = len(pts) - 1). 

And the only way that this area can be beaten is when there is a taller line (a taller point) and it creates a new container with the existing height on the side that the index has not been incremented. So you want to keep the index that has the highest height to keep the option open when there is an index where the height is higher and can make a higher container thus a possibility of larger area. 

You will stop when the leftIndex and rightIndex cross over. 

This way the time complexity would be just going through the heights only ONCE (O(N) where N is the length of the heights array). 

<!-- 
```python
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        l = 0
        r = len(height)-1
        ht = 0
        area = 0
        while (l < r):
            ht = min(height[l], height[r])
            area = max(area, ht * (r-l))
            if (height[l] < height[r]):
                l+=1
            else:
                r-=1
        return area
```
 -->

    