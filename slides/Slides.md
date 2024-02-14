---
marp: true
theme: custom-default
footer: 'https://chris-ayers.com'
---

# <!--fit-->Containerizing <i class="fas fa-box"></i><br/>.NET Applications
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

1. **Introduction to Containerization**
2. **Microservices and Cloud-Native Applications**
3. **Deep Dive into .NET and Containers**
4. **Security in Containerized .NET Apps**
5. **Leveraging Tools and Technologies**
6. **Demos**
7. **Q&A Session**

---

# Understanding Containerization

- Containerization encapsulates software and its dependencies within a single package
- Streamlines deployment across diverse environments
- Ensures uniform application performance.

---

# Containers vs. Virtual Machines (VMs)

<div class="columns">
<div>

Container                            
![width:400px](./img/container.png)  
- Lightweight images (MBs)             
- Fast (seconds to start)              
- Shared OS can pose security concerns 
- Efficient resource utilization       

</div>

<div>

Virtual Machines (VMs)

![width:400px](./img/vm.png) 
- Larger images (GBs)          
- Slow (OS needs full init)    
- Superior isolation           
- Higher resource use          


</div>

</div>

---

# Let's define a few Container Terms

---

# Container Images

![bg right:40%](./img/container-image.png)

Executable packages that include code, runtime, libraries, and configs. They ensure consistent application behavior across environments by encapsulating all necessary components.

---

# Diving Deeper: Image Layers

**Layered File System**
  - **Immutable**: Once created, layers are never modified.
  - **Reusable**: Layers can be shared across multiple images.

![w:1080px](./img/container-layers.drawio.png)

---

# Container Registries

<div class="columns">
<div>


Services for storing, managing, and sharing container images. They offer version and access control, facilitating secure and efficient distribution of images across development teams.


</div>
<div class="center">

![w:150px](./img/acr-logo.png)![w:150px](./img/github-packages-logo.png)
![w:150px](./img/docker-hub-logo.png)![w:150px](./img/harbor-logo.png)

</div>
</div>

---

# Container Runtimes

<div class="columns">
<div>

Software responsible for running containers and managing their lifecycle. Examples include Docker and containerd, providing the necessary environment for containers to execute.
There are high and low level runtimes.

</div>
<div class="center">

![w:200px](./img/containerd-logo.png)![w:300px](./img/crio-logo.png)![w:200px](./img/podman-logo.png)![w:300px](./img/runc-logo.png)

</div>
</div>

---

# Open Container Initiative (OCI)

![bg right 80%](./img/oci-logo.png)

A project under the Linux Foundation aiming to create open standards for container formats and runtime. It promotes interoperability and compatibility across different tools and platforms.

---

# Relationship between the Terms

<br/>

![w:1080](./img/relationship.drawio.png)

---

# Microservices Architecture

---

## Revolutionizing Software Development

- **Microservices**: Architectural style that structures an application as a collection of loosely coupled services.
- **Advantages**:
  - Improved modularity
  - Applications are easier to develop, test, deploy, and scale.
  - Each service can be deployed independently, enabling faster iterations.

---

# Cloud-Native Applications
## Leveraging the Full Potential of the Cloud

- **Cloud-Native**: Applications designed to capitalize on cloud computing frameworks.
- **Characteristics**:
  - Built and run in cloud environments.
  - Emphasize automation, scalability, and manageability.
  - Rely on containerization for deployment.

---

# Containers and Microservices: Synergy <i class="fas fa-handshake"></i>
## Enhancing Application Architecture

- **Role of Containers**: Provide a consistent and isolated environment for each microservice.
- **Impact**:
  - Simplified management and scaling of microservices.
  - Streamlined continuous integration and deployment (CI/CD) processes.

---

# Leveraging Containers for Cloud-Native Apps
## Strategies and Best Practices

- **Optimized Deployment**: Rapid provisioning and scaling.
- **Developer Productivity**: Uniform development environments.
- **Integration with Cloud Ecosystem**: Seamless compatibility with cloud services.

---

# .NET Containers

---

# Ideal .NET Container Images
> Microsoft has been providing .NET Container images for almost 10 years.
> Consistent Themes
- **Small** (faster registry pulls)
- **Secure** (non-root by default, no shell or tools)
- **Compliant** (minimal dependencies)
- **Composable** (add localizations, etc. as needed)
- **Compatible** (glibc vs musl libc)
- **Supported** (long-term support)

---

![bg fit](./img/image-sizes.png)

---

![bg fit](./img/image-general.png)

---

![bg fit](./img/chiseled.png)

---

# Dockerfiles in .NET
## The Foundation of Containerization

- **Basics**: Dockerfiles define the steps to create a container image for .NET applications.
- **Structure**: Includes base image selection, copying application files, and setting up entry points.
- **Customization**: Tailoring Dockerfiles for specific application requirements.

---

# Multi-Stage Docker Builds

- **Concept**: Multi-stage builds separate the building and running of applications into different stages.
- **Advantages**:
  - Reduced image size by excluding unnecessary build tools and files in the final image.
  - Enhanced security by minimizing the attack surface on runtime containers.

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

# Image Tagging and Management
## Organizing and Versioning

- **Tagging**: Assigns identifiable tags to Docker images, aiding in version control and organization.
- **Best Practices**:
  - Use semantic versioning or specific build identifiers.
  - Maintain clear and consistent tagging conventions for easy tracking.

---

# Configuration

---

# Configuration with Environment Variables
## Dynamic Application Settings

- **Usage**: Configure .NET applications in containers using environment variables.
- **Benefits**:
  - Externalizes configuration from the application, allowing for flexibility.
  - Facilitates easy updates and modifications without rebuilding images.

---

# Implementing CSI Secret Store
## Secure Secret Management

- **Introduction**: CSI Secret Store provides a secure way to store and manage sensitive information.
- **Integration with .NET**:
  - Seamlessly integrates with Kubernetes, enhancing the security of .NET containerized applications.
  - Automates the injection of secrets into containers at runtime.

---

# Security

---

# Security in Containerized .NET Apps
## Safeguarding Your Applications

- **Challenges**: Identifying and addressing security risks in containerized environments.
- **Security Measures**: Implementing best practices for securing .NET containers.
- **Essential Tools**: Leveraging advanced tools for security enhancement.

---

# Container Scanning in .NET
## Ensuring Security and Compliance

- **Purpose**: Identifies security vulnerabilities and compliance issues in container images.
- **Tools and Practices**:
  - Utilize tools like Clair, Trivy, or Docker Scan.
  - Regularly scan images during development and before deployment.

---

# Leveraging Tools and Technologies
## Empowering Your Containerization Journey

- **Essential Tools**: Overview of critical tools for container management and deployment.
- **Automation**: Strategies for automating deployment and management processes.
- **Technology Overview**: Exploring Kubernetes, Docker Compose, and other pivotal technologies.

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
