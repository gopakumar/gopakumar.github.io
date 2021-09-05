---
title: "Reverse A Linked List"
header:
  overlay_image: /assets/images/dp.jpg
  overlay_filter: rgba(140,140,140, .6)
  caption: "Photo credit: [**PEXELS**](https://PEXELS.com)"
  actions:
    #- label: "Learn more"
    #  url: "https://unsplash.com"
categories:
  - Linked List
toc: true
toc_sticky: true

tags:
  - Python
  - LinkedList
#last_modified_at: 2018-04-23T16:00:52-04:00
---

Make it recursive, Optimze with Dict or array.

1. Write simple Reverse
2. Modify to  N node Revere
3. Call N node reverse in a loop to reverse N adj Nodes



### 1. Reverse (N Nodes) Linked List 
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        return self.reverseNAdjNodes(head,8)
    def printVals(self,head):
        while(ptr):
            print(ptr.val)
            ptr = ptr.next
    def simpleReverse(self,head):
        cur = head
        prev = None
        while (cur):
            temp = cur.next
            cur.next = prev
            prev = cur
            cur = temp
        head = prev
        return head
        
        
    def reversenNnodes(self,head,nodes):
            ptr = head
            nextptr = ptr.next
            
            cur = head
            endAfterReverse = head
            prev = None
            while (cur and nodes):
                nodes-=1
                temp = cur.next
                cur.next = prev
                prev = cur
                cur = temp
            head = prev
            endAfterReverse.next = cur
            return head

    def reverseNAdjNodes(self,head,reverseAdjNodes):    
        #reverseAdjNodes = -1
        head = self.reversenNnodes(head,reverseAdjNodes)
        temphead = head
        
        count = reverseAdjNodes-1
        while(temphead and temphead.next ):
            if count == 0:
                # TODO CAll only if more than n Nodes  left
                temphead.next = self.reversenNnodes(temphead.next,reverseAdjNodes) 
                count = reverseAdjNodes
            count -=1
            temphead = temphead.next
        return head


# Input
[1,2,3,4,5,6,7,8,9,10,11,12,13]
#Output
return self.reverseNAdjNodes(head,-1)
    [13,12,11,10,9,8,7,6,5,4,3,2,1]


return self.reverseNAdjNodes(head,2)
    [2,1,4,3,6,5,8,7,10,9,12,11,13]

return self.reverseNAdjNodes(head,7)
    [7,6,5,4,3,2,1,13,12,11,10,9,8]


```

