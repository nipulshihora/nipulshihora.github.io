---
layout: default
title: Events
permalink: /events/
author_profile: true
---


<h2>Academic Events</h2>

<div class="filters">
  <label>Role:
    <select id="roleFilter"><option value="all">All</option></select>
  </label>
  <label>Year:
    <select id="yearFilter"><option value="all">All</option></select>
  </label>
  <label>Event Type:
    <select id="typeFilter"><option value="all">All</option></select>
  </label>
</div>

<ul id="eventList" class="event-list"></ul>

<script>
  const events = {{ site.data.events | jsonify }};

  function populateFilters(events) {
    const roleSet = new Set(), yearSet = new Set(), typeSet = new Set();
    events.forEach(e => {
      roleSet.add(e.role);
      typeSet.add(e.event_type);
      const year = e.date.split('-')[0].trim();
      yearSet.add(year);
    });

    const roleFilter = document.getElementById('roleFilter');
    roleSet.forEach(r => roleFilter.innerHTML += `<option value="${r}">${r}</option>`);

    const yearFilter = document.getElementById('yearFilter');
    [...yearSet].sort((a, b) => b - a).forEach(y => yearFilter.innerHTML += `<option value="${y}">${y}</option>`);

    const typeFilter = document.getElementById('typeFilter');
    typeSet.forEach(t => typeFilter.innerHTML += `<option value="${t}">${t}</option>`);
  }

  function displayEvents() {
    const role = document.getElementById('roleFilter').value;
    const year = document.getElementById('yearFilter').value;
    const type = document.getElementById('typeFilter').value;
    const eventList = document.getElementById('eventList');
    eventList.innerHTML = '';

    const filtered = events.filter(e => {
      const eventYear = e.date.split('-')[0].trim();
      return (role === 'all' || e.role === role) &&
             (year === 'all' || eventYear === year) &&
             (type === 'all' || e.event_type === type);
    });

    filtered.sort((a, b) => (a.date < b.date ? 1 : -1)).forEach(e => {
      const li = document.createElement('li');
      li.innerHTML = `<strong>${e.date}</strong>: ${e.event_type} - <em>${e.event_name}</em> (${e.role}${e.organizer ? ' by ' + e.organizer : ''})`;
      eventList.appendChild(li);
    });
  }

  document.addEventListener('DOMContentLoaded', () => {
    populateFilters(events);
    document.querySelectorAll('select').forEach(s => s.addEventListener('change', displayEvents));
    displayEvents();
  });
</script>

<style>
  .filters {
    margin-bottom: 20px;
  }
  select {
    padding: 4px 8px;
    margin-right: 10px;
  }
  .event-list {
    list-style-type: none;
    padding-left: 0;
  }
  .event-list li {
    padding: 6px 10px;
    background: #f5f5f5;
    border: 1px solid #ddd;
    border-radius: 5px;
    margin-bottom: 8px;
    font-size: 15px;
  }
</style>
