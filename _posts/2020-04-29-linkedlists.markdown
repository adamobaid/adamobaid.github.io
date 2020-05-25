---
layout: post
title:  "Linked lists implementation in Python"
date:   2020-05-08 19:26:00 -0400
categories: jekyll update
---


A singly linked List can be thought of as a collection of elements(data points) where each element points to the next one, ending with the “tail” element which points to Nothing. In doubly linked lists, elements also reference their previous neighbour.


```python


class SinglyElement:
    def __init__(self, val):
        self.val = val
        self.next = None


```

Doubly linked lists can be thought of as a two - way street while singly linked lists are one way from head to tail.


```python


class DoublyElement:
    def __init__(self, val):
        self.val = val
        self.next = None
        self.previous = None


```
# Linked Lists Basic Operations in Python

This arrangmeent allows for fast insertion and deletion operations, because only a few pointers will need to be updated to keep the same order. Unlike arrays where all indices must be updated to account for the shift caused by an add or delete operations.

On the other hand, arrays handle accessing and searching much faster, because in a linked list one have to start searching from the head of the list no matter the location of the targeted element.

To define the list itself, 3 properties are needed: head, tail and length.


```python


class LinkedList:
    def __init__(self):
        self.head = None
        self.tail = None
        self.length = 0


```

Next is to figure out how certain operations will work in the linked list. For example, to insert an element to the begining of the list, the following adjustments are needed:

The new element must be assigned as the new head of the list
the next property of the new element must point to the old head
In a doubly linked list, an extra operation is needed, which is to update the previous propertry to the old head to point to the new element.

# Inserting Elements

**To add an element to the end of the list or “push": **
- set the new element to be the new tail of the list
- set next proptery of the old tail to point to the new element
- and finally increase the length of the list by 1.

Note that there's no need to update the next property of the new element to point to None, since it is None when it was instantiated. An edge case to look out for here is if the list is completely empty(there is no head), in this case we can simply set the new element to be the head and the tail.


```python


def push(self, val):
    new_element = SinglyElement(val)
    if self.head == None:
        self.head = new_element
        self.tail = self.head
    else:
        self.tail.next = new_element
        self.tail = new_element
        self.length += 1
        return self


```

In a doubly linked list, there's an additional step of setting the previous property of the new element to point to the old tail, that is `new_element.pervious = self.tail`

**To add an element to the beginning of the list**, the process involves manipulating the head property and the pointers of the element(s). For example, in a doubly linked list:
* set the previous property of the old head to the new element
* set the head of the list to the new element
* set the next property of the new element to point to the old head
* set the previous property of the new head to None


# Removing Elements

The logic behind removing an element from the beginning of the list is similar to the above. The head of the list must now point to the new element, the new element .next will point to the old head, and the length of the list will increase by 1. In case of Dounly linked lists, there is the addtional step of updating the .previous of the old head to point to the new element too.

A big difference between singly linked lists and doubly linked lists takes places when removing elements from the end of the list. In the case of singly lists, the code must start by traversing the whole list from the head, until it reaches the tail, then perform the pointers update. On the other hand, In doubly lists, one can simply utilize the previous property of the tail
eliminating the need to traverse the whole list before being able to remove the last element.


```python
# removing an element at the end of a singly lined list


def pop(self):
    if (self.head == None):
        return None
    popped_element = self.head
    while(popped_element.next):
        new_tail = popped_element  # keeps track of tail - pre
        current = popped_element.next  # keeps track of current - curernt - post

    self.tail = new_tail
    self.tail.next = None
    self.length -= 1
    return popped_element


```


```python
# removing an element at the end of a doubly linked list


def pop(self):
    if (self.head == None):
        return None

    popped_element = self.tail
    if (self.length == 1):
        self.head = None
        self.tail = None
    else:
        self.tail = popped_element.prev
        self.tail.next = None
        popped_element.prev = None
        self.length -= 1
    return popped_element


```

# Removing or Inserting Elements at a specific position

Because there is no notion of indices in linked lists, accessing elements as straightforward as in arrays or lists. As seen in singly lists, when removing an element from the end, we need to traverse the whole list starting from the head until we reach the wanted position. In doubly lists, this can be improved a bit by utilizing the previous propertry, which in this case will allow us to start traversing from the tail or the head of the list.

Here's an example using singly lists
```python


def Insert(self, idx, val):
    # check edge cases
    if (idx < 0 or idx > self.length):
        return False
    # If the user wants to insert at the beg. > call Push
    if (idx == self.length):
        self.push(val)
    # If the user wants to insert at the beg. > call unshift
    if (idx == 0):
        self.unsift(val)

    counter = 0
    # Start at the head
    current = self.head
    # traveres until target
    while (counter != idx):
        current = current.next
        counter += 1

    # Create new element by invoking class Element
    new_element = SinglyElement(val)
    # Get the node before insertion position
    pre_node = self.get(idx - 1)
    # Store the .next of the previous element in a temporary variable
    temp = pre_node.next
    # point the .next of previous element to the new element
    pre_node.next = new_element
    # point new node's next to element on the right of the new node
    new_element.next = temp
    # increasing the length of the list by 1
    self.length += 1

    return True


```

```
