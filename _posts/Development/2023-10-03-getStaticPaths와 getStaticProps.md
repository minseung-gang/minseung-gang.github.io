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

[getStaticPaths](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-paths)는 페이지의 경로를 정적으로 생성해준다. `getStaticPaths`에서 반환된 경로들은 빌드할때 코드된 구문들을 통해 데이터를 확인한뒤, 페이지에서 각 게시물의 동적 경로를 생성한다.

```tsx
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

[getStaticProps](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props)는 경로의 데이터를 가져오기 위해 서버 측에서 실행된다.
정적 페이지를 프리렌더링하여 서버 부하를 줄일 수 있고 빠른 페이지 로딩을 가능하게 하는 용도로 사용된다고 한다.

```tsx
export const getStaticProps: GetStaticProps = async ({ params }) => {
  console.log(params);
  const stores = (await import("../public/store.json")).default;
  const store = stores.find((store) => store.name === params?.name);
  return { props: { store } };
};
```

getStaticPaths나 getStaticProps에서 반환된 데이터는 Next.js가 함수 간의 관계를 이해하여 페이지 컴포넌트의 props로 전달된다.
