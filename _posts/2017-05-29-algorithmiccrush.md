---
layout: page
tags: ['HackerRank']
title: Algorithmic Crush
---

May 29th, 2017

My second coding challenge was ["Algorithmic Crush"](https://www.hackerrank.com/challenges/crush)


According to the site, 

"You are given a list of size N, initialized with zeroes. You have to perform M operations on the list and output the maximum of final values of all the  elements in the list. For every operation, you are given three integers a, b and k and you have to add value k to all the elements ranging from index a to b (both inclusive)."

So the first way I thought of was to go through all M operations and increment each indices one by one with k. I was pretty sure that it was going to timeout. And it did. 
<!-- ```python
N, M = input().strip().split(' ')
arr = [0]*int(N)

for _ in range(int(M)):
    a,b,k = input().strip().split(' ')
    for i in range(int(a)-1,int(b)): # it was 1-indexing
        arr[i] += int(k)
print(max(arr))
``` -->
this is O(MN) since a to b can be the whole N. 

So the next way I thought was to just store conveniently such that we can just go through N once and know what the maximum value is instead of going through the whole N M times - especially since you start with all zeros. The only information we need is the maximum sum so we just need to add from start to end of the array and see what results in the highest sum. To do that, it is important to add k and subtract k at the right positions. Since k is added for a and b inclusive, we want to subtract k at b+1 th index. So going through M operations, update the a and b+1th index with +k and -k respectively. Then, we now have to just go through len(N) array and keep adding the values of each index to see what is the highest value. *Note that we need to have len(N+1) array because you want to subtract at b+1th index and b could be the last index.*
<!-- ```python
N, M = input().strip().split(' ')
arr = [0]*(int(N)+1)

for _ in range(int(M)):
    a,b,k = input().strip().split(' ')
    arr[int(a)-1] += int(k)
    arr[int(b)] -= int(k)
bestVal = 0
curVal = 0
for i in arr:
    curVal+=i
    if curVal > bestVal:
        bestVal = curVal

print(bestVal)
``` -->
Time Complexity would be O(M+N) [since adding k and -k is constant, the first term is only M.]
Spatially, the complexity is the same as the first approach. 

