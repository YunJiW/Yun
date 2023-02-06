---
layout: page
title: "프로그래머스 - 야근 지수"
---


## 문제 

https://school.programmers.co.kr/learn/courses/30/lessons/12927

## 구현방법

야근을 시작한 시점에서 남은 일의 작업량을 제곱하는 거기 때문에 가장 일이 많이 남은 것들을 나열해서 각 1씩 감소시켜서 제곱의 값을 낮추는 방법으로 피로도를 최소화를 시킬 수 있습니다.

그렇기때문에 우선순위큐를 통해서 값들을 큰 수로 나열시켰으며, 맨앞의 값이 0일경우 전체적으로 일을 다 해냈다고 판단하여 야근 피로도는 0이되고 아닐경우 맨앞의 값을 꺼내서 -1을 시켜주고 우선순위큐에 다시 넣어주면서 진행하였고, 마지막에 큐의 들어있는 값들중 0이 아닌 값들의 대해서 제곱을 해서 값들을 더해서 리턴하였습니다.



## 구현알고리즘

#####  우선순위큐





## 코드



```java
import java.util.Collections;
import java.util.PriorityQueue;


public class night_shift
{
	
	public long solution(int n, int[] works) {
		long answer =0;
		
		//일의 크기가 큰순으로 정렬
		PriorityQueue<Integer> work = new PriorityQueue<>(Collections.reverseOrder());
		for(int index = 0; index< works.length;index++)
		{
			work.offer(works[index]);
		}
		
		//야근 쳐내기
		for(int index = 0; index <n;index++) {
			int work_weight = work.poll();
			if(work_weight == 0) {
				return 0;
			}
			
			work.offer(work_weight-1);
		}
		
		while(!work.isEmpty()) {
			int weight = work.poll();
			if(weight != 0) {
				answer+= Math.pow(weight, 2);
			}
		}
		
		
		return answer;
	}


}

```

