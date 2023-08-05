---
layout: post
title: "Change GitLab Installation Port without Affecting GitLab Runners"
date: 2023-08-03 09:25:14
description: This post describes how to change GitLab instance's port without affecting GitLab runners.
tags: [gitlab]
comments: true
---

When you change GitLab's installation port from 80 to any other port, you need to specify the external URL to which the runners will connect.

> Note: don't use port 8080 since is already used by Puma.

# Get GitLab Up and Running
Get GitLab up an running with Docker Swarm. Here are some links that can help you:

- https://docs.gitlab.com/ee/install/docker.html
- https://geek-cookbook.funkypenguin.co.nz/recipes/gitlab/

# Change the Port and Restart GitLab
Once you have GitLab up and running on port 80, change the port in the `docker-compose.yml` file. I set the port to 8787.

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

Change the `external_url` in the file `gitlab/config/gitlab.rb`:

```bash
external_url 'http://192.168.1.50'
```

Update you GitLab service (get the services with the command `docker stack services gitlab`):

```bash
docker service update --force <service_id>
```
 
 > Note: if your runner doesn't work you might need to register it again. Use the URL http://<IP_OR_DOMAIN>:8787.


# Add the Clone URL to the GitLab Runner
Once the runner is registered to your project and working, you need to change the `clone_url` so that the runner can download the project's file.

Add the `clone_url` to the `/etc/gitlab-runner/config.toml` inside the GitLab runner.

![GitLab runner clone url](img/gitlab_runner_clone_url.png)

Exit the GitLab runner and update the GitLab runner service:

```bash
docker service update --force <service_id>
```

You're all set.