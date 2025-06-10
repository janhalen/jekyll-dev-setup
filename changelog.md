---
title: Changelog
layout: default
---

# Changelog

{% for release in site.data.changelog %}
## {{ release.name }} <small>{{ release.published_at | date: "%Y-%m-%d" }}</small>
[View on GitHub]({{ release.html_url }})

{{ release.body | markdownify }}

**Author:** [{{ release.author.login }}]({{ release.author.html_url }})

---
{% endfor %}