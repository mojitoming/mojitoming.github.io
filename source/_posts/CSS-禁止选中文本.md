---
title: CSS 禁止选中文本
date: 2020-08-06 11:43:46
tags:
  - css
categories: WEB
---

```css
body{
    -moz-user-select:none; /*火狐*/
    -webkit-user-select:none; /*webkit浏览器*/
    -ms-user-select:none; /*IE10*/
    -khtml-user-select:none; /*早期浏览器*/
    user-select:none;
}
```

