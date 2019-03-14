---
layout: post
title: 'Word ladder'
subtitle: LeetCode problem
date: '2019-03-14 15:44:00'
background: /img/posts/IMG_7947.JPG
categories: algorithm
published: true
---

Given two words (*beginWord* and *endWord*), and a dictionary's word list, find the length of shortest transformation sequence from *beginWord* to *endWord*, such that:

1. Only one letter can be changed at a time.
2. Each transformed word must exist in the word list. Note that *beginWord* is *not* a transformed word.

**Note:**

- Return 0 if there is no such transformation sequence.
- All words have the same length.
- All words contain only lowercase alphabetic characters.
- You may assume no duplicates in the word list.
- You may assume *beginWord* and *endWord* are non-empty and are not the same.

**Example 1:**

```
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```

**Example 2:**

```
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: 0

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

주어진 단어리스트 중에 한번에 1글자씩 변경이 가능할때 주어진 시작 문자 부터 끝 문자까지 변경하는 가장 짧은 횟수를
구하는 문제이다. 가장 짧은 이라는 단어에서 유추 할수있는게 최단 경로 문제라는 것. 

결국에는 각 단어를 노드라고 생각하고, 각단어의 동일한 글자 위치에서 1글자를 뺐을때 동일한 단어가 되는 경우에는 
노드가 1가중치로 연결된다고 생각하면 그래프를 구성할 수 있다.






