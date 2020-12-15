---
title: web-import-on-interaction-pattern
date: 2020-12-15 11:13:55
tags:
- web
- security
---
## The Import on interaction pattern
-  user-centric performance metrics for measuring
    - [First Input Delay : FID](https://web.dev/fid/) : within 100ms
        - how fast your site loads can be measured with [First Contentful Paint : FCP](https://web.dev/fcp/)
        - the render time of the largest image or text block visible within the viewport [Largest Contentful Paint: LCP](https://web.dev/lcp/)
    - [Total Blocking Time : TBT](https://web.dev/lighthouse-total-blocking-time/)
    - [Total to Interactive : TTI](https://web.dev/interactive/)
 
- The different ways to load resources are, at a high-level
    - Eager - load resource right away (the normal way of loading scripts)
    - Lazy (Route-based) - load when a user navigates to a route or component
    - Lazy (On interaction) - load when the user clicks UI (e.g Show Chat)
    - Lazy (In viewport) - load when the user scrolls towards the component
    - Prefetch - load prior to needed, but after critical resources are loaded
    - Preload - eagerly, with a greater level of urgency
    
    
 
