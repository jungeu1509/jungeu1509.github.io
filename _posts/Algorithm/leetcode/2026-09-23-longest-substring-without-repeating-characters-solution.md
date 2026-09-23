---
layout: single
title:  "[LeetCode] 0003. Longest Substring Without Repeating Characters"
excerpt: "슬라이딩 윈도우로 반복 문자가 없는 최장 부분 문자열을 구하는 풀이와 O(n²) 브루트포스의 문제점 정리. Longest Substring Without Repeating Characters solved with a sliding window in Java"
header:
  teaser:
search: true
layout: post-single
permalink: /coding-test/leetcode/longest-substring-without-repeating-characters/
categories: 
  - Algorithm
tags:
  - algorithm
  - codingtest
  - leetcode
  - slidingwindow
  - java
date:  2026-09-23 21:34:34 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 3. Longest Substring Without Repeating Characters

## 문제 링크
[LeetCode - Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## 문제 설명
문자열 `s`가 주어졌을 때, 반복되는 문자가 없는 가장 긴 부분 문자열(substring)의 길이를 구합니다.

**예시:**
```
Input: s = "abcabcbb"
Output: 3
설명: 정답은 "abc"이며, 길이는 3입니다.
```

## 핵심 개념

### 1. 슬라이딩 윈도우 (Sliding Window)
- `windowStart` ~ `windowEnd` 두 포인터로 "반복 문자가 없는 구간"을 표현
- `windowEnd`는 항상 오른쪽으로 한 칸씩만 전진 (되돌아가지 않음)
- 중복 문자를 만나면 `windowStart`만 앞으로 점프시켜 윈도우를 좁힘

### 2. "마지막으로 본 위치" 테이블
- `Map<Character, Integer>`에 각 문자가 **가장 최근에 등장한 인덱스**만 저장
- 중복 여부는 `containsKey`로, 그 위치가 현재 윈도우 안인지(`>= windowStart`)까지 같이 확인해야 함
  - 윈도우 밖에서 봤던 문자는 "중복"으로 치면 안 됨 (흔한 함정)

### 3. 한 번의 순회로 끝내기
- 모든 시작점에서 다시 훑지 않고, 문자열 전체를 **딱 한 번만** 순회

## 접근 방법 (최종 O(n) 풀이)

1. `lastSeen` 맵과 `windowStart = 0`, `maxLength = 0` 초기화
2. `windowEnd`를 0부터 `s.length() - 1`까지 순회하며
   - 현재 문자가 `lastSeen`에 있고, 그 위치가 `windowStart` 이상이면 → `windowStart`를 그 위치 + 1로 이동
   - `lastSeen`에 현재 문자의 최신 위치 갱신
   - `maxLength = max(maxLength, windowEnd - windowStart + 1)`
3. `maxLength` 반환

## 처음에 시도했던 방식과 문제점

### ❌ 시도 1: 컴파일은 되지만 느린 브루트포스 (O(n²))
```java
public int lengthOfLongestSubstring(String s) {
int result = 0;
for (int i = 0; i < s.length(); i++) {
    HashMap<String,Integer> map = new HashMap<>();
    map.put(String.valueOf(s.charAt(i)), i);
    for (int j = i + 1; j < s.length(); j++) {
        if (map.containsKey(String.valueOf(s.charAt(j)))) {
            break;
        }
        map.put(String.valueOf(s.charAt(j)),j);
    }
    result = Math.max(result, map.size());
}
return result;
}
```
**문제:**
- 모든 시작 인덱스 `i`마다 안쪽 루프를 다시 돌기 때문에 최악의 경우(반복 문자가 거의 없는 긴 문자열) `O(n²)`
- `i`마다 `HashMap`을 새로 만들어 GC 부담(할당/해제) 추가
- 이미 지나온 구간을 계속 재탐색 — 한 번 확인한 정보를 버리고 다시 계산

**해결:**
- `HashMap` 하나를 계속 재사용하면서 "마지막으로 본 위치"만 기억 → 윈도우 시작점을 앞으로만 이동 (아래 "올바른 풀이" 참고)

## 올바른 풀이

```java
public int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> lastSeen = new HashMap<>();
    int maxLength = 0;
    int windowStart = 0;

    for (int windowEnd = 0; windowEnd < s.length(); windowEnd++) {
        char c = s.charAt(windowEnd);
        if (lastSeen.containsKey(c) && lastSeen.get(c) >= windowStart) {
            windowStart = lastSeen.get(c) + 1;
        }
        lastSeen.put(c, windowEnd);
        maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
    }

    return maxLength;
}
```

## 복잡도 분석

- **시간 복잡도:** O(n)
  - `windowEnd`가 문자열을 한 번만 순회, `windowStart`도 총 n번 이하로만 이동
- **공간 복잡도:** O(min(n, Σ))
  - `Σ`는 사용 가능한 문자 집합 크기 (예: ASCII 128) — 맵에는 현재 윈도우 안의 서로 다른 문자만 저장됨

## 핵심 포인트

1. **슬라이딩 윈도우**: 포인터를 되돌리지 않고 한 방향으로만 이동시켜야 O(n)이 됨
2. **"윈도우 안에 있는지" 체크 필수**: 단순히 `containsKey`만 보면 윈도우 밖의 과거 위치까지 중복으로 오인함
3. **HashMap 실제 API 확인**: `push`/`contain`이 아니라 `put`/`containsKey` (`get`으로 값 조회)
4. **자료구조 재사용**: 루프마다 새 컬렉션을 만들지 말고 하나를 유지하며 갱신하면 성능과 GC 부담 모두 개선됨

## 테스트 케이스

```java
// Case 1: 기본 케이스
s = "abcabcbb"
결과 = 3  // "abc"

// Case 2: 모두 같은 문자
s = "bbbbb"
결과 = 1  // "b"

// Case 3: 윈도우 밖 문자는 중복 아님
s = "pwwkew"
결과 = 3  // "wke"

// Case 4: 빈 문자열
s = ""
결과 = 0

// Case 5: 윈도우 재설정 확인
s = "abba"
결과 = 2  // "ab" 또는 "ba"
```
