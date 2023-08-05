---
layout: post
title: "Install MkDocs with Material Theme in Cloudflare Pages"
date: 2023-08-04 09:25:14
description: Install MkDocs with Material Theme in Cloudflare Pages.
tags: []
comments: true
---

Install MkDocs and start a new project:
```bash
pip3 install mkdocs mkdocs-material
mkdocs new <project-name>
```

Change the theme in the `mkdocs.yml` file:
```yaml
site_name: My Docs

theme:
  name: material
```

`cd` into your project and put the MkDocs dependency in a requirements file:
```bash
cd <project-name>
pip3 freeze > requirements.txt
```

Create a new github repo on the GitHub website and push the changes in your MkDocs project repo:
```bash
git init
git remote add origin https://github.com/<your-gh-username>/<repository-name>
git add .
git commit -m "Initial commit"
git branch -M main
git push -u origin main
```

Create a new application in your cloudflare in **Workers & Pages > Create application > Pages > Connect to Git** and select your project's repository.

Go to **Environment variables (advanced) > Add variable** > and add the variable PYTHON_VERSION with a value of 3.7
