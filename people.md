---
layout: default
title: "Who"
---

## About this project

{% for p in site.data.investigators %}
  <div class="biog">
        {% if p.image %}
	  <img class="biog" src="images/people/thumbs/{{ p.image }}" />
        {% else %}
          <img class="biog" src="images/people/thumbs/unknown.png" />
        {% endif %}
        <h3>{{ p.name }}, {{ p.affil }}</h3>
        <p>{{ p.biog }}</p> 
  </div>
{% endfor %}
 

