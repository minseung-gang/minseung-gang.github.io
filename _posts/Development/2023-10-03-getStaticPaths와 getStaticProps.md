---
title: getStaticPaths와 getStaticProps에 대해서
date: 2023-10-03 21:10:00 +0800
categories: [Development, NextJs]
pin: true
tags: [getStaticPaths, getStaticProps]
---

`getStaticPaths`와 `getStaticProps`는 Next.js에서 사용되는 두 가지 중요한 함수로, 정적 생성된 페이지를 만들기 위해 함께 사용된다.
<br/>

## getStaticPaths란?

[getStaticPaths](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-paths)는 페이지의 경로를 정적으로 생성해준다. `getStaticPaths`에서 반환된 경로들은 빌드할때 코드의 구문들을 통해 데이터를 확인한뒤, 페이지에서 각 게시물의 동적 경로를 생성한다.

```tsx
// [name].tsx
export const getStaticPaths: GetStaticPaths = async () => {
  const stores = (await import("../public/store.json")).default;

  const paths = stores.map((store) => ({
    params: {
      name: store.name,
    },
  }));
  return { paths, fallback: false };
};
```

참고사항으로 return을 할 때, fallback 옵션을 통해 빌드할 경로를 제한하거나 동적 경로를 생성할 때 필요한 데이터를 가져올 수 있다.

## getStaticProps란?

[getStaticProps](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props)는 페이지가 렌더링 되기 전에 원하는 데이터를 불러와야하는 경우 사용하며, 경로의 데이터를 가져오기 위해 서버 측에서 실행된다.
정적 페이지를 프리렌더링하여 서버 부하를 줄일 수 있고 빠른 페이지 로딩을 가능하게 하는 용도로 사용된다고 한다.

```tsx
// [name].tsx
export const getStaticProps: GetStaticProps = async ({ params }) => {
  console.log(params);
  const stores = (await import("../public/store.json")).default;
  const store = stores.find((store) => store.name === params?.name);
  return { props: { store } };
};
```

getStaticPaths나 getStaticProps에서 반환된 데이터는 Next.js가 함수 간의 관계를 이해하여 페이지 컴포넌트의 props로 전달된다.

---

### 궁금했던 점

전에는 보통 `getStaticProps`는 api를 사용하여 외부데이터를 가져와야할 때 사용을 했었다.
그렇기에 `getStaticProps`에 매개변수를 넣어 사용해보지는 못했었는데, 이번에 `getStaticPaths`와 함께 사용하면 `getStaticProps`의 매개변수(parameter)에는 어떤 값이 들어올까 궁금하여 확인해 보았다.

글쓴이가 접속한 url은 `http://localhost:3000/임진강수라상`이었고, getStaticProps의 매개변수는 `{ name:임진강수라상 }`이 찍혀있는걸 확인할 수 있었다.
getStaticPaths에 있는 paths 변수에는 여러개의 리스트들이 존재할 것이고 함수로 인해 여러 개의 동적 경로가 생성되었을 텐데, getStaticProps에서 받는 params에는 왜 하나의 데이터가 들어오는 걸까라는 바보같은 생각을 했었다.

당연하게도 Nextjs가 각 경로에 대해 params를 순차적으로 전달하는 것이 아니라, 동적 경로를 사용하여 라우팅할 때 요청된 URL의 params 값을 전달하기 때문이라는 것을 알 수 있었다.

`개발자가 어떤 접근 방식을 선택하느냐에 따라 프레임워크의 제어 주도권에 차이가 생길 수 있다`라는 것을 생각해 본 시간이었다.
