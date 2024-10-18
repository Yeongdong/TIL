# DEEP DIVE: LRU 페이지 교체 알고리즘

* LRU 알고리즘은 1. 페이지가 캐시에 있는지를 확인하고 2. 만약 없다면 가장 참조가 오래된 페이지와 참조를 하려는 해당 페이지와 스와핑하는 알고리즘

## 로직

* 가장 오래된 것: 연결리스트의 가장 마지막 요소
* 가장 최근의 것: 연결리스트의 가장 첫번째 요소
* 페이지가 들어왔을 때 해당 페이지가 캐시에 있는지 확인하는 로직(즉, 탐색에 관한 로직)을 연결리스트로만 구현하면, 연결리스트 탐색의 시간복잡도는 O(n)이다.
* 탐색의 시간복잡도를 줄이기 위해 해시테이블을 사용한다.
    * 해시테이블의 최악의 시간복잡도는 O(n)이지만, 평균적으로 O(1)의 시간복잡도를 가진다.

```java
import java.util.HashMap;
import java.util.LinkedList;

class LRUCache {
    private int csize;    // 캐시 크기
    private LinkedList<Integer> li;   // 페이지 목록을 저장하는 리스트
    private Map<Integer, Integer> hash;   // 페이지 번호를 저장하고 리스트에서의 위치를 저장하는 해시맵

    public LRUCache(int n) {
        csize = n;
        li = new LinkedList<>();
        hash = new HashMap<>();
    }

    public void refer(int x) {
        if (!hash.containsKey(x)) {    // 페이지가 캐시에 없는 경우
            if (li.size() == csize) {  // 캐시가 가득 찬 경우
                int last = li.removeLast();   // 마지막 페이지 제거
                hash.remove(last);  // 해시맵에서도 제거
            }
        } else {
            System.out.println("해당 페이지 제거: " + li.indexOf(x) + "번째 " + x);
            li.remove((Integer) x); // 리스트에서 페이지 제거
        }
        li.addFirst(x);   // 페이지를 리스트의 맨 앞에 삽입
        hash.put(x, 0);     // 해시맵에 페이지 번호와 위치 추가(위치 정보는 필요하지 않으므로 0으로 대체)
    }

    public void display() {
        for (int page : li) {
            System.out.print(page + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        LRUCache ca = new LRUCache(3);
        int[] testPage = {1, 3, 0, 3, 5, 6, 3};

        for (int i : testPage) {
            ca.refer(i);    // 페이지 참조
            ca.display(); // 현재 캐시 상태 출력
        }
    }
}
/*
1
3 1
0 3 1
해당 페이지 제거 : 1번째 3
3 0 1
5 3 0
6 5 3
해당 페이지 제거 : 2번째 3
3 6 5
*/
```

* 참고
    * 최악, 평균시간복잡도를 반드시 말하고 이후에 무조건 최악의 시간복잡도가 나타난다면 어떤 자료구조가 대안이 있나요? 물어볼 시, 맴이 탐색에 O(logN)이 걸리기 때문에 맵을 사용한다고 얘기하는게 좋다

# DEEP DIVE : list의 add() 시간복잡도가 O(1)인 이유

* add()를 호출하게 되면 요소가 1개 추가되고 이로 인해 list의 크기가 증가하는 "비용"만 들게 됨
* 즉, 이 비용이 시간복잡도가 됨
* 배열의 크기가 매번 증가하는 거싱 아니라 배열이 다 채워져있을 때 추가하려고 하면 배열의 크기를 *2로 늘리기 때문에 결과적으로 O(1)의 결과를 가짐

# 배열과 연결리스트 차이

|                     | 배열   | 연결리스트 |
|---------------------|------|-------|
| 메모리 공간              | 연속적  | 불연속적  |
| 삽입, 삭제(맨 끝, 맨 앞 제외) | O(n) | O(1   |
| n번째 요소 참조           | O(1) | O(n)  |
| 탐색                  | O(n) | O(n)  |

* 데이터 추가, 삭제를 많이 하는 것은 연결리스트
* 참조를 많이 하는 것은 배열

# 로또 번호 7개를 셔플할 때 어떤 자료구조를 구축해야 하며 로직은 어떻게 될까?

## 로직

* 참조, swap 필요
* 7개 수 중 자유롭게 랜덤으로 2개의 수를 참조하고, 그 2개의 수를 swap하는 로직 필요

## 플로우

1. 삽입, 삭제, 탐색이 필요하지 않음
2. 로또 번호는 순차적으로 구성되어있기 때문에 이에 대한 자료구조는 배열, 연결리스트가 있음
3. 스택과 큐는 자유로이 2개를 참조해 swap을 해야하기 때문에 랜덤 접근이 불가능한 자료구조는 배제. 이때문에 연결리스트 또한 배제
4. 참조(random access)dp O(1)인 **배열**을 사용

## 코드

```java
import java.util.Arrays;
import java.util.Random;

public class ShuffleRand {
    static void randomize(int[] arr) {
        int n = arr.length;
        Random r = new Random();
        for (int i = n - 1; i > 0; i--) {
            int j = r.nextInt(i + 1);
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
        System.out.println(Arrays.toString(arr));
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5, 6, 7};
        randomize(arr);
    }
}
```
