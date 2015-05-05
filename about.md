---
layout: page
title: About us
permalink: /about/
banner_image: bowtux.jpg
banner_image_alt: About us
---

Our development team was established in 2012 as part of the the [Devision of Social Sciences IT Service Center][dssit]. We are tasked with providing quality software tools for our academic personnel to better streamline their business processes using secure and modern development techniques.

Our team is composed of three members:

<div>
{% for author in site.authors %}
  <div class="person-info">
  <div class="person">
    <a href="https://github.com/{{ author[1].github }}">
      <img
        src="http://www.gravatar.com/avatar/{{ author[1].gravatar }}?s=100"
        alt="{{ author[1].name }}" width="100" height="100" class="img-circle" />
    </a>
    <div class="person-desc">
      <h5>
        <a href="https://github.com/{{ author[1].github }}">
          {{ author[1].name }}
        </a>
      </h5>
      <h6>{{ author[1].title }}</h6>
      <h6>{{ author[1].email }}</h6>
    </div>
  </div>
  </div>
{% endfor %}
</div>
---

{% include social.html %}

[dssit]: http://it.dss.ucdavis.edu/
