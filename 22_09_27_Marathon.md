# Programmers 

### 완주하지 못한 선수

1. https://school.programmers.co.kr/learn/courses/30/parts/12077
2. 사용 언어 : java
----

1. 문제 설명
> + 참여한 선수들의 이름이 담긴 배열 participant N명    
> + 완주한 선수들의 이름이 담긴 배열 completion N-1명   
> + 완주하지 못한 선수를 구하는 것이 문제!(이름이 같은 선수 존재할 수 있음)

2. List를 이용한 풀이
```java
import java.util.ArrayList;
import java.util.Arrays;

//테스트케이스는 통과하지만 효율성 테스트는 통과하지 못함
class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";
        ArrayList result = new ArrayList(Arrays.asList(completion));
        for(int i = 0; i < participant.length; i++) {
            String name = participant[i];
            if(result.contains(name)) {
                result.remove(name);
                answer = answer + 'o';
            }
            else
                answer = answer + 'x';
        }
        answer = participant[answer.indexOf('x')];
        return answer;
    }
}
 ```
 + completion 배열을 리스트 = result 로 바꾼다. (원소를 제거하기 위해)
 + participant 배열을 돌면서 완주 목록에 있으면 answer에 'o' 표시를 하고 완주목록에서 제거
 + answer에서 'x'가 있는 인덱스의 참가자 이름 반환
 + 모든 테스트케이스는 통과했지만 제한시간 내 통과하지 못함
3. Collection을 이용한 풀이
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";
        List<String> list = new ArrayList<String>();
        Collections.addAll(list, participant);
        Collections.addAll(list, completion);
        
        //풀이 = participant 와 completion 를 합쳐서 개수가 홀수인 것을 출력
        for(int i = 0; i < participant.length; i++) {
            if(Collections.frequency(list,participant[i]) % 2 != 0) {
                answer =  participant[i];
                break;
            }
        }
        return answer;
    }
}
```
+ participant와 completion를 모두 합쳐 하나의 리스트로 만든다.
+ 각 원소의 개수를 구해 홀수인 원소의 이름을 반환한다.
+ 이 방법 역시 모든 테스트케이스는 통과하지만 제한시간 내 통과하지 못함
4. HashMap을 이용한 풀이
```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

//문제 유형이 HashMap이길래 사용해보았다.
class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";
        HashMap<String, Integer> map = new HashMap<>();
        //참가자 수만큼 더해주고
        for(int i = 0; i < participant.length; i++){
            map.put(participant[i], map.getOrDefault(participant[i], 0) + 1);
        }
        //완주한 수 만큼 빼준다.
        for(int i = 0; i < completion.length; i++){
            map.put(completion[i], map.getOrDefault(completion[i], 0) - 1);
        }

        //value 값이 0이 아닌 참가자의 이름을 찾아내면 된다.
        Iterator<Map.Entry<String, Integer>> itr = map.entrySet().iterator();
        while(itr.hasNext()) {
            Map.Entry<String, Integer> entry = itr.next();
            if (entry.getValue() != 0){
                answer = entry.getKey();
                break;
            }
        }
        return answer;
    }
}
```
+ [참고] https://coding-grandpa.tistory.com/entry/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EC%99%84%EC%A3%BC%ED%95%98%EC%A7%80-%EB%AA%BB%ED%95%9C-%EC%84%A0%EC%88%98-%ED%95%B4%EC%8B%9C-Lv-1
+ HashMap은 자주을 안해봐서 가장 나중에 적용해보았다.
+ 나는 반복문에서 가장 많은 시간이 걸릴 것으로 생각해 반복문을 가장 적게 사용하는 방법을 생각했었다.
+ 하지만 생각해보니 list에서 내가 원하는 값을 구하는 것도 구현을 안했을 뿐 시간이 걸린다.
+ 그렇게 생각하니 당연히 HashMap이 빠를 수 밖에 없다는 것을 알았다.
+ HashMap 문법은 공부해봐야겠다.
