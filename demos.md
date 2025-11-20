
ACR_NAME=cacontainersdemo.azurecr.io
az group create --name rg-containers-demos --location eastus
az acr create --name cacontainersdemo -g rg-containers-demos -l eastus --sku Standard
az acr login -n cacontainersdemo

---

cd src/SampleApi
cd src/hello-containers

---

docker build . -t sampleapi:latest
docker run -it --rm -p 8080:8080 sampleapi:latest

--- size

docker run -it --rm --entrypoint /bin/bash sampleapi:latest

dotnet publish -t:PublishContainer -p ContainerImageTag=published
dotnet publish -t:PublishContainer -p ContainerImageTag=alpine -p ContainerFamily=alpine 
dotnet publish -t:PublishContainer -p ContainerImageTag=chiseled -p ContainerFamily=noble-chiseled
dotnet publish -t:PublishContainer -p ContainerImageTag=aot -p ContainerFamily=noble-chiseled-extra

--- packages

docker run --rm anchore/syft mcr.microsoft.com/dotnet/runtime:10.0 | grep dotnet | wc -l
docker run --rm anchore/syft mcr.microsoft.com/dotnet/runtime:10.0 | grep deb | wc -l

docker run --rm anchore/syft mcr.microsoft.com/dotnet/runtime:10.0-noble-chiseled | grep dotnet | wc -l
docker run --rm anchore/syft mcr.microsoft.com/dotnet/runtime:10.0-noble-chiseled | grep deb | wc -l

--- security

docker run -it --rm --entrypoint /bin/bash hello-containers:latest

whoami
apt

docker run -it --rm --entrypoint /bin/bash --user root hello-containers:latest

---

wget https://github.com/aquasecurity/trivy/releases/download/v0.58.2/trivy_0.58.2_Linux-ARM64.deb
sudo dpkg -i trivy_0.58.2_Linux-ARM64.deb

trivy i hello-containers
trivy i hello-containers:alpine
trivy i hello-containers:chiseled

dotnet publish -t:PublishContainer -p ContainerImageTag=arm64 --arch arm64
docker run sampleapi:arm64

ACR_NAME=cacontainersdemo.azurecr.io
az acr build --registry $ACR_NAME --image test:v1 --file Dockerfile .

$env:ACR_NAME = "cacontainersdemo.azurecr.io"          
az acr build --registry $env:ACR_NAME --image test:v1 --file Dockerfile .


docker login cacontainersdemo.azurecr.io

az acr login -n $ACR_NAME
docker run -p 8080:8080 $ACR_NAME/test:v1

