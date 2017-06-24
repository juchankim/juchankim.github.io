---
layout: page
tags: ['Leetcode']
title: Trapping Rain Water
---

June 2nd, 2017

Today's coding challenge was ["Trapping Rain Water"](https://leetcode.com/problems/trapping-rain-water)

According to the site, 

"Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining."
Example is as below:

<img src='/assets/postpics/rainwatertrap.png'>

The first approach I thought was to just go through from left to right. So keep the best elevation and whenever there is a lower elevation, the water can get trapped. 

However, this will not work in a case there is no height on the right that is equivalent to best elevation of the left - since to trap water, you need at least an equivalent elevation or higher elevation. 

Then, the next approach will be go both directions - left to right and right to left. Only increment/decrement the index and add the elevation difference of that side only when the elevation of the opposite end is higher. This ensures that the water will be trapped since we know that the elevation is as high as the best one on the side that we are changing index. By doing so, we will meet somewhere in the middle of the index and we stop and the left and right index are about to cross each other over. 
<!-- 
```python
def trap(self, height):
    """
    :type height: List[int]
    :rtype: int
    """
    left = 0
    right = len(height)-1
    res = 0
    bestLeft = 0
    bestRight = 0
    while (left < right):
        if (height[left]<height[right]):
            if (bestLeft < height[left]):
                bestLeft = height[left]
            else:
                res += (bestLeft - height[left])
            left+=1
        else:
            if (bestRight < height[right]):
                bestRight = height[right]
            else:
                res += bestRight - height[right]
            right-=1
    return res
```
 -->

    