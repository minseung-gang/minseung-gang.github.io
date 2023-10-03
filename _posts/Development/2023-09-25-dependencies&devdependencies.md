---
title: dependencies와 devDependency의 차이
date: 2023-09-23 19:47:00 +0800
categories: [Development, Frontend]
pin: true
tags: [dependencies]
---

## dependencies란?

dependencies는 웹 어플리케이션의 동작과 관련된 내용이 있으며 후에 어플리케이션을 배포할 때 다운로드한 라이브러리가 포함된다.
`npm install 라이브러리명`으로 설치한다.

### dependencies예시

`axios`나 `lodash`와 같이 유틸리티 라이브러리로 프로그래밍 작업을 보다 간편하게 만들거나 유용한 기능을 제공하는 라이브러리를 예로 들 수 있다.

## devDependencies란?

devDependencies는 웹 어플리케이션의 동작과 상관은 없지만 말 그대로 개발을 위하여 라이브러리를 설치한다.
배포를 할때 다운로드 한 라이브러리를 사용하지 않아 빌드의 시간을 줄이고 불필요한 라이브러리를 포함시키지 않는 방법으로 사용한다.
`npm install 라이브러리명 --save-dev`나 `npm install 라이브러리명 -D`로 설치를 한다.

### devDependencies예시

`eslint`나 `prettier`와 같이 코드 컨벤션을 위한 스타일 검사기로 로컬 개발단계에서만 사용되는 라이브러리들을 예로 들 수 있다.
