---
layout: home
title: Home
---

# Welcome to Threat Hunting Research

This blog shares threat hunting analysis, detection techniques, and security research. Here you'll find:

- **Threat Analysis** - Deep dives into malware, APTs, and attack patterns
- **Detection Engineering** - Building and refining detection rules
- **Hunting Techniques** - Proactive threat hunting methodologies
- **Tool Reviews** - Security tools and frameworks
- **Case Studies** - Real-world incident analysis

## Recent Posts

{% for post in site.posts limit:5 %}
- [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %d, %Y" }}
  {{ post.excerpt }}
{% endfor %}

---

*Stay vigilant, hunt proactively.*
