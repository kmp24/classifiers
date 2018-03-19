---
layout: archive
title: "Places I've Been"
permalink: /places/
author_profile: true
---
Click to view larger images!
<iframe src='https://rawgit.com/kmp24/oldsite/master/PlacesThings.html' style='height: 750px; width: 1000px'></iframe>


{% include base_path %}


{% for post in site.places %}
  {% include archive-single.html %}
{% endfor %}
