---
title: adaptive-loading
date: 2019-12-11 13:45:22
tags:
---
https://youtu.be/puUPpVrIRkc

- Adaptive Loading for improving performance (slick experience)
Do we need to deliver the exact same experience to every user?
많은 웹사이트들에서 자바스크립트가 많이 사용되고 있고 자바스크립트는 single thread 기반으로 실행된다.
다양한 디바이스들의 성능은 Benchmark 를 보더라도 매년 향상되고 있지만 싱글 CPU 자체의 성능은 많이 향상되지 않았다.

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

####memory
navigator.deviceMemory

####cpu
navigator.hardwareConcurrency

####Adaptive Capability Toggling

####Adaptive Delivery with Client Hints
Enable
```html
<meta http-equiv="Accept-CH" content="Device-Memory, Viewport-Width Save-Data"/>
```
Added
Accept-CH : Device-Memory, Save-Data, Viewport-Width

slick experience <-> choppy experience

Do not just respond based on screen size, adapt based on actual device hardware.

1) Define buckets 
2) Integrate buckets into logging
3) Adapt loading based on buckets


browser 기준으로

1) Log hardwareConcurrency and deviceMemory
2) Group by hardwareConcurrency, DeviceMemory, and OS when looking at perf data
3) figure out buckets based on groupings.
 
adaptivity should be considered in your core frameworks

Think about animations

Low End <- animation 제거 
High End

Mobile Websites (Viewport-Width, CPU 등)

Load fast vs respond fast

Trade-off

```javascript
if (isLowEndDevice()) {
    scheduler.unstable_forceFrameRate(15);
}
```

