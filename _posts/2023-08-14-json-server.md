---
title: Json-server를 만들어보자
author: song
date: 2023-08-12 23:10:00 +0800
categories: [frontend]
tags: [json-server]
math: true
mermaid: true
image:
  path: https://github.com/shyunju7/shyunju7/assets/38373150/8749f401-72e6-4bd6-92e8-7be8af6464c6
  alt: Responsive rendering of Chirpy theme on multiple devices.
---

최근에 `json-server`를 통해 간단하게 프로젝트를 수행했다. 간단하게 사용할 수 있는 테스트 서버 느낌이었는데, 알아두면 유용할 것 같아서 정리해두려고 한다.

### json-server란

`json-server`란, 가짜 API 서버를 만드는 툴이다. 짧은 시간동안 REST API가 필요할 때, 간단하게 만들어서 테스트해 볼 수 있다. API가 만들어지기 전에 연결 작업을 진행할 때 사용하면 유용할 것 같다.

설치와 사용방법 모두 간단하다. 먼저, npm(or yarn)을 통해 설치한다.

```zsh
npm i -g json-server
yarn global add json-server
```

먼저, 폴더를 생성하고 폴더 아래에 db.json파일을 만들었다. 파일 안에는 아래와 같이 `member`이라는 이름의 배열을 만들어서 멤버의 정보를 담고 있는 형태로 저장하였다. 이제 끝났다!

```json
{
  "member": [
    {
      "id": 1,
      "userName": "김말똥",
      "phoneNumber": "010-1111-1234",
      "position": "개발"
    },
    {
      "id": 2,
      "userName": "아무개",
      "phoneNumber": "010-2222-1234",
      "position": "디자인"
    }
  ]
}
```

아래 명령어로 실행해보자!
`json-server --watch db.json --port 3001`

그럼 다음과 같이 포트번호를 통해 내가 만든 db.json을 볼 수 있다. 한번 API 테스트를 진행해보자!
<img src="https://github.com/shyunju7/shyunju7/assets/38373150/e684b932-b0eb-4916-bb76-16ea82cf3fca" 
width="400" height="auto" alt="실행화면 이미지"/>

아래 테스트 화면처럼 db.json을 이용해서 간단하게 조회, 추가, 수정, 삭제 잡업을 실행할 수 있다. 또한, `json-server`는 페이징, 필터링, 정렬등의 기능도 지원한다.

![실행화면](https://github.com/shyunju7/shyunju7/assets/38373150/06dd98ab-2afd-45f2-bc11-90719411f7f1)

이 외에도 데이터의 수를 `_limit`을 통해 제한하거나 `_sort`와 `_order`를 사용한 간단한 정렬 기능을 설정할 수 있다.

```
/member?_limit=20
/member?_sort=id&order=DESC
/member?_sort=position&order=ASC
```

궁금한 건 공부하고 바로바로 정리해두자!

[json-server](https://github.com/typicode/json-server)
