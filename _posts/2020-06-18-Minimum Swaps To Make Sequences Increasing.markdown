---
layout: post
title: ' Minimum Swaps To Make Sequences Increasing'
subtitle: LeetCode 문제 풀이
date: '2020-06-18 15:23:00-0000'
background: /img/posts/IMG_7614-2.JPG
categories: algorithm
published: true

---



출처: https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/discuss/119879/C%2B%2BJavaPython-DP-O(N)-Solution



두개의 배열이 순차적으로 증가 하게 swap을 하는데 드는 최소 비용을 구하는 문제.
순차적으로 증가하게 만들려면 A[i-1] < A[i]가 만족하는 지 전체 배열을 확인 하면 된다.

```swap[i]``` 는 A[i] 와 B[i] 까지를 순차적으로 증가 시키는 배열로 만들기 위하여 i까지 swap한 최저 횟수를 저장한다. 
`not_swap[i]` 는 위와 같이 A[i], B[i ]를 순차적으로 증가시키는 배열로 만들기 위하여 i를 swap을 안했을 경우에 최저 swap의 횟수를 저장한다.



1. `A[i - 1] < A[i] && B[i - 1] < B[i]`.

   이 경우에는 이미 두 배열다 계속 증가 중이므로 이 상태를 유지하면 된다. 

   - i-1 번째를 swap 하지 않고 i 번째도 swap하지 않는다.`not_swap[i] = not_swap[i-1]`
   - i-1 도 swap하고 i 번째도 swap 함. 현재 swap[i]를 기존 swap[i-1]에 1을 더해줌 .
     `swap[i] = swap[i-1]+1` 

2. `A[i-1] < B[i] && B[i-1] < A[i]`

   예를 들면 

   ```
   A 3 2
   B 1 5
   ```

   의 경우 두가지의 경우로 두 배열을 증가하게 할 수 있다.

   - i-1번째를 swap하고 i번째를 swap하지 않음. `not_swap[i] = min(not_swap[i],swap[i-1])`
   - i-1번째를 swap 하지 않고 i 번째를 swap 함. `swap[i] = min(swap[i],not_swap[i-1] + 1)`



```c++
    int minSwap(vector<int>& A, vector<int>& B) {
        int N = A.size();
        int not_swap[1000] = {0};
        int swap[1000] = {1};
        for (int i = 1; i < N; ++i) {
            not_swap[i] = swap[i] = N;
            if (A[i - 1] < A[i] && B[i - 1] < B[i]) {
                not_swap[i] = not_swap[i - 1];
                swap[i] = swap[i - 1] + 1;
            }
            if (A[i - 1] < B[i] && B[i - 1] < A[i]) {
                not_swap[i] = min(not_swap[i], swap[i - 1]);
                swap[i] = min(swap[i], not_swap[i - 1] + 1);
            }
        }
        return min(swap[N - 1], not_swap[N - 1]);
    }
```




