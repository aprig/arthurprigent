---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if author.googlescholar %}
You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% assign sorted_pubs = site.publications | sort: 'date' %}  <!-- oldest first -->
{% assign pubs_by_year = sorted_pubs | group_by_exp:"item", "item.date | date: '%Y'" %}

<style>
  .publication-year {
    margin-top: 2em;
    font-size: 1.5em;
    border-bottom: 2px solid #ccc;
    padding-bottom: 0.2em;
  }
  .publication-card {
    padding: 0.5em 1em;
    margin: 0.5em 0;
    border-left: 4px solid #007acc;
    background-color: #f9f9f9;
    border-radius: 5px;
  }
  .publication-card a {
    text-decoration: none;
    color: #007acc;
    font-weight: bold;
  }
</style>

{% assign counter = 0 %}  <!-- Initialize counter -->

{% for year_group in pubs_by_year %}
  <div class="publication-year">{{ year_group.name }}</div>
  {% for pub in year_group.items %}
    {% assign counter = counter | plus: 1 %}
    <div class="publication-card">
      <strong>{{ counter }}.</strong>
      {% assign citation = pub.citation | replace: "Prigent", "<strong>Prigent</strong>" %}
      {{ citation | markdownify }}
      {% if pub.paperurl %}
        &nbsp;[PDF]({{ pub.paperurl }})
      {% endif %}
    </div>
  {% endfor %}
{% endfor %}
