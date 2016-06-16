---
layout: gallery
title: "Gallery"
---

<div class="galleria">
{% for p in site.data.gallery %}
        <img src="/images/ricardo/thumbs/{{ p.file }}">

<!--        <a href="/images/ricardo/large/{{ p.file }}" border="0"><img src="/images/ricardo/thumbs/{{ p.file }}"></a> -->
{% endfor %}
</div>

Photos by and copyright <a href="http://www.photobrazil.com">Ricardo Funari</a>.

