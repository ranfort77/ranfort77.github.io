---
title: 사이트 분석
last_modified_at: 2019-05-03T00:46
categories:
  - web
tags:
  - html
  - css
  - javascript
  - web
toc: true
author_profile: false
---



## _layouts/default.html

```html
<!-- _layouts/default.html -->
<html xmlns="http://www.w3.org/1999/xhtml">

<head>
{% include head.html %}
</head>

<body>
    <header>
    {% include header.html %}
    </header>
    <!-- content -->
    <div class="content">
    {{ content }}
    </div>
    <!-- footer -->
    {% include footer.html %}
</body>
</html>
```

## _includes/head.html

```html
<!-- _includes/head.html -->
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>{{ site.title }}</title>
<link href="/assets/css/style.css" rel="stylesheet" type="text/css" />
<script type="text/javascript" src="https://code.jquery.com/jquery-latest.min.js"></script>
<script type="text/javascript" src="/assets/js/board.js"></script>

{% if page.nav == "tutorial" %}
    {% include tutorial_navigation.html %}
{% endif %}
```

```html
<!-- _includes/tutorial_navigation.html -->
<script language="javascript">
	$(document).ready(function() {

	// 기존 css에서 플로팅 배너 위치(top)값을 가져와 저장한다.
	var floatPosition = parseInt($("#leftMenu").css('top'));
	// 250px 이런식으로 가져오므로 여기서 숫자만 가져온다. parseInt( 값 );
	if( isNaN(floatPosition)){
		var offsets = $('#leftMenu').offset();
		floatPosition = offsets.top;
	}

	//alert(floatPosition);

	$(window).scroll(function() {
		// 현재 스크롤 위치를 가져온다.
		var scrollTop = $(window).scrollTop();

		//alert(scrollTop);

		var newPosition = (scrollTop - 400) + floatPosition + "px";

		/* 애니메이션 없이 바로 따라감
		 $("#floatMenu").css('top', newPosition);
		 */
		
		if(scrollTop > 400){
			$("#leftMenu").stop().animate({
				"top" : newPosition
			}, 500);
		}
	}).scroll();
});
</script>

<style>
#leftMenu {
	position: absolute;
	width: 280px;
	height: 1042px;
}
</style>
```

## _includes/header.html

```html
	<div class="top_img_wp">
    	<div class="logo">
        	<h1><a href="/">{{ site.logo-title }}</a></h1>
            <span><a href="/">{{ site.logo-sub-title }}</a></span>
        </div>
    </div>
    <nav>
    	<ul>

		    {% for item in site.data.navigation.main %}
			    {% if item.img %}
			        <li><a href="{{ item.link }}"><img src="{{ item.img }}"></a></li>
				{% else %}
			        <li><a href="{{ item.link }}">{{ item.title }}</a></li>
				{% endif %}
			{% endfor %}

        </ul>
    </nav>
```

