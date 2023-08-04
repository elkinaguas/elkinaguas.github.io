---
layout: post
title: "Change GitLab Installation Port without Affecting GitLab Runners"
date: 2023-08-03 09:25:14
description: This post describes how to change GitLab instance's port without affecting GitLab runners.
tags: [ansible,awx]
comments: true
---

When you change GitLab's installation port from 80 to any other port, you need to specify the external URL to which the runners will connect.

> Note: don't use port 8080 since is already used by Puma.

# Get GitLab Up and Running
Get GitLab up an running with Docker Swarm. Here are some links that can help you:

- https://docs.gitlab.com/ee/install/docker.html
- https://geek-cookbook.funkypenguin.co.nz/recipes/gitlab/

# Change the Port and Restart GitLab
Once you have GitLab up and running on port 80, stop the GitLab stack before changing the port:

```bash
# Here the stack is called gitlab
docker stack rm gitlab
```

Now change the port in the `docker-compose.yml` file. I set the port to 8787.

```yaml
version: "3.6"
services:
  gitlab:
    image: gitlab/gitlab-ee:latest
    ports:
      - "2222:22"
      - "8787:80"
      - "4433:443"
    volumes:
      - ./data:/var/opt/gitlab
      - ./logs:/var/log/gitlab
      - ./config:/etc/gitlab
    shm_size: '256m'
    environment:
      GITLAB_OMNIBUS_CONFIG: "from_file('/omnibus_config.rb')"
    configs:
      - source: gitlab
        target: /omnibus_config.rb
    secrets:
      - gitlab_root_password
  gitlab-runner:
    image: gitlab/gitlab-runner:alpine
    volumes:
      - /srv/gitlab-runner/config:/etc/gitlab-runner
      - /var/run/docker.sock:/var/run/docker.sock
    deploy:
      mode: replicated
      replicas: 2
    restart: always
configs:
  gitlab:
    file: ./gitlab.rb
secrets:
  gitlab_root_password:
    file: ./root_password.txt
```

Restart your GitLab instance:

```bash
docker stack deploy --compose-file docker-compose.yml gitlab
```
