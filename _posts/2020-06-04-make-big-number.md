---
title: 프로그래머스 큰 수 만들기
description: 프로그래머스 코딩 테스트 -> 그리디 알고리즘 -> 큰수 만들기.
categories:
 - 코딩테스트
tags: coding-test, greedy-algorithm
---
# 큰 수 만들기

### 문제 설명
어떤 숫자에서 k개의 수를 제거했을 때 얻을 수 있는 가장 큰 숫자를 구하려 합니다.

예를 들어, 숫자 1924에서 수 두 개를 제거하면 [19, 12, 14, 92, 94, 24] 를 만들 수 있습니다. 이 중 가장 큰 숫자는 94 입니다.

문자열 형식으로 숫자 number와 제거할 수의 개수 k가 solution 함수의 매개변수로 주어집니다. number에서 k 개의 수를 제거했을 때 만들 수 있는 수 중 가장 큰 숫자를 문자열 형태로 return 하도록 solution 함수를 완성하세요.

### 제한 조건
- number는 1자리 이상, 1,000,000자리 이하인 숫자입니다.
- k는 1 이상 number의 자릿수 미만인 자연수입니다.

### 입출력 예

|number | k | return|
|-------|---|-------|
|"1924"|2|"94"|
|"1231234"|3|"3234"|
|"4177252841"|4|"775841"|

## 풀이
처음 시도한 풀이 방법은
1. number 중에 뺄 숫자의 자리를 고른다.
2. k개의 자리를 모두 골랐으면
3. number에서 고르지 않는 숫자만 추출하여 results 리스트에 저장한다.
4. results 리스트중 가장 큰 수를 리턴한다.

입니다. 위의 알고리즘을 재귀를 이용한 permutation코드와 유사하게 작성하였습니다.

``` java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Solution {
    public String solution(String number, int k) {
        String answer = "";
        boolean[] selected = new boolean[number.length()];

        List<String> results = new ArrayList<>();
        permutation(0, k, number, results, selected);

        answer = Collections.max(results);
        return answer;
    }

    private void permutation(int n, int k, String number, List<String> results, boolean[] selected) {
        if (n == k) {
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < number.length(); i++) {
                if (selected[i]) continue;
                sb.append(number.charAt(i));
            }
            results.add(sb.toString());
            return;
        }
        for (int i = 0; i < number.length(); i++) {
            if (selected[i]) continue;
            selected[i] = true;
            permutation(n + 1, k, number, results, selected);
            selected[i] = false;
        }
    }

    public static void main(String[] args) {
        String number = "1924";
        int k = 2;
        System.out.println(new Solution().solution(number, k));
    }
}
```

### 결과
테스트는 모두 통과하였지만 제출하니 대부분이 시간초과가 났습니다.

제한 조건 중 숫자가 백만자리까지 가능하다는 것을 숫자가 백만까지 가능하다는 걸로 착각하여 나타난 문제점이었습니다.

## 두번째 풀이
