---
title: "how-to-handle-config-for-blazor-wasm-app-in-container"
author: "Jerzy Wickowski"
images:
  - "images/blog/red-sharp-pruner.jpg"
url: "posts/dotnet/how-to-handle-config-for-blazor-wasm-app-in-container"
date: 2023-09-02T14:42:00+01:00
draft: true
tags: ["dotnet", "docker", "container", "k8s", "kubernetes", "devops", "configuration"]
categories: ["dotnet"]
type: "regular"
---

- how it is working
- why env variables are not working
- use appsettings.json.development
  - for dev 
  - and local docker maybe
- override appsettings.json on START
