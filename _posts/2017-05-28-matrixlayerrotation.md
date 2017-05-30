---
layout: page
tags: ['HackerRank']
title: Matrix Layer Rotation
---

May 28th, 2017

My first coding challenge was ["Matrix Layer Rotation"](https://www.hackerrank.com/challenges/matrix-rotation-algo)
<img src='/assets/postpics/matrix-rotation.png'>

According to the site, 

"You are given a 2D matrix, a, of dimension MxN and a positive integer R. You have to rotate the matrix R times and print the resultant matrix. Rotation should be in anti-clockwise direction."

Seeing that the constraint is that R can go upto 10^9, I realized that naive way of looping through each rotation is very inefficient and probably will lead to timeouts. Looking closely at the image, I saw that rotations happen at each layer and despite the fact that there might be 10^9 rotations, we can eliminate the huge number first of all by using modulo of how many elements there are in each layer. We are guaranteed that the original matrix will come back every len(layer) rotation.

So the best way to do this is to loop through each layer (loop upto half of min(M and N) which is the number of layers)
grab the elements of the layer and put it into an temporary array. So the leftover from modulo is the index that will be the (0,0) of the grid. From then on, you just keep going through the temporary array and fill the layer again in a clockwise direction (come back to 0th index after reaching the end of the temporary array).

Also, I found it useful to take in note of the decreasing M and N for each layer by 2's - which was helpful in looping the layer. 

```python
M,N,R = input().strip().split(' ')
grid = []

for G in range(int(M)):
    G_t = input().strip().split(' ')
    grid.append(G_t)

for i in range(int(min(int(M),int(N))/2)): # each layer
    temp = []
    curM = int(M) - 2*i
    curN = int(N) - 2*i
    for j in range(0,curN): # get the top row
        temp.append(grid[i][i+j])
    for j in range(1, curM):
        temp.append(grid[j+i][i+curN-1])
    for j in range(1, curN):
        temp.append(grid[i+curM-1][i+curN-1-j])
    for j in range(1, curM-1):
        temp.append(grid[i+curM-1-j][i])
    index = int(R)%len(temp)
    for j in range(0, curN):
        grid[i][i+j] = temp[index]
        index = (index + 1) % len(temp)
    for j in range(1, curM):
        grid[j+i][i+curN-1] = temp [index]
        index = (index + 1) % len(temp)
    for j in range(1, curN):
        grid[i+curM-1][i+curN-1-j] = temp[index]
        index = (index + 1) % len(temp)
    for j in range(1, curM-1):
        grid[i+curM-1-j][i] = temp[index]
        index = (index +1) % len(temp)
for G in range(int(M)):
    G_t = ' '.join(grid[G])
    print(G_t)
```
By doing this, it will take O(N) where N is the number of elements in the grid. 

Reflection: Although coming up with what to do was easy, implementing took a long time as looping through each layer was little tricky with keeping track of all the variables and getting indices correct. In turns out, it was the "implementation" part that was the hard part of this challenge (looking at the tag that website put this challenge under). Anyway, by doing this first challenge, I found that it is best to do this in the morning so that I don't feel sleepy and end up not being able to keep track of how long it took. (Yes. I failed to time myself.) I still have tomorrow. :)




















