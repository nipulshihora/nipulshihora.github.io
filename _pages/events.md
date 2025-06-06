---
layout: default
title: ""
permalink: /events/
---


<h2>Academic Events</h2>

<div class="filters" style="display: flex; flex-wrap: wrap; gap: 15px; margin-bottom: 20px; font-family: Arial, sans-serif;">
  <label><strong>Role:</strong>
    <select id="roleFilter"><option value="all">All</option></select>
  </label>
  <label><strong>Year:</strong>
    <select id="yearFilter"><option value="all">All</option></select>
  </label>
  <label><strong>Event Type:</strong>
    <select id="typeFilter"><option value="all">All</option></select>
  </label>
</div>

<ul id="eventList" class="event-list"></ul>

<script>
// Embedded professional events
const events = [
  {
    "event_type": "Session",
    "event_name": "Weekly Academic & Office Practice Sessions",
    "date": "2025",
    "role": "Participant",
    "organizer": "IIT Indore"
  },
  {
    "event_type": "Webinar",
    "event_name": "Online Awareness Session – How to Publish with Oxford Journals",
    "date": "2025",
    "role": "Participant",
    "organizer": "IIT Indore"
  },
  {
    "event_type": "Webinar",
    "event_name": "Turnitin Webinar on Paper Deletion and Repository Selection",
    "date": "2025",
    "role": "Participant",
    "organizer": ""
  },
  {
    "event_type": "Session",
    "event_name": "User Awareness Session on CMIE Database",
    "date": "2024",
    "role": "Participant",
    "organizer": "IIT Indore"
  },
  {
    "event_type": "Webinar",
    "event_name": "Unlocking the Secrets of Research: Reproducibility and Data Management",
    "date": "2024",
    "role": "Participant",
    "organizer": "Special Library Association (SLA)"
  },
  {
    "event_type": "Webinar",
    "event_name": "Enhancing Research with Open Data",
    "date": "2024",
    "role": "Participant",
    "organizer": "Online"
  },
  {
    "event_type": "Workshop",
    "event_name": "Author Workshop by IOP Publishing (UK)",
    "date": "2024",
    "role": "Participant",
    "organizer": "IIT Indore"
  },
  {
    "event_type": "Webinar",
    "event_name": "Mastering Data Management Plans",
    "date": "2024",
    "role": "Participant",
    "organizer": ""
  },
  {
    "event_type": "Training",
    "event_name": "Training Session – Procurement of Goods and Services on GeM Portal",
    "date": "2024",
    "role": "Participant",
    "organizer": "IIT Indore"
  },
  {
    "event_type": "Session",
    "event_name": "User Awareness Session on MLA International Bibliography",
    "date": "2024",
    "role": "Participant",
    "organizer": "IIT Indore"
  },
  {
    "event_type": "Session",
    "event_name": "Training Session on CMIE Prowess Database",
    "date": "2024",
    "role": "Participant",
    "organizer": ""
  },
  {
    "event_type": "Workshop",
    "event_name": "Summer School on Building Next-Gen Digital Repositories (DSpace)",
    "date": "2024-08-05",
    "role": "Participant",
    "organizer": "IIT Delhi"
  },
  {
    "event_type": "Workshop",
    "event_name": "Awareness Programme on ORCID and INFLIBNET Services",
    "date": "2024",
    "role": "Participant",
    "organizer": "Devi Ahilya Vishwavidyalaya, Indore & INFLIBNET Centre, Gandhinagar"
  },
  {
    "event_type": "Training",
    "event_name": "FLY Training Program",
    "date": "2023-12-02",
    "role": "Participant",
    "organizer": "IIT Indore"
  },
  {
    "event_type": "Session",
    "event_name": "EPWRF India Time Series Database Awareness Session",
    "date": "2023",
    "role": "Participant",
    "organizer": "Online"
  },
  {
    "event_type": "Session",
    "event_name": "Turnitin Feedback Studio Awareness Session",
    "date": "2023",
    "role": "Participant",
    "organizer": "Online"
  },
  {
    "event_type": "Training",
    "event_name": "Knimbus Admin Training",
    "date": "2023",
    "role": "Participant",
    "organizer": "IIT Indore"
  },
  {
    "event_type": "Webinar",
    "event_name": "LIB-TAN Monthly Talk Series – 2",
    "date": "2022",
    "role": "Resource Person",
    "organizer": "Online"
  },
  {
    "event_type": "Training",
    "event_name": "In-house Training for RFID System",
    "date": "2022",
    "role": "Participant",
    "organizer": "IIT Indore"
  },
  {
    "event_type": "Training",
    "event_name": "Training of Officials of Autonomous Bodies, Institutions, etc.",
    "date": "2022",
    "role": "Participant",
    "organizer": ""
  },
  {
    "event_type": "Session",
    "event_name": "In-house Skill Enhancement Training Session",
    "date": "2021",
    "role": "Participant",
    "organizer": "IIT Indore"
  },
  {
    "event_type": "Workshop",
    "event_name": "One-day Workshop on Implementing Successful Library & Information Teaching Session in Academic Libraries",
    "date": "2018",
    "role": "Participant",
    "organizer": "IIT Gandhinagar"
  },
  {
    "event_type": "Session",
    "event_name": "Librarian Day Seminar 2018 on Startup India and Role of Libraries",
    "date": "2018",
    "role": "Participant",
    "organizer": "MICA Ahmedabad"
  },
  {
    "event_type": "Workshop",
    "event_name": "KOHA: Installation and Operations",
    "date": "2018",
    "role": "Technical Resource Person",
    "organizer": "INFLIBNET Centre Gandhinagar"
  },
  {
    "event_type": "Workshop",
    "event_name": "SOUL 2.0 Advance Training Program",
    "date": "2017",
    "role": "Technical Resource Person",
    "organizer": "INFLIBNET Centre Gandhinagar"
  },
  {
    "event_type": "Workshop",
    "event_name": "E-Resource Management – CORAL ERMS Software",
    "date": "2017",
    "role": "Technical Resource Person",
    "organizer": "INFLIBNET Centre Gandhinagar"
  },
  {
    "event_type": "Training",
    "event_name": "PDP: Design and Development of Institutional Repository Using DSpace",
    "date": "2017",
    "role": "Resource Person",
    "organizer": "UGC-Human Resource Development Centre, Sardar Patel University, Vallabh Vidyanagar"
  },
  {
    "event_type": "Workshop",
    "event_name": "Five-Day National Workshop on Capacity Building Program on ICT for LIS (Hindi)",
    "date": "2017",
    "role": "Technical Resource Person",
    "organizer": "INFLIBNET Centre Gandhinagar"
  },
  {
    "event_type": "Seminar",
    "event_name": "Re-imagining Today’s Librarianship",
    "date": "2017",
    "role": "Organizing Committee Member",
    "organizer": "Collaboration of ADINET, Ahmedabad & Adani Institute of Infrastructure, Ahmedabad"
  },
  {
    "event_type": "Workshop",
    "event_name": "Two-day Workshop on Developing Institutional Repository Using DSpace",
    "date": "2016",
    "role": "Participant",
    "organizer": "NIFT Bhubaneswar"
  },
  {
    "event_type": "Seminar",
    "event_name": "Librarian’s Development Programme (LDP)",
    "date": "2015",
    "role": "Participant",
    "organizer": "KIIT University, Bhubaneswar"
  },
  {
    "event_type": "Seminar",
    "event_name": "Librarian’s Day Seminar 2012",
    "date": "2012",
    "role": "Participant",
    "organizer": "ADINET, Ahmedabad"
  },
  {
    "event_type": "Seminar",
    "event_name": "Social Responsibility of LIS Professionals",
    "date": "2011",
    "role": "Participant",
    "organizer": "Alumni Association of Dept. of Library & Information Science, S. P. University, Vallabh Vidyanagar"
  }
];

function populateFilters(events) {
  const roles = new Set(), years = new Set(), types = new Set();
  events.forEach(e => {
    roles.add(e.role);
    types.add(e.event_type);
    const year = e.date.split('-')[0];
    years.add(year);
  });

  roles.forEach(r => roleFilter.innerHTML += `<option value="${r}">${r}</option>`);
  [...years].sort((a, b) => b - a).forEach(y => yearFilter.innerHTML += `<option value="${y}">${y}</option>`);
  types.forEach(t => typeFilter.innerHTML += `<option value="${t}">${t}</option>`);
}

function displayEvents() {
  const role = roleFilter.value;
  const year = yearFilter.value;
  const type = typeFilter.value;
  const list = document.getElementById("eventList");
  list.innerHTML = '';

  const filtered = events.filter(e => {
    const eventYear = e.date.split('-')[0];
    return (role === 'all' || e.role === role) &&
           (year === 'all' || eventYear === year) &&
           (type === 'all' || e.event_type === type);
  });

  filtered.sort((a, b) => b.date.localeCompare(a.date)).forEach(e => {
    const li = document.createElement('li');
    li.innerHTML = `
      <span class="date">${e.date}</span> — 
      <span class="type">${e.event_type}</span>: 
      <span class="name"><strong>${e.event_name}</strong></span> 
      <span class="meta">(${e.role}${e.organizer ? ', ' + e.organizer : ''})</span>
    `;
    list.appendChild(li);
  });
}

document.addEventListener('DOMContentLoaded', () => {
  populateFilters(events);
  document.querySelectorAll('select').forEach(el => el.addEventListener('change', displayEvents));
  displayEvents();
});
</script>

<style>
  .event-list {
    list-style: none;
    padding: 0;
    font-family: 'Segoe UI', sans-serif;
  }
  .event-list li {
    padding: 10px 15px;
    margin-bottom: 8px;
    background: #f9f9f9;
    border-left: 4px solid #007acc;
    border-radius: 4px;
    box-shadow: 0 1px 3px rgba(0,0,0,0.08);
    font-size: 15px;
  }
  .event-list li .date {
    color: #333;
    font-weight: bold;
    margin-right: 8px;
  }
  .event-list li .type {
    color: #006699;
    font-style: italic;
  }
  .event-list li .name {
    color: #111;
  }
  .event-list li .meta {
    color: #555;
    margin-left: 6px;
    font-style: normal;
  }
</style>
