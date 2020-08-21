---
title: css-mobile
date: 2020-08-21 22:13:48
tags:
- css
---

### ViewPort Setting
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

- width=device-width : 페이지의 너비를 기기의 스크린 너비로 설정합니다. 즉, 렌더링 영역을 기기의 뷰포트의 크기와 같게 만들어 줍니다.
- height=device-height : 페이지의 높이를 기기의 스크린 너비로 설정합니다. 즉, 렌더링 영역을 기기의 뷰포트의 크기와 같게 만들어 줍니다.
- initial-scale=1.0 : 처음 페이지 로딩시 확대/축소가 되지 않은 원래 크기를 사용하도록 합니다. 0~10 사이의 값을 가집니다.
- minimum-scale : 줄일 수 있는 최소 크기를 지정합니다. 0~10 사이의 값을 가집니다.
- maximum-scale : 늘릴 수 있는 최대 크기를 지정합니다. 0~10 사이의 값을 가집니다.
- user-scalable : yes 또는 no 값을 가지며 사용자가 화면을 확대/축소 할 수 있는지는 지정합니다.

### Safari Setting
```html
<meta name="apple-mobile-web-app-capable" content="yes" />
```
- yes면 전체화면 모드로 작동함, no면 작동하지 않음

```html
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
```
- default
- black
- black-translucent : 검정 반투명

### browser Setting
- Mobile일 경우 최상위 부모 태그에 Position: fixed를 주고 overflow: hidden을 주자