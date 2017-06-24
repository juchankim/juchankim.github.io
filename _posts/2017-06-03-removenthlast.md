---
layout: page
tags: ['Leetcode']
title: Remove Nth Node From End of List
---

June 3rd, 2017

Today's coding challenge was ["Remove Nth Node From End of List"](https://leetcode.com/problems/remove-nth-node-from-end-of-list)

According to the site, 

"Given a linked list, remove the nth node from the end of list and return its head."


We do not know how long the linked list is. So one way that we can solve this is to have two pointers that keep track of "n node" difference. So if n was 1, then the two pointers will be one after another. Since the last one is the 1st node from the end, when the faster pointer hits the end (cur.next becomes null), the slow pointer will be at the n+1 th node where we have access to the nth node which we have to delete. So we would need to change the next pointer to the next next one. 
One caveat is that before even doing the second loop for both fast and slow pointers, if fast is already None, then we have to remove the head hence we just return head.next (we are guaranteed to have valid n). 

<!-- ```python
def removeNthFromEnd(self, head, n):
    """
    :type head: ListNode
    :type n: int
    :rtype: ListNode
    """
    
    hare = head
    turtle = head
    while n > 0:
        hare = hare.next
        n -= 1
    if hare == None:
        return head.next
    while hare.next != None:
        hare = hare.next
        turtle = turtle.next
    turtle.next = turtle.next.next

    return head
``` -->