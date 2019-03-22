---
layout: post
title: 'Binary Search Tree Iterator'
subtitle: LeetCode BST 문제
date: '2019-03-22 15:31:00-0000'
background: /img/posts/times_square__new_york_by_nravemaster-d4bzwgm.jpg
categories: algorithm
published: true
---

Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling `next()` will return the next smallest number in the BST.

 



**Example:**

**![img](https://assets.leetcode.com/uploads/2018/12/25/bst-tree.png)**

```
BSTIterator iterator = new BSTIterator(root);
iterator.next();    // return 3
iterator.next();    // return 7
iterator.hasNext(); // return true
iterator.next();    // return 9
iterator.hasNext(); // return true
iterator.next();    // return 15
iterator.hasNext(); // return true
iterator.next();    // return 20
iterator.hasNext(); // return false
```

 

**Note:**

- `next()` and `hasNext()` should run in average O(1) time and uses O(*h*) memory, where *h* is the height of the tree.
- You may assume that `next()` call will always be valid, that is, there will be at least a next smallest number in the BST when `next()` is called.

이 문제는 주어진 BST에서 next함수가 호출 될때마다 BST내에서 제일 작은 값부터 순차적으로 값을 리턴하는 함수를 구현하고,
hasNext를 호출했을 경우에는 다음 최소값이 있는지 없는지를 리턴하면 된다.


