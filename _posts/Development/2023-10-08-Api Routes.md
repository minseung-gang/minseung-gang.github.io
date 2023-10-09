---
title: getStaticPaths와 getStaticProps에 대해서
date: 2023-10-08 11:32:00 +0800
categories: [Development, NextJs]
pin: true
tags: [Api Route]
---

Nextjs는 다른 서버가 필요 없이 특정 API 요청을 처리하기 위한 전용 서버 사이드 라우트를 쉽게 생성할 수 있는 API Routes를 지원한다.

## 사용하는 방법

Api 라우터를 사용하려면 pages폴더 하위에 있는 api 폴더 내에 파일을 만들어 사용하면 된다. `pages/api/hello.ts`

api 폴더 내에있는 파일 자체가 api의 경로가 된다.

`https://localhost:3000/api/hello`

참고로 `pages/api` 폴더에 생성된 파일 명에 `\_` 또는 `.` 접두사를 붙여 주면 API Endpoint를 만들지 않는다.

api 라우터는 기본적으로 `요청객체(req)`와 `응답객체(res)`를 매개변수로 받고 있으며, 해당 `handler함수`를 export해야한다.

```tsx
// Next.js API route support: https://nextjs.org/docs/api-routes/introduction
import type { NextApiRequest, NextApiResponse } from "next";

type Data = {
  name: string;
};

export default function handler(
  req: NextApiRequest,
  res: NextApiResponse<Data>
) {
  res.status(200).json({ name: "John Doe" });
}
```

여기서 `주의할 점`은 Api 라우터는 서버리스이기 때문에, DB와 지속적인 연결을 유지해야하거나 실시간 통신이 필요한 경우에는 사용이 적절하지 않다.

또한 `getStaticProps`나 `getServerSide`에서는 사용하지 않는 편이 좋다. 해당 함수는 빌드할때만 실행되고 클라이언트 번들에 추가되지 않기 때문이다. 그렇기 때문에 Next 공식문서에서는 db에 직접 요청하는 방식을 권고하고 있다.<br/>

## Api Route에는 어떤 장점이 있을까?

1. api 관련 로직과 나머지 Next.js 로직을 완전히 분리시킬 수 있다. url 혹은 api의 명세서가 바뀌었을 때 api route의 코드의 로직만 변경하면 된다.

2. 아래 코드와 같이 외부 서비스의 URL을 사용할 때 사용자에게는 next js서버의 도메인(/api/stores)이 보여지지 않아 백엔드 url을 보여주지않아 보안에도 도움이 된다.

```tsx
// pages/api/stores

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse<Store[]>
) {
  const stores = (await import("../../public/store.json")).default as Store[];
  res.status(200).json(stores);
}
```

3. 서버리스이기 때문에 인프라를 유지할 필요가 없으며 요청이 들어올때 마다 함수를 실행시키기 때문에 적은 비용으로 운영할 수 있다.
