---
layout: page
tags: ['Leetcode']
title: Reverse Nodes in k-Group
---

June 4th, 2017

Today's coding challenge was ["Reverse Nodes in k-Group"](https://leetcode.com/problems/reverse-nodes-in-k-group)

According to the site, 

"Given a linked list, reverse the nodes of a linked list k at a time and return its modified list."


This I thought was similar to the previous problem of removing nth node. Because it is reflected on the last node of each kth node, when we get to kth node, we have a pointer at the beginning of the "length k cycle" and keep adding it as a next of the reflected node. Then the next in the cycle will be added as a next of the reflected node and thus can see reversed k list. After that, we can go to the next length k cycle by calling the function again with the reflected node's next pointer. We can take care  of not having full length k by the fast pointer which will signal that full length k does not exist by having a null pointer before length k is filled up. 



<!-- ```python
def reverseKGroup(self, head, k):
    """
    :type head: ListNode
    :type k: int
    :rtype: ListNode
    """
    if head == None:
        return head
    else:
        n = 1
        cur = head
        while (n < k and cur.next != None):
            cur = cur.next # the ref pt
            n += 1
        if n < k:
            return head
        else:
            nextHead = self.reverseKGroup(cur.next, k)
            new = head
            cur.next = nextHead
            while new != cur:
                temp = cur.next
                cur.next = new
                temp2 = new.next
                new.next = temp
                new = temp2
            head = cur
            return head
``` -->