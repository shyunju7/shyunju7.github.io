---
title: 이벤트 루프(Event Loop)
author: song
date: 2023-08-17 21:55:00 +0800
categories: [frontend]
tags: [이벤트루프, Javascript]
---

<img width="1299" alt="Group 20" src="https://github.com/shyunju7/shyunju7/assets/38373150/f04cf69a-7627-4883-aa02-0b0431b1829e">

오늘은 `Event Loop`에 대해 간단하게 정리해보려고 한다. 나는 [What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ) 강의를 통해서 이벤트 루프에 대해 정리해볼 수 있었다.(너무 유용한 강의였다!)

### Javascript는 싱글 스레드 언어!

`Javascript`는 싱글 스레드 언어로, 한 번에 하나의 작업만을 처리할 수 있다. (하나의 콜스택을 가지고 있기 때문에 동기적으로만 동작할 수 있다는 특징이 있다.) 하지만, `Javascript`를 사용해서 개발하다보면, 이벤트를 처리하거나 여러 개의 작업을 요청할 때도 있다. 싱글 스레드인 `Javascript`에서 어떻게 가능할까? 바로 <span class="highlight-keyword">Event Loop</span>가 Javascript에 동시성을 지원해주고 있는 것이다.

<br/>

### 동작 방식에 대해 알아보자

![eventloop](https://github.com/shyunju7/shyunju7/assets/38373150/797f5328-f0e9-41b6-8b64-c6169316be63)

Javascript에서 비동기 호출이 발생하면 해당 작업을 브라우저의 WebAPIs로 넘긴다. 그리고 동기적인 작업을 콜스택에 먼저 담아서 처리한다. 이후에 비동기 호출 작업이 끝나면 WebAPIs는 비동기 호출의 콜백함수를 콜백큐에 추가한다. 이벤트 루프는 콜스택과 콜백큐의 상태를 확인하며, 콜 스택이 비었을 때 콜백큐에 있는 콜백을 콜스택으로 밀어 올려준다.
