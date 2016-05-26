---
layout: default
title: "Who"
---

## About this project

{% for p in site.data.investigators %}
  <div class="biog">
        {% if p.image %}
	    <img class="biog" src="images/people/thumbs/{{ p.image }}"/>
        {% endif %}
        <h3 style="padding-bottom: 0px; margin-bottom: 0px;">{{ p.name }}, {{ p.affil }}</h3>
        <p>{{ p.biog }}</p>
  </div>
{% endfor %}
