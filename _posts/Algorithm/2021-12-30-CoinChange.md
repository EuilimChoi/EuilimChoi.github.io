---
layout: single
title: "CoinChange"

excerpt: "다양한 동전들을 가지고 특정 금액을 만들 수 있는 모든 경우의 수를 리턴해야 합니다."

categories: "Algorithm"
tag: ["DP"]

sidebar:
  nav: "sidebar_main"
tagline: " "
header:
  overlay_image: /assets/images/DSCF3606.JPG
  overlay_filter: 0.5
---

## 문제

다양한 동전들을 가지고 특정 금액을 만들 수 있는 모든 경우의 수를 리턴해야 합니다.

예를 들어, 100원, 500원짜리 동전을 가지고 1,000원을 만들 수 있는 방법은 총 3가지 입니다.
100원 10개, 100원 5개 + 500원 1개, 500원 2개

## 입력

인자 1 : total
number 타입의 이하의 자연수
인자 2 : coins
number 타입을 요소로 갖는 배열
coins.length는 10,000 이하
coins[i]는 20 이하의 양의 정수

## 출력

number 타입을 리턴해야 합니다.
주의사항
동전의 금액은 다양하게 주어집니다.
coins는 오름차순으로 정렬되어 있습니다.
각 동전의 개수는 무수히 많다고 가정합니다.
입출력 예시
let total = 10;
let coins = [1, 5];
let output = coinChange(total, coins);
console.log(output); // --> 3

total = 4;
coins = [1, 2, 3];
output = coinChange(total, coins);
console.log(output); // --> 4 ([1, 1, 1, 1], [1, 1, 2], [2, 2], [1, 3])

## Advanaced

coinChange를 계산하는 효율적인 알고리즘(O(M \* N))이 존재합니다(total과 coins.length가 N, M일 경우). 쉽지 않기 때문에 바로 레퍼런스 코드를 보고 이해하는 데 집중하시기 바랍니다.

## 풀이

```javascript
const coinChange = function (total, coins) {
  // 10, [1,5] 라고 했을때
  // table[i][j]는 coins[j]까지 사용해서 i만큼의 금액을 만들 수 있는 경우의 수를 저장
  const table = [];
  for (let i = 0; i < total + 1; i++) {
    table.push(Array(coins.length).fill(0));
  }
  // 모든 경우에 0을 만들 수 있는 경우는 1 (base case)
  for (let i = 0; i < coins.length; i++) {
    table[0][i] = 1;
  }

  for (let amount = 1; amount <= total; amount++) {
    // 작은 금액부터 차례대로 경우의 수를 구한다. (bottom-up)
    for (let idx = 0; idx < coins.length; idx++) {
      let coinIncluded = 0;
      if (amount - coins[idx] >= 0) {
        coinIncluded = table[amount - coins[idx]][idx];
      }

      let coinExcluded = 0;
      if (idx >= 1) {
        // 동전을 순서대로 검사하고 있기 때문에, 바로 직전의 경우만 고려하면 된다.
        // 단, 0번째 동전은 직전이 없으므로 제외한다.
        coinExcluded = table[amount][idx - 1];
      }

      table[amount][idx] = coinIncluded + coinExcluded;
    }
  }

  return table[total][coins.length - 1];
};
```
