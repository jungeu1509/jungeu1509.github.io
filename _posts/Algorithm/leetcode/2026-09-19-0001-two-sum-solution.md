---
layout: single
title:  "[LeetCode] 0001. Two Sum"
excerpt: "Two Sum 문제를 브루트포스와 HashMap(One-pass)으로 푸는 두 가지 풀이 정리. Two Sum solved with brute force and one-pass HashMap in Java"
header:
  teaser:
search: true
layout: post-single
categories: 
  - Algorithm
tags:
  - algorithm
  - codingtest
  - leetcode
  - hashmap
  - java
date:  2026-09-19 10:53:01 +0900
last_modified_at: 2026-09-23 23:46:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# Two Sum (LeetCode 1번)

- 난이도: Easy
- 링크: https://leetcode.com/problems/two-sum/description/

## 문제 설명

정수 배열 `nums`와 정수 `target`이 주어질 때, 두 수를 더해서 `target`이 되는 **두 인덱스**를 반환한다.

- 정답은 정확히 하나만 존재한다고 가정한다.
- 같은 원소를 두 번 사용할 수 없다.
- 정답의 순서는 상관없다.

### 예시

```
입력: nums = [2,7,11,15], target = 9
출력: [0,1]
설명: nums[0] + nums[1] == 9 이므로 [0, 1]을 반환한다.
```

```
입력: nums = [3,2,4], target = 6
출력: [1,2]
```

```
입력: nums = [3,3], target = 6
출력: [0,1]
```

### 제약 조건

- `2 <= nums.length <= 10^4`
- `-10^9 <= nums[i] <= 10^9`
- `-10^9 <= target <= 10^9`
- 정답은 항상 하나만 존재한다.

**심화 질문**: O(n²)보다 빠른 알고리즘으로 풀 수 있는가?

---

## 풀이 1: 브루트포스 (이중 반복문)

### 아이디어

가장 단순한 방법. 모든 두 숫자 쌍을 하나씩 다 더해보면서 `target`과 같은지 확인한다.

### Step by Step

1. 바깥쪽 반복문 `i`는 0부터 `nums.length - 1`까지 순회한다.
2. 안쪽 반복문 `j`는 `i + 1`부터 `nums.length - 1`까지 순회한다. (같은 원소를 두 번 쓰지 않기 위해 `j`는 항상 `i`보다 크게 시작)
3. `nums[i] + nums[j] == target`이면 `[i, j]`를 반환한다.

### 코드

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[] { i, j };
                }
            }
        }
        return new int[0]; // 문제 조건상 도달하지 않음
    }
}
```

### 복잡도

- 시간복잡도: **O(n²)** — 모든 쌍을 다 확인
- 공간복잡도: **O(1)** — 추가 자료구조 없음

---

## 풀이 2: HashMap (One-pass)

### 아이디어

`nums[i] + nums[j] == target`이라는 조건은 곧 `nums[j] == target - nums[i]`와 같다.
즉, 배열을 순회하면서 **"target에서 현재 숫자를 뺀 값(보수, complement)이 이전에 나온 적 있는가?"**만 확인하면 된다.

이 "이전에 나온 숫자들"을 빠르게(O(1)) 조회하기 위해 HashMap(숫자 → 인덱스)을 사용한다.

### Step by Step

1. `HashMap<Integer, Integer> map`을 만든다. (key = 숫자 값, value = 그 숫자의 인덱스)
2. 배열을 한 번만 순회한다 (`i = 0`부터 `nums.length - 1`까지).
3. 매 반복마다 `complement = target - nums[i]`를 계산한다.
4. `map`에 `complement`가 이미 들어있는지 확인한다.
   - 있으면 → `[map.get(complement), i]`를 반환한다.
   - 없으면 → `nums[i]`와 `i`를 `map`에 저장하고 다음 인덱스로 넘어간다.

### 코드

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<>(); // 숫자 -> 인덱스

        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];

            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };
            }

            map.put(nums[i], i);
        }

        return new int[0]; // 문제 조건상 도달하지 않음
    }
}
```

### 예시로 따라가기

`nums = [2, 7, 11, 15]`, `target = 9`

| i | nums[i] | complement (9 - nums[i]) | map에 있는가? | 동작 |
|---|---|---|---|---|
| 0 | 2 | 7 | 없음 | map에 `{2: 0}` 저장 |
| 1 | 7 | 2 | **있음 (2 → 0)** | `[0, 1]` 반환 |

### 복잡도

- 시간복잡도: **O(n)** — 배열을 한 번만 순회
- 공간복잡도: **O(n)** — 최악의 경우 모든 원소를 map에 저장

---

## 풀이 비교

| 방식 | 시간복잡도 | 공간복잡도 | 특징 |
|---|---|---|---|
| 브루트포스 (이중 for문) | O(n²) | O(1) | 구현은 단순하지만 느림 |
| HashMap (One-pass) | **O(n)** | O(n) | 시간을 줄이는 대신 공간을 더 씀 (시간-공간 트레이드오프) |

`nums.length`가 최대 10⁴까지 가능하므로, 실제 코딩테스트/인터뷰에서는 HashMap 풀이가 권장된다.
