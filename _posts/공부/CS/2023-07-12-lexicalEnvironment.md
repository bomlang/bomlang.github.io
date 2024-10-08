---
layout: post
title: "lexical environment에 대해서"
date: 2023-07-12
categories:
  - study
  - CS
description: >
  JavaScript의 lexical environment에 대해서 학습하고 기록합니다.
---

# 렉시컬 환경과 스코프(Scope)

## 개요 - 왜 자바스크립트 개발자들은 var를 쓰지 말라고 하는가?

자바스크립트 초기에 사용되었던 `var`는 여전히 사용할 수 있지만, 많은 개발자들이 `var`의 사용을 지양하고 있습니다. 그 이유는 주로 호이스팅과 관련이 있습니다.

`var`는 함수 레벨로 호이스팅되며, 이 과정에서 변수가 `undefined`로 초기화됩니다. 이로 인해 코드의 실행 결과를 예측하기 어려울 뿐만 아니라, 오류가 발생하더라도 `undefined` 때문에 디버깅이 어려울 수 있습니다.

### 호이스팅과 렉시컬 환경

렉시컬 환경에 대해 이야기하기 전에, 호이스팅과의 관계를 이해하는 것이 중요합니다.

호이스팅은 변수와 함수 선언이 해당 스코프의 최상단으로 끌어올려지는 자바스크립트의 특성을 말합니다. 렉시컬 환경은 이 호이스팅과 밀접한 관련이 있습니다.

<br/>

## 렉시컬 환경과 스코프(Scope)

렉시컬 환경을 이해하기 위해서는 스코프에 대한 이해가 필요합니다. 자바스크립트에서 스코프는 크게 두 가지로 나뉩니다:

1. 전역 스코프(Global Scope)
2. 지역 스코프(Local Scope)

<br/>

그리고 스코프는 다음과 같은 유효 범위를 가집니다:

1. 전역(Global)
2. 함수(Function)
3. 블록(Block)

렉시컬 환경은 코드가 작성된 문맥(context)에서 결정되는 변수와 함수의 유효 범위를 가리킵니다.

자바스크립트 엔진이 코드를 실행하기 전에, 각 스코프는 렉시컬 환경을 통해 정의됩니다.

### 코드 예시와 함께 렉시컬 환경 이해하기

아래는 렉시컬 환경을 이해하기 위한 간단한 코드 예시입니다:

```js
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1
```

이 코드가 실행되면, 자바스크립트는 먼저 전역 스코프(Global Scope)를 설정합니다. 전역 스코프에서는 `var x = 1;`이 선언되어 있으며, 이는 전역 변수 x를 정의합니다. 이후 `foo()`와 `bar()` 함수가 전역 스코프에 선언됩니다.

`foo()` 함수가 호출되면, 새로운 함수 스코프(Function Scope)가 생성됩니다. 이 함수 스코프 내에서는 `var x = 10;`이 선언되지만, 이 `x`는 전역 스코프의 x와는 별개의 변수입니다.

하지만 `bar()` 함수는 전역 스코프에서 정의된 x를 참조합니다. 이는 `bar()` 함수가 정의된 위치에서의 렉시컬 환경에 의해 결정됩니다. 즉, `bar()` 함수는 전역 스코프에서 x를 찾게 됩니다.

<br/>

## 렉시컬 환경의 구조와 동작

렉시컬 환경은 크게 두 부분으로 구성됩니다:

1. 환경 레코드(Environment Record): 현재 스코프 내에 정의된 변수와 함수들의 정보를 담고 있습니다.
2. 외부 환경 참조(Outer Environment Reference): 상위(외부) 스코프를 참조하는 포인터입니다.

각 스코프는 자신만의 환경 레코드를 가지며, 외부 환경 참조를 통해 상위 스코프에 접근할 수 있습니다. 이러한 구조 덕분에 자바스크립트는 중첩된 함수 내부에서도 외부의 변수를 참조할 수 있습니다.

이 구조는 클로저(Closure)와도 밀접한 관계가 있습니다. 클로저는 함수가 외부 함수의 스코프에 접근할 수 있도록 해주는 메커니즘으로, 렉시컬 환경을 깊이 이해하는 것이 클로저를 이해하는 데 중요한 역할을 합니다.

<br/>

## var와 let, const의 차이점

`var`와 달리, `let`과 const는 블록 레벨의 스코프(Block Scope)를 따릅니다. 이는 `let`이나 `const`로 선언된 변수는 해당 블록 내에서만 유효하다는 것을 의미합니다. 이로 인해 의도치 않은 호이스팅 문제를 피할 수 있으며, 코드의 예측 가능성을 높여줍니다.

예를 들어, 아래와 같은 코드를 보겠습니다:

```js
if (true) {
  var x = 10;
}
console.log(x); // 10

if (true) {
  let y = 10;
}
console.log(y); // ReferenceError: y is not defined
```

위 코드에서 var로 선언된 `x`는 블록 외부에서 접근이 가능하지만, `let`으로 선언된 `y`는 블록 외부에서 접근할 수 없습니다. 이는 `let`과 `const`가 블록 스코프를 가지기 때문에 발생하는 차이입니다.

<br/>

## 끝내며

이 글에서는 자바스크립트에서 렉시컬 환경이 어떻게 작동하는지, 그리고 `var`와 `let`, `const`의 차이점을 통해 스코프와 호이스팅의 중요성을 살펴보았습니다.

렉시컬 환경은 자바스크립트에서 중요한 개념 중 하나로, 이를 잘 이해하는 것이 자바스크립트의 동작 방식을 깊이 있게 이해하는 데 필수적이라 생각합니다.
