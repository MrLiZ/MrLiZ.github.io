---
title: HTML+CSS常见问题
date: 2017-07-25 13:18:38
categories:
- 前端-html+css
tags:
- 前端
- html
- css
---

### html中显示所有空格

html中默认不管多少空格都是只显示一个，不过添加以下css属性之后可以让所有空格都显示：

```
white-space: pre !important;
```

### 修改placeholder颜色

修改html输入框中placeholder的字体颜色可以通过以下样式：

```
input::-webkit-input-placeholder { /* WebKit browsers */
  color: $color-font;
}
input:-moz-placeholder { /* Mozilla Firefox 4 to 18 */
  color: $color-font;
}
input::-moz-placeholder { /* Mozilla Firefox 19+ */
  color: $color-font;
}
input:-ms-input-placeholder { /* Internet Explorer 10+ */
  color: $color-font;
}
```



