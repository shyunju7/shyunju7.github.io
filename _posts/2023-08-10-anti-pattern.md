---
title: 안티패턴(anti-pattern)
author: song
date: 2023-08-10 22:33:00 +0800
categories: [dev]
tags: [안티패턴]
math: true
mermaid: true
image:
  path: https://github.com/shyunju7/shyunju7.github.io/assets/38373150/1a18552b-f802-47db-8c1c-36c35deefcc9
  alt: Responsive rendering of Chirpy theme on multiple devices.
---

오늘의 주제는 `안티패턴`이다. <br/>
안티패턴이 무엇인지 간단하게 정리하고, 개발하면서 자주 사용한 안티패턴 종류와 개선 방법에 대해 정리해보려고 한다. <br/>
(주로 `javascript`를 사용한다.)

<br/>

### 안티패턴이란?

`안티패턴`은 실제 많이 사용되는 패턴이지만 비효율적이거나 비생산적인 패턴을 의미한다. 안티패턴은 성능, 디버깅, 유지보수, 가독성등에 있어 부정적인 영향을 줄 수 있기 때문에 지양해야하는 패턴이다.

---

### 1. 중괄호(`{}`)를 생략하지 말자

평소에 `if/while/for`문 등을 사용할 때, 아래와 같이 코드를 작성해왔다. 코드가 짧아 깔끔해 보인다고 생각했다.

```javascript
// Bad
for (let i = 0; i < N; i++) answer[i] = i;

// Bad
if (answer !== 0) count++;
```

한 줄짜리 제어문이더라도 중괄호`{}`를 생략하지 않는 것이 좋다고 한다. 중괄호`{}`를 생략하게 되면 코드의 구조를 애매하게 하고 가독성을 해칠 수 있다고 한다.

---

### 2. `parseInt`의 두 번째 파라미터인 기수를 생략하지 말자

`javascript`에서 `parseInt`함수는 문자열을 파싱하여 특정 진수의 정수로 반환해준다. 이 때, 두 번쨰 인자로 기수를 보내는데, 기수를 생략할 경우 브라우저에서 자체적으로 변환하여 숫자 형식을 판단하게 된다. 때문에 반환할 기수를 인자로 보내 올바른 결과를 만들어야 한다. (10진수로 변환하기 위해 사용한다면 `Number()`와 `+`를 사용하는 것이 좋다.)

```javascript
let str = "51";
let num = parseInt(str, 2); // 2진수로 변환
num = Number(str); // 10진수로 변환
num = +str; // 10진수로 변환
```

---

### 3. 객체 순회시 `for in`을 사용하지 말자

`for in`은 프로토타입 체인에 있는 모든 프로퍼티를 순회하기 때문에 기본 for문을 사용할 때보다 느리게 동작한다. 아래 코드의 배열의 합을 구하기 위해 for문, map, for of문, for in문을 사용하였고, 걸린 시간을 측정하였다.

```javascript
let array = Array(10000000)
  .fill()
  .map((_, index) => index);
let length = array.length;
let sum = 0;

// 1. for문
for (let idx = 0; idx < length; idx++) {
  sum += array[idx];
}

// 2. for of문
for (let num of array) {
  sum += num;
}

// 3. for in문
for (let idx in array) {
  sum += array[idx];
}
```

실행 결과를 확인하면 다른 반복문에 비하여 `for in`문의 속도가 느린 것을 확인할 수 있다.

<img width="927" alt="실행결과 이미지" src="https://github.com/shyunju7/shyunju7.github.io/assets/38373150/a1bd703e-0419-4526-aacf-afc950574820">

---

### 4. 배열 순회 시 `array.length` 속성을 참조하지 말자

아래 코드와 같이 배열을 탐색할 때, `array.length`를 사용하여 반복문에 범위를 설정하였다. 하지만, 이러한 방법은 불필요한 연산을 반복하기 때문에 지양해야한다.

```javascript

// Bad
const numbers = Array.from({length: 1000}).fill(0);
for(let n = 0; n < numbers.length; n++) { ... }
```

---

### 마무리

정리한 내용 외에도 많은 안티 패턴이 존재한다. 아래 링크를 방문하면 더 많은 안티 패턴 종류와 개선 방법을 학습할 수 있다.
블로그에 정리된 내용 외에 더 많은 안티 패턴 사례와 개선 방법은 아래 링크를 통해서 확인할 수 있다.

[Toast UI 안티 패턴](https://ui.toast.com/fe-guide/ko_ANTI-PATTERN#%EB%B0%B0%EC%97%B4-%EC%88%9C%ED%9A%8C-%EC%8B%9C-%EB%A7%A4%EB%B2%88-arraylength%EC%86%8D%EC%84%B1%EC%9D%84-%EC%B0%B8%EC%A1%B0%ED%95%98%EC%A7%80-%EC%95%8A%EB%8A%94%EB%8B%A4-legacy) <br/>
[no-plusplus - ESLint - Pluggable JavaScript Linter](https://eslint.org/docs/latest/rules/no-plusplus)
