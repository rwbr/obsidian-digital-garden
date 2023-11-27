---
title: "How to remove all Docker images at once"
date: 2019-05-21T21:29:22+02:00
tags: [docker]
---

# How to remove all Docker images at once

Sometimes it is necessary to remove all Docker images or containers on your machine. Here is how it goes. It's pretty easy.

![[container-ship.jpeg]]

## Problem

You use Docker. You have created a bunch of containers and images over time that you want to get rid of all at once.

## Solution

**Attention**: This will delete all your images or containers. It is not possible to restore them.

```bash
#!/bin/bash
# Delete all containers
docker rm $(docker ps -a -q)
# Delete all images
docker rmi $(docker images -q)
```