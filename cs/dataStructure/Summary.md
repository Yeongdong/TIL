# 자료구조와 시간복잡도 총정리

## 배열

* 참조: O(1)
* 탐색: O(n)
* 맨 끝에 삽입/삭제: O(1)
* 맨 끝 제외 삽입/삭제: O(n)

## 연결리스트

* 참조: O(n)
* 탐색: O(n)
* 삽입/삭제: O(1)

## 스택

* n번째 참조: O(n)
* 가장 앞부분 참조: O(1)
* 탐색: O(n)
* 삽입/삭제(n번째 제외): O(1)

## 큐

* n번째 참조: O(n)
* 가장 앞부분 참조: O(1)
* 탐색: O(n)
* 삽입/삭제(n번째 제외): O(1)

## 맵

* 참조: O(logN)
* 탐색: O(logN)
* 삽입/삭제: O(logN)

## 셋

* 참조: O(logN)
* 탐색: O(logN)
* 삽입/삭제: O(logN)

## 해시테이블

* 참조: O(n)
* 탐색: O(n)
* 삽입/삭제: O(n)
* 해시테이블은 최악의 시간복잡도와 평균 시간복잡도 차이가 크며 이를 고려해서 쓸지 말지를 고민해야 함
* 평균 시간복잡도
    * 참조: O(1)
    * 탐색: O(1)
    * 삽입/삭제: O(1)

## 힙

* 참조(최대 또는 최소값을 참조): O(1)
* 탐색: O(n)
* 삽입/삭제: O(logN)