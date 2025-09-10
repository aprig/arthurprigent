---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---
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

<!-- BEGIN: Publications grouped by year -->
{% assign sorted_pubs = site.publications | sort: 'date' | reverse %}
{% assign pubs_by_year = sorted_pubs | group_by_exp:"item", "item.date | date: '%Y'" %}

{% for year_group in pubs_by_year %}
  <h2>{{ year_group.name }}</h2>
  <ul>
    {% for pub in year_group.items %}
      <li>
        {{ pub.citation }}
        {% if pub.paperurl %}
          [PDF]({{ pub.paperurl }})
        {% endif %}
      </li>
    {% endfor %}
  </ul>
{% endfor %}
<!-- END: Publications grouped by year -->
