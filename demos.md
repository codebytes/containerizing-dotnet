
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
dotnet publish -t:PublishContainer -p ContainerImageTag=chiseled -p ContainerFamily=jammy-chiseled

--- packages

docker run --rm anchore/syft mcr.microsoft.com/dotnet/runtime:8.0 | grep dotnet | wc -l
docker run --rm anchore/syft mcr.microsoft.com/dotnet/runtime:8.0 | grep deb | wc -l

docker run --rm anchore/syft mcr.microsoft.com/dotnet/runtime:8.0-jammy-chiseled | grep dotnet | wc -l
docker run --rm anchore/syft mcr.microsoft.com/dotnet/runtime:8.0-jammy-chiseled | grep deb | wc -l

--- security

docker run -it --rm --entrypoint /bin/bash hello-containers:latest

whoami
apt

docker run -it --rm --entrypoint /bin/bash --user root hello-containers:latest

---

wget https://github.com/aquasecurity/trivy/releases/download/v056.1/trivy_0.56.1_Linux-ARM64.deb
sudo dpkg -i trivy_0.18.3_Linux-64bit.deb

trivy i hello-containers
trivy i hello-containers:alpine
trivy i hello-containers:chiseled

dotnet publish -t:PublishContainer -p ContainerImageTag=arm64 --arch arm64
docker run sampleapi:arm64

ACR_NAME=cacontainersdemo.azurecr.io
az acr build --registry $ACR_NAME --image test:v1 --file Dockerfile .

docker login cacontainersdemo.azurecr.io

az acr login -n $ACR_NAME
docker run -p 8080:8080 $ACR_NAME/test:v1

