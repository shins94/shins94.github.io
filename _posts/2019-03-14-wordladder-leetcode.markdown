---
layout: post
title: 'Word ladder'
subtitle: LeetCode problem
date: '2019-03-14 15:44:00'
background: /img/posts/03.jpg
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

아래 그림과 같이 각 단어를 노드라고 생각하고, 각 단어의 동일한 글자 위치에서 1글자를 뺐을때 동일한 단어가 되는 경우에는 
노드가 1 가중치로 연결된다고 생각하면 단어의 변경 과정을 그래프로 구성할 수 있다.

<center>
  <img class="img" src="https://leetcode.com/problems/word-ladder/Figures/127/Word_Ladder_2.png" alt="Demo Image">
</center>

위와 같이 그래프를 구성하면 **Example 1:** 은 아래와 같은 그래프로 바꿔 그릴수 있다.

<center>
<img class="img" src="https://leetcode.com/problems/word-ladder/Figures/127/Word_Ladder_1.png" alt="Demo Image">
</center>

모든 간선이 양방향이고 가중치는 1인 위의 그림 같은 그래프로 바뀌며. 이 그래프를 구성한 다음 BFS로 시작 노드부터 각 노드까지의 최단 경로를 구해주면 된다.

우선 그래프를 구성하기 위해 Map을 이용하여 각 단어가 1글자씩 "*" 로 변경 됬을 경우를 Map에 저장한다. 그리고 시작 노드부터 BFS큐에 넣고
현재 시작 단어까지의 변경 횟수(dist)를 1로 설정한다. 큐에서 front값을 pop하여 현재 노드 변수 current에 저장한다. 
current 노드(단어)에서 한 글자씩 "*"로 변경하면서 *가 포함된 단어를 key로 Map에 저장된 단어를 꺼내와서 dist값을 확인하여 방문한적이
있는 여부를 확인. 방문한적이 없으면 그 노드까지 오는 최단 경로의 길이를 현재 노드(단어)까지의 경로값에 +1값으로 설정한다. 
그리고 최단 경로 값이 업데이트 된 그 노드를 que에 push하는 식으로 BFS를 수행 모든 노드(단어)에 도달하는 최단경로의 길이를 구한다. 
최종적으로 dist[endWord]를 리턴하여 시작 단어 부터 최종 단어까지 변경하는 횟수를 구한다.

```c++

#include <bits/stdc++.h>

using namespace std;

int ladderLength(string beginWord, string endWord, vector<string>& wordList) {

    map<string,vector<string>> mp;
    map<string,int> dist;
    
    //map으로 각 단어에서 한글자씩 *인 상태로 변경하여 map["h*t"] 이런식으로 채워준다.
    for(int i = 0; i < wordList.size(); i++) {
        
        string word = wordList[i];
        
        for(int i = 0; i < word.size(); i++ ) {
        
            string temp = word;
            temp[i] = '*';

            mp[temp].push_back(word);
            
        }
        
        dist[word] = 0;
    }
    
    queue<string> q;
    q.push(beginWord);
    dist[beginWord] = 1;
    
    while(!q.empty()) {
    
        string current = q.front();
        q.pop();
        
        for(int i = 0; i < current.size(); i++ ) {
            
            string temp = current;
            temp[i] = '*';

            for(int j = 0; j < mp[temp].size(); j++) {
            
                string next = mp[temp][j];
                
                if(dist[next] == 0) {
                    dist[next] = dist[current] + 1;
                    q.push(next);
                }
            }
            
        }
    
    }
    
    return dist[endWord];
    
}


```

