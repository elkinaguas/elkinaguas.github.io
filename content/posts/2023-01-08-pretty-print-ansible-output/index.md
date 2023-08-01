---
layout: post
title: "Pretty Print Ansible Output"
date: 2023-07-31 09:25:14
description: Configure ansible so that its output is prettier
tags: [ansible]
comments: true
---

In the same dir where your Ansible playbook is create a file named `ansible.cfg`.

Paste the following content in the file:
```bash
[defaults]
stdout_callback = community.general.yaml
```

You'are all set.

{{ template "_internal/disqus.html" . }}
