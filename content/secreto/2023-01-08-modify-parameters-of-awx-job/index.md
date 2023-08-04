---
layout: post
title: "Modify Parameters of AWX's Job Template"
date: 2023-08-03 09:25:14
description: Modify Parameters
tags: [ansible,awx]
comments: true
---

If you want to modify, let's say, the parameter `ask_limit_on_launch` to `True`, you should do this by patching through an API call:

```bash
curl -X PATCH -H "Content-Type: application/json" --user <USER>:<PASSWORD> -k https://<AWX_URL>/api/v2/job_templates/<JOB_ID>/ -d '{"ask_limit_on_launch": "true"}' | jq .
```
After doing that you can pass limit as a content when executing a job:

```bash
curl -X POST -H "Content-Type: application/json" --user <USER>:<PASSWORD> -k https://<AWX_URL>/api/v2/job_templates/<JOB_ID>/launch/ -d '{"limit": "<HOST1>,<HOST2>"}' | jq .
```

You're all set.