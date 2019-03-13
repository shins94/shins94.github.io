---
layout: post
title: 'Pair that sum equal or closest to given K from two arrays.'
subtitle: Amazon online assessment problem
date: '2019-03-04 10:44:00 -0400'
background: /img/posts/IMG_7947.JPG
categories: algorithm
published: true
---

So the question was you have two kinds of applications foreground and background. In the array list given for both type of applications 0 element is id of the application and 1st element is the memory used by the application and K is the system memory capacity. We need to find which foreground and background applications for which the total would be either equal to K or closest to K. So for example 1 foreground ({1,2}) background {2,3} and foreground {2,4} background {1,1} would be out put as sum of the memory would be 5. For second example there is no foreground or background application for which K = 4. So the closest total would be 3 so the out put would id's of the 1st application i.e. {1,1}.


```
input :  foreground { {1,2},{2,4},{3,1},{4,5}}
            background { {1,1},{2,3},{3,2} }
            K =5
output : { {1,2} , {2,1} }

input :  foreground { {1,2},{2,4}}
            background { {1,1} }
            K =4
output : { {1,1} }
```

포그라운드, 백그라운드 앱의 아이디와 메모리가 Array로 주어지고. 주어진 K과 같거나 
아니면 K보다 작은 값중에 제일 큰 값을 가지는 (closest to K) 포그라운드, 백그라운드의 
인덱스를 어레이 리스트로 return 하는 문제이다.

기본적으로 min heap을 이용하여 각 포그라운드 앱 인덱스 마다 모든 백그라운드 앱 인덱스와 
주어진 토탈 K 와 포그라운드, 백그라운드 앱의 사용 메모리의 차이를 가지고 List를 구성한다.

```
{포그라운드 index,백그라운드 index,total - (포그라운드 메모리+ 백그라운드 메모리) } 
```
이 리스트들을 Priority queue에 넣는다. 이 PQ는 아래 소스와 같이 각 리스트의 3번째 아이템의 
크기 오름차순으로 정렬되는 PQ이다. 각 리스트의 세번째 아이템은 **total - (포그라운드 메모리+ 백그라운드 메모리)**
이므로 PQ에는 사용 메모리가 토탈 값에 가까운 순으로 정렬되어 들어가 있게 된다. 
이 PQ값에서 값을 뽑아서 음수 값의 경우는 제외 하고 0일 경우 (사용 메모리가 K와 일치)를 result로 
추가하거나 0아닐 경우에는(result.size() ==0 일 경우) 제일 가까운 값이 1개만 정답이 되므로 
result에 값을 추가하고 프로그램을 종료하면 답을 구하게 된다.


```java
public List<List> closestPq(List<List> backgrounds, List<List> foregrounds, int total) {

        PriorityQueue<List<Integer>> pq = 
            new PriorityQueue<List<Integer>>((p1,p2) -> p1.get(2) - p2.get(2));
        
        for(List<Integer> forground : foregrounds) {
    		for(List<Integer> background : backgrounds) {
    			int diff = total - forground.get(1) - background.get(1);
    			List<Integer> list = new ArrayList<>();
    			list.add(forground.get(0));
    			list.add(background.get(0));
    			list.add(diff);
    			pq.add(list);
    		}
    	}
        
        List<List<Integer>> result = new ArrayList<>();
        while(!pq.isEmpty()) {
        	
        	List<Integer> list = pq.poll();
        	if(list.get(2) < 0) {
        		continue;
        	}else if(list.get(2) == 0) {
        		list.remove(2);
	        	result.add(list);
        	}else if(result.size() ==0){
        		list.remove(2);
	        	result.add(list);
	        	break;
        	}
        }
        return result;
    }
```

```c++
//
//  main.cpp
//  ApplicationMemory
//
//  Created by Sung Hyun Shin on 11/03/2019.
//  Copyright © 2019 Sung Hyun Shin. All rights reserved.
//

#include <bits/stdc++.h>
using namespace std;

struct List {
    int forg_i;
    int backg_i;
    int diff;
};

struct cmp {
    bool operator()(List a, List b) {
        return a.diff > b.diff;
    }

};


vector<vector<int>> closest(vector<vector<int>> forgrounds,vector<vector<int>> backgrounds,int K) {

    priority_queue<List,vector<List>,cmp> pq;
    
    for(int i = 0; i < forgrounds.size(); i++) {
        for(int j = 0; j < backgrounds.size(); j++) {
            
            int diff = K - forgrounds[i].at(1) - backgrounds[j].at(1);
            
            if(diff < 0)
                continue;
            
            List list;
            list.forg_i  = forgrounds[i].at(0);
            list.backg_i = backgrounds[j].at(0);
            list.diff = diff;
            pq.push(list);
        }
    }
    
    vector<vector<int>> result;
    
    while(!pq.empty()) {

        List list = pq.top();
        pq.pop();
        int value = list.diff;

        if (value == 0) {
            
            vector <int> item;
            item.push_back(list.forg_i);
            item.push_back(list.backg_i);
            result.push_back(item);
        } else if(result.size() == 0) {
            vector <int> item;
            item.push_back(list.forg_i);
            item.push_back(list.backg_i);
            result.push_back(item);
            break;
        }
    }
    
    return result;
}


int main(int argc, const char * argv[]) {
    // insert code here...
    vector<vector<int>> forgrounds;
    vector<vector<int>> backgrounds;
    
    forgrounds.push_back({1,2});
    forgrounds.push_back({2,4});
    forgrounds.push_back({3,1});
    forgrounds.push_back({4,5});
    
    backgrounds.push_back({1,1});
    backgrounds.push_back({2,3});
    backgrounds.push_back({3,2});
    
    vector<vector<int>> result = closest(forgrounds,backgrounds,5);

    return 0;
}

```
