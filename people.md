---
layout: default
title: "Who"
---

## About this project

{% for p in site.data.investigators %}
  <div class="biog">
	<img class="biog" src="images/people/thumbs/{{ p.image }}" />
        <h2>{{ p.name }}</h2>
        <h3>{{ p.affil }}</h3>
        <p>{{ p.biog }}</p> 
  </div>
{% endfor %}
 

