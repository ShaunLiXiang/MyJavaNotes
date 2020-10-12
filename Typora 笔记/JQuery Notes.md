# jQuery Notes

jQuery 中文 API文档

https://jquery.cuishifeng.cn/index.html

# **一、引入资源**

```html
<script src="./jquery/jquery-3.2.1/jquery-3.2.1.js" type="text/javascript" charset="utf-8"></script>
<script src="jquery/jquery-3.2.1/jquery-3.2.1.min.js" type="text/javascript" charset="utf-8"></script>
```

# 二、选择器

## 1.**在DOM加载完成时运行的代码，可以这样写：**

```html
$(docuemnt).ready(function(){    });
```

```html
$(document).ready(function(){
  // 在这里写你的代码...
});
```

### 1.1.标签选择器

$("p")

### 1.2类选择器

$(".class")

### 1.3id选择器

$("#id")

### 1.4子标签选择器

$("ul li")

$("ul > li")

### 1.5特定标签中的id或class选择器

$("p#id")

$("p.class")

### 1.6同级标签选择器

#### 描述:找到所有与表单同辈的 input 元素

$("form ~ input")