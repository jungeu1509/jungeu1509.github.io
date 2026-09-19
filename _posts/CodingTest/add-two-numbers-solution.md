---
layout: single
title:  "[LeetCode] Add Two Numbers"
excerpt: "연결 리스트로 표현된 두 수를 더하는 Add Two Numbers 풀이와 올림(carry) 처리 시 흔한 실수 정리. Add Two Numbers solved with linked lists and carry handling in Java"
header:
  teaser:
search: true
layout: post-single
permalink: /coding-test/leetcode/add-two-numbers/
categories: 
  - CodingTest
tags:
  - algorithm
  - codingtest
  - leetcode
  - linkedlist
  - java
date:  2026-09-19 11:57:06 +0900
last_modified_at: 2026-09-19 11:57:06 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 2. Add Two Numbers

## 문제 링크
[LeetCode - Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

## 문제 설명
두 개의 비어있지 않은 연결 리스트가 주어지며, 각 리스트는 음이 아닌 정수를 **역순**으로 저장하고 있습니다. 각 노드는 한 자리 숫자를 포함하며, 두 숫자를 더한 결과를 같은 형식의 연결 리스트로 반환해야 합니다.

**예시:**
```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
설명: 342 + 465 = 807
```

## 핵심 개념

### 1. 역순 저장
- 숫자 342는 `2 → 4 → 3` 형태로 저장됨
- 일의 자리부터 시작하므로 덧셈하기 편리함

### 2. 올림(Carry) 처리
- 두 자릿수를 더했을 때 10 이상이면 다음 자리로 올림
- 예: 7 + 8 = 15 → 현재 자리는 5, 다음 자리로 1 올림

### 3. 리스트 길이 차이
- 두 리스트의 길이가 다를 수 있음
- 짧은 리스트가 끝나면 0으로 처리

## 접근 방법

1. **더미 노드(Dummy Node) 사용**
   - 결과 리스트의 시작점을 쉽게 관리하기 위해 사용
   - 최종 결과는 `dummy.next`를 반환

2. **두 리스트 동시 순회**
   - 각 노드의 값을 읽어서 더함
   - 올림값도 함께 계산

3. **종료 조건**
   - 두 리스트 모두 끝나고 (`now1 == null && now2 == null`)
   - 올림값도 없을 때 (`carryFlag == false`)

## 주의할 점 및 흔한 실수

### ❌ 실수 1: 루프 조건 오류
```java
while (now1 != null || now2 != null || !carryFlag) {  // ❌ !carryFlag
    ...
    if (resultVal >= 10) {
        carryFlag = true;
    } else {
        carryFlag = false;  // false로 설정되면 !carryFlag가 true
    }
}
```
**문제:**
- `!carryFlag`를 사용하면 carryFlag가 false일 때 루프 계속 실행
- 무한 루프 발생 → 메모리 초과

**해결:**
```java
while (now1 != null || now2 != null || carryFlag) {  // ✅ carryFlag
```

### ❌ 실수 2: 루프 변수와 조건 변수 불일치
```java
while (l1 != null || l2 != null) {  // ❌ l1, l2 체크
    ...
    now1 = now1.next;  // now1, now2 업데이트
    now2 = now2.next;
}
```
**문제:**
- 루프 조건은 `l1`, `l2`를 보지만 실제 업데이트는 `now1`, `now2`
- `l1`, `l2`는 변하지 않아 무한 루프

**해결:**
- 조건과 업데이트 변수를 일치시킴

### ❌ 실수 3: 올림값 초기화
```java
for (int i = 0; true; i++) {
    carryFlag = false;  // ❌ 매 루프마다 초기화
    int sum = val1 + val2 + (carryFlag ? 1 : 0);
    ...
}
```
**문제:**
- 올림값을 루프 시작마다 초기화하면 이전 올림 소실

**해결:**
- 올림값은 루프 밖에서 선언하고, 필요할 때만 업데이트

## 올바른 풀이

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    boolean carryFlag = false;
    ListNode head = new ListNode(0);  // 더미 노드
    ListNode nowNode = head;

    while (l1 != null || l2 != null || carryFlag) {
        int val1 = (l1 != null) ? l1.val : 0;
        int val2 = (l2 != null) ? l2.val : 0;

        int resultVal = val1 + val2 + (carryFlag ? 1 : 0);

        // 올림 처리
        if (resultVal >= 10) {
            carryFlag = true;
            resultVal -= 10;
        } else {
            carryFlag = false;
        }

        // 새 노드 추가
        nowNode.next = new ListNode(resultVal);
        nowNode = nowNode.next;

        // 다음 노드로 이동
        if (l1 != null) l1 = l1.next;
        if (l2 != null) l2 = l2.next;
    }

    return head.next;  // 더미 노드 다음부터 반환
}
```

## 더 간결한 풀이 (정수형 carry 사용)

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    int carry = 0;

    while (l1 != null || l2 != null || carry != 0) {
        int val1 = (l1 != null) ? l1.val : 0;
        int val2 = (l2 != null) ? l2.val : 0;

        int sum = val1 + val2 + carry;
        carry = sum / 10;  // 올림: 10으로 나눈 몫

        current.next = new ListNode(sum % 10);  // 현재 자리: 10으로 나눈 나머지
        current = current.next;

        if (l1 != null) l1 = l1.next;
        if (l2 != null) l2 = l2.next;
    }

    return dummy.next;
}
```

## 복잡도 분석

- **시간 복잡도:** O(max(m, n))
  - m: l1의 길이, n: l2의 길이
  - 두 리스트 중 긴 리스트만큼 순회

- **공간 복잡도:** O(max(m, n))
  - 결과 리스트의 길이는 최대 max(m, n) + 1

## 핵심 포인트

1. **더미 노드 활용**: 결과 리스트의 시작을 쉽게 관리
2. **올림 처리**: 10으로 나눈 몫/나머지로 간단히 계산
3. **널 안전성**: 리스트 길이가 달라도 안전하게 처리
4. **루프 조건**: 두 리스트와 올림값 모두 확인
5. **변수 일관성**: 루프 조건과 업데이트 변수를 일치시키기

## 테스트 케이스

```java
// Case 1: 기본 덧셈
l1 = [2,4,3], l2 = [5,6,4]
결과 = [7,0,8]  // 342 + 465 = 807

// Case 2: 리스트 길이 다름
l1 = [9,9], l2 = [1]
결과 = [0,0,1]  // 99 + 1 = 100

// Case 3: 마지막 올림
l1 = [9,9,9], l2 = [1]
결과 = [0,0,0,1]  // 999 + 1 = 1000
```
