---
layout: page
title: "프로그래머스 - 할인행사"
---



https://school.programmers.co.kr/learn/courses/30/lessons/131127





## 구현방법

원하는 물건과 숫자들을 매칭시켜놓기 위해서 HashMap으로 값들을 저장시켜주고

첫날부터 연속해서 10일이 되는 날짜까지 돌리면서 값들을 새로운 HashMap에다가 저장해줍니다.

만약 중간에 원하는 물건이랑 매칭이 안될경우 그 날짜는 바로 넘어가고 다음 날부터 다시 계산해주면서 진행합니다.

연속 10일동안 필요한 물건을 가져올 경우 원하는 물건의 개수가 일치하는지 체크해서 일치할경우 answer+=1을 해주고 아닐경우 flag가 false가 되면서 넘어가게 됩니다.



## 코드



```java
package LV2;

import java.util.HashMap;

public class sale_event
{
	public int solution(String[] want, int[] number, String[] discount) {
		int answer = 0;

		HashMap<String, Integer> map = new HashMap<>();

		// HashMap으로 매칭 시켜둠.
		for (int index = 0; index < want.length; index++) {
			map.put(want[index], number[index]);
		}

		for (int index = 0; index < discount.length; index++) {
			//10일이 안될경우 break;
			if (index + 9 >= discount.length)
				break;
			
			
			if (!map.containsKey(discount[index]))
				continue;

			// 체크용 hashMap
			HashMap<String, Integer> checking_count = new HashMap<>();

			boolean checking = true;


			for (int days = index; days <= index + 9; days++) {
				// 내가 필요한 제품이 아닐경우 이날은 아니기 떄문에 끝내줌.
				if (!map.containsKey(discount[days])) {
					break;
				} else {
					checking_count.put(discount[days], checking_count.getOrDefault(discount[days], 0) + 1);
				}
			}
			if (checking_count.isEmpty())
				continue;

			// 필요한 제품의 개수가 하나라도 틀릴경우 checking = false가 됨.
			for (String apples : map.keySet()) {
				if (map.get(apples) != checking_count.get(apples)) {
					checking = false;
					break;
				}
			}
			if (checking) {
				answer += 1;
			}
		}

		return answer;
	}
}
```

