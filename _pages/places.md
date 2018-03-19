---
layout: archive
title: "Places I've Been"
permalink: /places/
author_profile: true
---

<iframe src='//https://rawgit.com/kmp24/oldsite/master/PlacesThings.html' style='width: 1300px;'></iframe>


{% include base_path %}


{% for post in site.places %}
  {% include archive-single.html %}
{% endfor %}
