
# title

## Goal

To deploy the same app to different compute options, including

- Virtual machine (Windows and Linux)
- Web app (Windows and Linux)
- Container
- Kubernetes
- Azure Functions (?)
- API management

## The code

- VSCODE - core 2.1
- vscode - core 3.1 asp generate templates not updated
- switched to vs2019
- got simple rest api working
- added memory cache, some dependencies not right version
- finally got that working
- decided, fine, good enough for a POC
- Went to check-in integrated Git, plug-in could not authenticate
- sorted that out, realized that plugin still defaults to 'master' while github defaults to 'main'
- can't merge them because not same orgin
- fine, finally decided to go nuclear option, creat blank new folder and clone empty repo from GH, copy solution files, add, commit, push. and on the 7th day, G-d rested..... sorry, that's a different story.

## VM on Windows

Manual

Ok, first up, publish to a VM. I'v used VMs. And even published some old ASP.net sites back before there were 12 versions of the .net framework

almost too many trouble to list
how the crap do i deploy an ASP.Net Core app to IIS on Windows datacenter v2?

- finally got it working by uninstalling all asp.core and re-installing (possibly not needed..)
- published from vs using file, target framework core, frameowrk dependent (not stand-alone) and portable option
- copied app to IIS site folder, allowed read-permissions, and it worked
- except i did forget to publish my data folder, so after that it worked. Well, locally

next challnge, accessible from my machine (not the azure vm)

- added a nsg inbound rule to allow traffic on 8080. seems like it should work, but nope. next i'll check the iis server setting?
- Actually, needed to add an inbound firewall rule to the VM firewall

whew ... that was more difficult than expected

next steps for VM deployment: be able to auto install the asp.net core hosting and deploy the app and data folder at VM init. maybe also try with a spot instance. Extra credit for setting up ci/cd pipeline with azure devops (or hopefully just github actions). that may be a bridge too far for now, but at least i got a small ugly win.

ARM with embedded script

## VM on Linux

## App Service (Windows)

script init
<code>
\# Install IIS (with Management Console)
Install-WindowsFeature -name Web-Server -IncludeManagementTools

\# Install ASP.NET 4.6
Install-WindowsFeature Web-Asp-Net45

\# Install Web Management Service
Install-WindowsFeature -Name Web-Mgmt-Service

$whb_installer_url = "https://download.visualstudio.microsoft.com/download/pr/fa3f472e-f47f-4ef5-8242-d3438dd59b42/9b2d9d4eecb33fe98060fd2a2cb01dcd/dotnet-hosting-3.1.0-win.exe"

\# use the scratch drive
$whb_installer_file = "D:\dotnet-hosting.exe"

Invoke-WebRequest -Uri $whb_installer_url -OutFile $whb_installer_file

Invoke-Expression -Command "$whb_installer_file /install /quiet /norestart"

###########################################################

$wd_installer_url = "https://download.microsoft.com/download/0/1/D/01DC28EA-638C-4A22-A57B-4CEF97755C6C/WebDeploy_amd64_en-US.msi"

\# use the scratch drive
$wd_installer_file = "D:\WebDeploy.msi"
Invoke-WebRequest -Uri $wd_installer_url -OutFile $wd_installer_file

Invoke-Expression -Command "wd_installer_file /quiet /norestart"

\# next steps:
\# install cli to pull git repo and deploy

</code>

FTP deployment - success

From Visual Studio

Azure CLI

Mostluy successful with az CLI

<code>az webapp deployment source config-zip --src .\publish.zip -n "starwarsdialogapiv2" -g az104 </code>

Todo:

- I need to add my data folder and files to my deployment
- Move folder/file paths to web.config or whatever the modern incarnation is

## App Service (Linux)
