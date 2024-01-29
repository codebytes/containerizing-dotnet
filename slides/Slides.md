---
marp: true
theme: custom-default
footer: 'https://chris-ayers.com'
---

# <!--fit-->Containerizing<br/>.NET Applications
## Chris Ayers
![bg right:50%](./img/dotnet-logo.png)

---

![bg left:40%](./img/portrait.png)

# Chris Ayers
## Senior Customer Engineer<br>Microsoft

<i class="fa-brands fa-twitter"></i> Twitter : @Chris\_L\_Ayers
<i class="fa-brands fa-mastodon"></i> Mastodon: @Chrisayers@hachyderm.io
<i class="fa-brands fa-linkedin"></i> LinkedIn: [chris\-l\-ayers](https://linkedin.com/in/chris-l-ayers/)
<i class="fa fa-window-maximize"></i> Blog: [https://chris-ayers\.com/](https://chris-ayers.com/)
<i class="fa-brands fa-github"></i> GitHub: [Codebytes](https://github.com/codebytes)

---

# Agenda

1. Introduction to Containerization
2. Microservices and Cloud-Native Applications
3. Deep Dive into .NET and Containers
4. Security in Containerized .NET Apps
5. Leveraging Tools and Technologies
6. Demonstration
7. Q&A

---

# Introduction to Containerization

- What is Containerization?
- Why is it important for .NET developers?
- Understanding Containers vs. VMs

---

# Microservices and Cloud-Native Applications

- Defining Microservices Architecture
- Benefits of Cloud-Native Applications
- How containers support Microservices and Cloud-Native Apps

---

# Deep Dive into .NET and Containers

- Containerizing a .NET App: A Step-by-Step Guide
- Understanding Docker and Kubernetes in .NET
- Best Practices for Containerized .NET Applications

---

# Dockerfile

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build-env
WORKDIR /App

# Copy everything
COPY . ./
# Restore as distinct layers
RUN dotnet restore
# Build and publish a release
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /App
COPY --from=build-env /App/out .
ENTRYPOINT ["dotnet", "DotNet.Docker.dll"]
```

---

# Security in Containerized .NET Apps

- Security Challenges in Containerization
- Best Practices for Securing .NET Containers
- Tools and Techniques for Enhanced Security

---

# Leveraging Tools and Technologies

- Essential Tools for .NET Container Deployment
- Automating and Optimizing Deployment Processes
- Overview of Kubernetes, Docker Compose, and other tools

---

# Other Tools

- https://github.com/lippertmarkus/konet
- https://github.com/dotnet/docker-tools
- https://github.com/tmds/build-image
- https://github.com/dotnet/sdk-container-builds
- https://github.com/dotnet/dotnet-docker

---

# Questions?

![bg right](img/owl.png)

---

<div class="columns">
<div>

## Resources

#### GitHub Repo
[**https://github.com/codebytes/containerizing-dotnet**](https://github.com/codebytes/containerizing-dotnet)


</div>

<div>

## Contact

<i class="fa-brands fa-twitter"></i> Twitter: @Chris\_L\_Ayers
<i class="fa-brands fa-mastodon"></i> Mastodon: @Chrisayers@hachyderm.io
<i class="fa-brands fa-linkedin"></i> LinkedIn: - [chris\-l\-ayers](https://linkedin.com/in/chris-l-ayers/)
<i class="fa fa-window-maximize"></i> Blog: [https://chris-ayers\.com/](https://chris-ayers.com/)
<i class="fa-brands fa-github"></i> GitHub: [Codebytes](https://github.com/codebytes)

</div>
</div>
