---
layout: default
title: Events
permalink: /events.html
---

# Academic Events

{% for event in site.data.events %}
<div class="event">
  <strong>{{ event.event_type }}</strong>: {{ event.event_name }} ({{ event.date }})<br>
  <em>{{ event.role }}</em> {{ event.organizer }}<br>
  {{ event.description }}
</div>
<hr>
{% endfor %}
