---
layout: page
title: About
description: 知识改变命运
keywords: superyjcqw, 出水小葱
comments: true
menu: 关于
permalink: /about/
---

念念不忘，必有回响。

有一口气，点一盏灯。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
