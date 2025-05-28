---
title: 'DSpace Permissions on Item Bitstreams'
date: 2025-05-28
permalink: /posts/2025/05/blog-post-3/

tags:
  - Dspace
  - Permissions on Item Bitstreams
  - Embargo
---


---

This document provides an overview of how **permissions and restrictions** work for **bitstreams** (attached files) in [DSpace](https://dspace.lyrasis.org/), including how to manage access using resource policies.

---

## ✅ Available Permissions

Each bitstream in DSpace can have the following permissions assigned:

| **Permission** | **Description** |
|----------------|-----------------|
| `READ`         | Allows viewing/downloading the file. |
| `WRITE`        | Allows modifying the bitstream (rarely used). |
| `DELETE`       | Allows deletion of the bitstream. |
| `ADD`          | Allows adding new bitstreams to the item. |
| `ADMIN`        | Full control: manage policies, edit, delete, etc. |

---

## 🔒 Common Access Scenarios

| **Use Case**            | **Group**          | **Permissions** |
|-------------------------|--------------------|------------------|
| Open Access             | `Anonymous`        | `READ`           |
| Registered Users Only   | `Registered Users` | `READ`           |
| Embargoed File          | None (initially)   | Add `READ` after embargo ends |
| Institutional Access    | Custom group (e.g. `IIT Indore`) | `READ` |
| Private/Restricted      | No group assigned  | No access granted |

---

## ⏳ Embargo Example

To set an embargo on a thesis PDF until **2025-12-31**:

1. Set no `READ` permission initially.
2. Use DSpace's embargo metadata (e.g., `dc.date.embargoed`).
3. Configure automatic policy update (via `embargo.submission.*` settings in `dspace.cfg`).

---

## 🛠️ Managing Permissions

### Through DSpace UI
- Go to: **Edit Item → Bitstreams → Manage Policies**
- Add/edit policies for groups and permissions.

### Using CLI
```bash
[dspace]/bin/dspace policy set --type bitstream --id [bitstream_id] --action READ --group Anonymous
