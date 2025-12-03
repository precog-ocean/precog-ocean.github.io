---
permalink: /researchteam
author_profile: false
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/squid2_copy.png
  caption: "Photo credit: [**NOAA**](https://unsplash.com)"
excerpt: ""
title: Research Team
---

{% for staff in site.researchers %}
  <h2>
    <a href="{{ staff.url }}">
    <p>
    <img src="{{ staff.impath }}"  style="float:right;width:110px;height:110px;">
    {{ staff.name }} <br>
    </p>
    </a>
    {{ staff.position }}
  </h2>
  <p></p>
{% endfor %}