---
title: "How to run blazor app in docker container"
author: "Jerzy Wickowski"
images:
  - "images/blog/red-sharp-pruner.jpg"
url: "posts/dotnet/how-to-run-blazor-wasm-app-in-container"
date: 2023-09-02T14:42:00+01:00
draft: false
tags: ["dotnet", "docker", "container", "k8s", "kubernetes", "devops", "blazor","wasm","WebAssembly"]
categories: ["dotnet"]
type: "regular"
---

In one of my project I decided to use Blazor WebAssembly and I wanted to run it in docker, but it did't work woth default confgiuration. So I wlil describe you what is wrong with default configurationa and what you need to change to be able to run Blazor WebAssembly app with docker.

# What is wrong with default config
When I created new Blazor WebAssembly project, I have selected I want to run it with docker. It generated me a file. Here you can see it:
{{<code language="docker"  file="static/examples/CodePruner.Examples/CodePruner.Examples.BlazorWasm.Containerization/default.Dockerfile" >}}

Now you can go to the project with the docker file and build it:
```
docker build -f default.Dockerfile -t blazor-wasm-default ..
```

fine.... it should have been built. then try to run it:
```
docker run -p 8080:80 -d blazor-wasm-default
```

...and it will not work. When you check logs you will see something like this:
```
2023-09-02 08:22:05 The command could not be loaded, possibly because:
2023-09-02 08:22:05   * You intended to execute a .NET application:
2023-09-02 08:22:05       The application 'CodePruner.Examples.BlazorWasm.Containerization.dll' does not exist.
2023-09-02 08:22:05   * You intended to execute a .NET SDK command:
2023-09-02 08:22:05       No .NET SDKs were found.
2023-09-02 08:22:05 
2023-09-02 08:22:05 Download a .NET SDK:
2023-09-02 08:22:05 https://aka.ms/dotnet/download
2023-09-02 08:22:05 
2023-09-02 08:22:05 Learn about SDK resolution:
2023-09-02 08:22:05 https://aka.ms/dotnet/sdk-not-found
```

It means that there is no file like `CodePruner.Examples.BlazorWasm.Containerization.dll` and when you check the container you will be sure, it is true. 

But why? Because Blazor WebAssembly app is a static page. It doesn't need to have dotnet in container. It rather needs just to serve a static files. In that situation we can make this solution more generic.

# Deployng a Static WebApp as container (based on Blazor WebAssembly)
Two most popular tools to handle it are:
1. `apache httpd`
2. `nginx`

In our example we chose the 2nd option `nginx`. Here are steps to achieve it:
### Add config file for nginx
You need to add a config file `nginx.conf`. I suggest to add it into project. The file should looks like that:
{{<code language="docker"  file="static/examples/CodePruner.Examples/CodePruner.Examples.BlazorWasm.Containerization/nginx.conf" >}}

### Change dockerfile
Now you need to apply changes in dockerfile. There are three main changes:
1. use `nginx` as an output image
2. copy build output to `/usr/share/nginx/html` - it is the place from `ngnix` serves files
3. copy the config to `/etc/nginx/nginx.conf`

and here is a complete docker file:
{{<code language="docker"  file="static/examples/CodePruner.Examples/CodePruner.Examples.BlazorWasm.Containerization/Dockerfile" >}}

### Lets try to run it
First we need to build it with:
```
docker build -f Dockerfile -t blazor-wasm ..
```

then run 
```
docker run -p 8080:80 -d blazor-wasm
```
and go to `http://localhost:8080/` to check that everything is working as it should.

# Does it useful for you?
Let me know in comments!

Do you have any question? Let me know!
