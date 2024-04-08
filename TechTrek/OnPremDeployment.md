# Deploy TechTrek On-Premise

While we love the capability the cloud offers us, it is not the be-all and we should also consider making our application work in on-premise scenarios. Fortunately, becuase the TechTrek is Dockerized, this becomes a trivial matter.

The TechTrek build and release pipeline will create a Linux and Windows docker container and all the application dependencies.

For this on-premise scenario, we will assume that our customer only has Windows Server skills in their organisation. For this and other reasons they are not able to consider running Kubernetes on premise.

> Before completing this installation review [this document](./OnPremConsiderations.md) to ensure that you give due consideration to your on-premise installation. 

## Installing Docker on Windows Server

Windows Server 2016 and later versions support Docker and Windows Containers. Install the Docker Engine on the Windows Server if it's not already installed.

You could use `Add Roles and Features` GUI or the following PowerShell commands to install the required services:

```powershell
Install-Module -Name DockerMsftProvider -Repository PSGallery -Force
Install-Package -Name docker -ProviderName DockerMsftProvider
Restart-Computer -Force
```

## Check List

In order the check if the feature has been installed run:

```powershell
Get-WindowsFeature -Name containers
```

The check the Docker is installed run:

```powershell
docker version
```

Check the Docker client communication info:

```powershell
docker info
```

Check that the Docker service engine is running:

```powershell
Get-Service docker
```

> The commands listed under this check list should be your goto commands to trouble shoot issues in the future.

## Download and Install a Test Docker Image

> This assumes that the server has access to the internet.

Use the following sample app as a test:

```powershell
docker run microsoft/dotnet-samples:dotnetapp-nanoserver-1809
```

If this works, then downloading and installing TechTrek image should just work.

```powershell
//TODO: DWS: Once the images are built 
docker run -d -p <HostPort>:<ContainerPort> --name <YourAppName> <YourImageName>
```

