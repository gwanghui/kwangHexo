---
title: adaptive-loading
date: 2019-12-11 13:45:22
tags:
---
- Adaptive Loading for improving performance
Do we need to deliver the exact same experience to every user?
많은 웹사이트들에서 자바스크립트가 많이 사용되고 있고 자바스크립트는 싱글 Thread기반으로 실행된다.
다양한 디바이스들의 성능은 Benchmark를 보더라도 매년 향상되고 있지만 싱글 CPU자체의 성능은 많이 향상되지 않았다.

for example 
youtube -> 514kb js download

network type change to fast 3g
youtube used low network module?

Network-aware Resource Loading
react-adaptive-hooks
adaptive-vue

19년 12월 기준 chrome Experimental

const network = navigator.connection.effectiveType;

```jsx harmony
import React from 'react';

import { useNetworkStatus } from 'react-adaptive-hooks/network';

const MyComponent = () => {
  const { effectiveConnectionType } = useNetworkStatus();

  let media;
  switch(effectiveConnectionType) {
    case '2g':
      media = <img src='medium-res.jpg'/>;
      break;
    case '3g':
      media = <img src='high-res.jpg'/>;
      break;
    case '4g':
      media = <video muted controls>...</video>;
      break;
    default:
      media = <video muted controls>...</video>;
      break;
  }

  return <div>{media}</div>;
};
```

