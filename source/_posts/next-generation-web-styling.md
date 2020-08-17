---
title: next-generation-web-styling
date: 2019-12-13 14:08:15
tags:
---
Closed Caption (폐쇄 자막, 자막의 표시여부를 설정할 수 있는 자) 
## 1) Scroll-snap

```css
section {
    overflow-x: auto;
    overscroll-behavior-y: contain;
    scroll-snap-type: y mandatory;
}

section > picture{
    scroll-snap-align: center;
}
```

### List with Sub-List HTML

```html
<ul>
    <li><a herf="...">One</a></li>
    <li><a herf="..." aria-haspopup="true">TWO</a>
        <ul class=""dropdown" aria-label="submenu">
            <li><a herf="...">SUB-1</a></li>
        </ul>
    </li>
</ul>
```
```css
li:hover > el,
li:focus > ul {
visibility: visible;
opacity: 1;

}
```

## 2) @media (prefers-*)
### prefers-reduced-motion
![](/images/next-generation-web-styling/prefer_1.png)
![](/images/next-generation-web-styling/prefer_2.png)
```css
@media (prefers-reduced-motion: reduce) {
    animation: cross-fade 3s ease-in-out infinite;

}
```

### prefers-color-scheme
```css
@media(prefers-color-scheme: dark) {
    --lightness: 90%;
    --text-1: hsl(200 10% var(--lightness));
}
```

## 3) Logical properties
### new mental modal 
the browser does all of that great work for you to make that website more internaltionalized.
![](/images/next-generation-web-styling/localproperty_1.png)
![](/images/next-generation-web-styling/localproperty_2.png)
![](/images/next-generation-web-styling/localproperty_3.png)
![](/images/next-generation-web-styling/localproperty_4.png)

```css
.box {  
    block-size: 300px;
    inline-size: 200px;
}

.h2, p {
    // margin-left: 3rem;
    margin-inline-start: 3rem;
}

main {
    //border-top: 2px dashed;
    border-block-start: 2px dashed;
    //margin-top: 3rem;
    margin-block-start: 3rem;
}
```
https://wit.nts-corp.com/2018/08/28/5317

## 4) Sticky situations

Classic sticky
The Sticky stack {CSS}
Ui layers overlap within the viewport

```css
dl > dt {
    position: sticky;
    top: 0;
}
```

```html
<dl>
<dt>A</dt>
...
<dt>B</dt>
</dl>
```

The sticky Desperado {HTML}

## 5) backdrop-filter
```css
element.style {
    backdrop-filter: blur(10px);
}
```

## 6) :is()
### any(), match()

```css
button:is(.focus, :focus) {
    ...
}

article > :is(h1,h2,h3,h4,h5,h6) {
    ...
}
```

## 7) grid gap gap (for Flexbox)

```css
.flex-button {
    display: inline-flex;
    gap: 1rem;
    place-items: center;
}
```

## 8) Houdini
### low-level css 

![](/images/next-generation-web-styling/houdini_1.png)
![](/images/next-generation-web-styling/houdini_2.png)


## 9) Properties & Values API
 ![](/images/next-generation-web-styling/property_value_1.png)
 ![](/images/next-generation-web-styling/property_value_2.png)
 ![](/images/next-generation-web-styling/property_value_3.png)

## 10) Typed OM
 ![](/images/next-generation-web-styling/typedOM_1.png)

## 11) Paint API
 ![](/images/next-generation-web-styling/paintedAPI_1.png)
 ![](/images/next-generation-web-styling/paintedAPI_2.png)
 ![](/images/next-generation-web-styling/paintedAPI_3.png)
 ![](/images/next-generation-web-styling/paintedAPI_4.png)


## 12) Animation Worklet
 ![](/images/next-generation-web-styling/animation_worklet_1.png)

## Speed Round
```css
.box { size: 50vw; }
iframe { aspect-ratio: 16 / 9;}
// min(),max(),clamp() 
h1 { font-size: clamp(1.5rem, 6vw);}


```

https://css-at-cds.netlify.com/