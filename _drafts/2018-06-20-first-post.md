---
title: Git for Deployment
date: 2018-06-20 00:00:00 +0000
layout: article
---
## Server

Initialize a bare repository outside of the live server.

```bash
mkdir -p /var/git/myProject.git
cd /var/git/myProject.git
git init --bare
```