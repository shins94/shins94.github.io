Amazon online assessment problem



So the question was you have two kinds of applications foreground and background. In the array list given for both type of applications 0 element is id of the application and 1st element is the memory used by the application and K is the system memory capacity. We need to find which foreground and background applications for which the total would be either equal to K or closest to K. So for example 1 foreground ({1,2}) background {2,3} and foreground {2,4} background{1,1} would be out put as sum of the memory would be 5. For second example there is no foreground or background application for which K = 4. So the closest total would be 3 so the out put would id's of the 1st application i.e. {1,1}.

포그라운드, 백그라운드 앱의 아이디와 메모리가 Array로 주어지고. 주어진 K과 같거나 아니면 K보다 작은 값중에 제일 큰 값을 가지는 (closest to K)  포그라운드, 백그라운드의 인덱스를 어레이 리스트로 return 하는 문제이다.
못풀었다…leetcode 질문을 올렸더니 누군가 답변을 해줬음..기본적으로  min heap을 이용하여  각 포그라운드 앱 인덱스 마다





```
input :  foreground { {1,2},{2,4},{3,1},{4,5}}
            background {{1,1},{2,3},{3,2}}
            K =5
output : {{1,2},{2,1}}

input :  foreground { {1,2},{2,4}}
            background {{1,1}}
            K =4
output : {{1,1}}
```







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



