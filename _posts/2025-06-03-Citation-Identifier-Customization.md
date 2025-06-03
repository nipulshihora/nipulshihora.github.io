---
title: 'DSpace JSPUI Enhancement – Citation Identifier Customization'
date: 2025-06-03
permalink: /posts/2025/06/blog-post-6/

tags:
  - Dspace
  - Citation Identifier
  - JSPUI Enhancement
---


## ✅ Step 1: Add Property to dspace.cfg
Open your DSpace config file `([dspace]/config/dspace.cfg)` and add a new line at the end:

```
jspui.citation.baseurl = https://dspace.iiti.ac.in
```

## ✅ Step 2: Update display-item.jsp
Edit the file:

` [dspace-src]/dspace-jspui/src/main/webapp/item/display-item.jsp `
Replace this:
```
<%-- <strong>Please use this identifier to cite or link to this item:
<code><%= HandleManager.getCanonicalForm(handle) %></code></strong>--%>
<div class="well"><fmt:message key="jsp.display-item.identifier"/>
<code><%= preferredIdentifier %></code></div>
```

to


```
<%
    String baseUrl = ConfigurationManager.getProperty("jspui.citation.baseurl");
%>
<div class="well">
    <fmt:message key="jsp.display-item.identifier"/>
    <code><%= baseUrl + "/handle/" + item.getHandle() %></code>
</div>
```
      
## ✅ Step 3: Rebuild and Deploy JSPUI
Rebuild JSPUI:

`cd [dspace-src]/dspace`

```
mvn package
```

Redeploy JSPUI to Tomcat:

`cp -r [dspace]/webapps/jspui [tomcat]/webapps/`

Restart Tomcat:

```
sudo systemctl restart tomcat
```
## ✅ Result
Your display will now show:

`https://dspace.iiti.ac.in/handle/123456789/xxxx`
