# [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

## Description

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

Example 1:

    Input:
    {"$id":"1","next":{"$id":"2","next":null,"random":{"$ref":"2"},"val":2},"random":{"$ref":"2"},"val":1}

    Explanation:
    Node 1's value is 1, both of its next and random pointer points to Node 2.
    Node 2's value is 2, its next pointer points to null and its random pointer points to itself.

Note:

- You must return the copy of the given head as a reference to the cloned list.

## Solution

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;

    Node() {}

    Node(int _val, Node* _next, Node* _random) {
        val = _val;
        next = _next;
        random = _random;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(!head) return NULL;
        Node* pNode = head;
        while(pNode) {
            Node* clone = new Node(pNode -> val, pNode -> next, NULL);
            pNode -> next = clone;
            pNode = clone -> next;
        }
        
        pNode = head;
        while(pNode) {
            Node* clone = pNode -> next;
            if(pNode -> random) {
                clone -> random = pNode -> random -> next;
            }
            pNode = clone -> next;
        }
        
        pNode = head;
        Node* cloneHead = head -> next;
        Node* cloneNode = head -> next;
        pNode -> next = cloneHead -> next;
        pNode = pNode -> next;
        while(pNode) {
            cloneNode -> next = pNode -> next;
            cloneNode = cloneNode -> next;
            pNode -> next = cloneNode -> next;
            pNode = pNode -> next;
        }
        return cloneHead;
    }
};
```

The thought is simple.

1. save one copy of a node right behind it.
2. check if the node have random pointer. If so, let its clone's random pointer points at the clone which the node's random pointer points at.
3. separate the linked list into the original one and a cloned one.

===> The solution has O(n) time complexity.