---
layout: tags
icon: fas fa-tags
order: 2
---
## 📂 Categories

{% assign sorted_categories = site.categories | sort %}
<ul>
  {% for category in sorted_categories %}
    <li>
      <a href="{{ site.baseurl }}/categories/{{ category[0] | slugify }}/">
        {{ category[0] }} ({{ category[1].size }})
      </a>
    </li>
  {% endfor %}
</ul>

## 🏷️ Tags

{% assign sorted_tags = site.tags | sort %}
<ul>
  {% for tag in sorted_tags %}
    <li>
      <a href="{{ site.baseurl }}/tags/{{ tag[0] | slugify }}/">
        {{ tag[0] }} ({{ tag[1].size }})
      </a>
    </li>
  {% endfor %}
</ul>