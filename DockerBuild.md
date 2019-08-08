# Nuke Build inside a Docker container #

## Why? ##
- Building inside a Docker container allows *anyone* to build without having anything but Docker installed.
- The build runs the same OS/Tools/Versions on all machines
- New versions of OS/Tools can easily be tested

## How ##

### 1. Create a dockerfile ###

This example is a Windows Server 2019 with .net framework 4.7.2 installed
```dockerfile
FROM mcr.microsoft.com/dotnet/framework/sdk:4.7.2-windowsservercore-ltsc2016
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]
```

### 2. Create a Powershell file to start the build ###

Create a file named e.g. `DockerBuild.ps1`
```powershell
param (
    [string]$target = "Build"
 )

# Build the dockerfile into a image called 'buildcontainer'
docker build -t buildcontainer .

# Start a new instance of the image 'buildcontainer'
# -i Keep STDIN open even if not attached
# -rm Automatically remove the container when it exits
# -w Working directory inside the container
# -m Memory limit (1GB is default on Windows)
# -v Bind mount a volume <path outside container>:<path inside container> ${PWD} is the current working dir
# "buildcontainer" name of the image
# "powershell .\Build.ps1 $target" the command that are run inside the container
docker run -i --rm -w c:\build -m 2gb -v ${PWD}:c:\build buildcontainer powershell .\Build.ps1 $target
```

### 3. Start build ###
```powershell 
.\DockerBuild.ps1 -Target:Build
```