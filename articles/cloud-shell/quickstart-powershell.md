---
title: PowerShell in Quick Start Azure-Cloud-Shell (Preview) | Microsoft Docs
description: Quick Start voor PowerShell in de Cloud-Shell
services: Azure
documentationcenter: 
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: damaerte
ms.openlocfilehash: b454720dd5bd2df036a400c8bfc1c383de5af542
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="quickstart-for-powershell-in-azure-cloud-shell-preview"></a>Quick Start voor PowerShell in de Azure-Cloud-Shell (Preview)

In dit document worden de PowerShell gebruiken in de Cloud-Shell in de [Azure-portal](https://aka.ms/PSCloudPreview).

> [!NOTE]
> Een [Bash in de Azure-Cloud-Shell](quickstart.md) Quick Start is ook beschikbaar.

## <a name="start-cloud-shell"></a>Cloud-Shell starten

1. Klik op **Cloud Shell** knop van de bovenste navigatiebalk van de Azure portal

  ![](media/quickstart-powershell/shell-icon.png)

2. Selecteer de PowerShell-omgeving in de vervolgkeuzelijst en kunt u in Azure station`(Azure:)`

  ![](media/quickstart-powershell/environment-ps.png)

## <a name="run-powershell-commands"></a>PowerShell-opdrachten uitvoeren

Voer reguliere PowerShell-opdrachten in de Cloud-Shell, zoals:

```Powershell
PS Azure:\> Get-Date
Monday, September 25, 2017 08:55:09 AM

PS Azure:\> Get-AzureRmVM -Status

ResourceGroupName       Name       Location                VmSize   OsType     ProvisioningState  PowerState
-----------------       ----       --------                ------   ------     -----------------  ----------
MyResourceGroup2        Demo        westus         Standard_DS1_v2  Windows    Succeeded           running
MyResourceGroup         MyVM1       eastus            Standard_DS1  Windows    Succeeded           running
MyResourceGroup         MyVM2       eastus   Standard_DS2_v2_Promo  Windows    Succeeded           deallocated
```

## <a name="navigate-azure-resources"></a>Azure-resources navigeren

 1. Lijst van uw abonnementen

    ``` Powershell
    PS Azure:\> dir
    ```

 2. `cd`aan uw voorkeur abonnement

    ``` Powershell
    PS Azure:\> cd MySubscriptionName
    PS Azure:\MySubscriptionName>
    ```

 3. Alle Azure-resources onder het huidige abonnement weergeven
 
    Type `dir` voor een lijst met meerdere weergaven van uw Azure-resources.
 
    ``` PowerShell
    PS Azure:\MySubscriptionName> dir

        Directory: azure:\MySubscriptionName

    Mode Name
    ---- ----
    +    AllResources
    +    ResourceGroups
    +    StorageAccounts
    +    VirtualMachines
    +    WebApps
     ```

### <a name="allresources-view"></a>AllResources weergeven 
Type `dir` onder `AllResources` directory om uw Azure-resources weer te geven.
    
    PS Azure:\MySubscriptionName> dir AllResources

### <a name="explore-resource-groups"></a>Resourcegroepen verkennen

 Gaat u naar de `ResourceGroups` directory en binnen een specifieke resourcegroep vindt u de virtuele machines.

``` PowerShell
PS Azure:\MySubscriptionName> cd ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines

PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines> dir


    Directory: Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines


VMName    Location   ProvisioningState VMSize          OS            SKU             OSVersion AdminUserName  NetworkInterfaceName
------    --------   ----------------- ------          --            ---             --------- -------------  --------------------
TestVm1   westus     Succeeded         Standard_DS2_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo371
TestVm2   westus     Succeeded         Standard_DS1_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo271

```
> [!NOTE]
> U merkt dat de tweede keer wanneer u typt `dir`, de cloud-shell is in staat om de items die veel sneller weer te geven.
> Dit is omdat de onderliggende items in cache zijn opgeslagen in het geheugen voor een betere gebruikerservaring.
U kunt echter altijd gebruiken `dir -Force` nieuwe gegevens ophalen.

### <a name="navigate-storage-resources"></a>Storage-resources navigeren
    
Door te voeren in de `StorageAccounts` map die u kunt eenvoudig uw storage-resources navigeren
    
``` PowerShell 
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> dir

    Directory: Azure:\MySubscriptionNameStorageAccounts\MyStorageAccountName\Files


Name          ConnectionString
----          ----------------
MyFileShare1  \\MyStorageAccountName.file.core.windows.net\MyFileShare1;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare2  \\MyStorageAccountName.file.core.windows.net\MyFileShare2;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare3  \\MyStorageAccountName.file.core.windows.net\MyFileShare3;AccountName=MyStorageAccountName AccountKey=<key>


```

Met de verbindingsreeks kunt u de volgende opdracht om te koppelen van de share Azure-bestanden.
        
``` PowerShell
net use <DesiredDriveLetter>: \\<MyStorageAccountName>.file.core.windows.net\<MyFileShareName> <AccountKey> /user:Azure\<MyStorageAccountName>


```

Zie voor meer informatie [koppelen van een Azure-bestanden delen en toegang tot de share in Windows][azmount].

U kunt ook de mappen onder de share Azure bestanden als volgt navigeren:

            
``` PowerShell
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> cd .\MyFileShare1\
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files\MyFileShare1> dir

Mode  Name
----  ----
+     TestFolder
.     hello.ps1

    
```

### <a name="interact-with-virtual-machines"></a>Werken met virtuele machines

U vindt uw virtuele machines onder het huidige abonnement via `VirtualMachines` directory.
    
``` PowerShell
PS Azure:\MySubscriptionName\VirtualMachines> dir

    Directory: Azure:\MySubscriptionName\VirtualMachines


Name       ResourceGroupName  Location  VmSize          OsType              NIC ProvisioningState  PowerState
----       -----------------  --------  ------          ------              --- -----------------  ----------
TestVm1    MyResourceGroup1   westus    Standard_DS2_v2 Windows       my2008r213         Succeeded     stopped
TestVm2    MyResourceGroup1   westus    Standard_DS1_v2 Windows          jpstest         Succeeded deallocated
TestVm10   MyResourceGroup2   eastus    Standard_DS1_v2 Windows           mytest         Succeeded     running


```

#### <a name="invoke-powershell-script-across-remote-vms"></a>Aanroepen van PowerShell-script over externe VM 's

 > [!WARNING]
 > Raadpleeg [probleemoplossing extern beheer van Azure Virtual machines](troubleshooting.md#powershell-resolutions).

  Ervan uitgaande dat u een virtuele machine, MyVM1, hebben we gebruiken `Invoke-AzureRmVMCommand` aan te roepen, een PowerShell-scriptblock op de externe computer.

  ``` Powershell
  Invoke-AzureRmVMCommand -Name MyVM1 -ResourceGroupName MyResourceGroup -Scriptblock {Get-ComputerInfo} -EnableRemoting
  ```
  U kunt ook eerst Navigeer naar de map van de virtuele machines en `Invoke-AzureRmVMCommand` als volgt.

  ``` Powershell
  PS Azure:\> cd MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines
  PS Azure:\MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Invoke-AzureRmVMCommand -Scriptblock{Get-ComputerInfo}
  ```
  U ziet de uitvoer ziet er als volgt:

  ``` Powershell
  PSComputerName                                          : 65.52.28.207
  RunspaceId                                              : 2c2b60da-f9b9-4f42-a282-93316cb06fe1
  WindowsBuildLabEx                                       : 14393.1066.amd64fre.rs1_release_sec.170327-1835
  WindowsCurrentVersion                                   : 6.3
  WindowsEditionId                                        : ServerDatacenter
  WindowsInstallationType                                 : Server
  WindowsInstallDateFromRegistry                          : 5/18/2017 11:26:08 PM
  WindowsProductId                                        : 00376-40000-00000-AA947
  WindowsProductName                                      : Windows Server 2016 Datacenter
  WindowsRegisteredOrganization                           :
   ...
  ```

#### <a name="interactively-log-on-to-a-remote-vm"></a>Interactief aanmelden bij een externe virtuele machine

U kunt `Enter-AzureRmVM` interactief aanmelden bij een virtuele machine in Azure wordt uitgevoerd.

  ``` Powershell
  Enter-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup -EnableRemoting
  ```

U kunt ook navigeren naar de `virtualMachines` directory eerste en voer `Enter-AzureRmVM` als volgt

  ``` Powershell
 PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Enter-AzureRmVM
 ```

### <a name="discover-webapps"></a>WebApps detecteren

Door te voeren in de `WebApps` map die u kunt eenvoudig uw web-apps resources navigeren

``` PowerShell
PS Azure:\MySubscriptionName> dir .\WebApps\

    Directory: Azure:\MySubscriptionName\WebApps


Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Stopped  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US



# You can use Azure cmdlets to Start/Stop your web apps
PS Azure:\MySubscriptionName\WebApps> Start-AzureRmWebApp -Name mywebapp1 -ResourceGroupName MyResourceGroup1

Name           State    ResourceGroup        EnabledHostNames                   Location
----           -----    -------------        ----------------                   --------
mywebapp1      Running  MyResourceGroup1     {mywebapp1.azurewebsites.net ...   West US

# Refresh the current state with -Force
PS Azure:\MySubscriptionName\WebApps> dir -Force

    Directory: Azure:\MySubscriptionName\WebApps


Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Running  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US

```

## <a name="ssh"></a>SSH

[Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH) is beschikbaar in PowerShell CloudShell.
Het openbaar / persoonlijk sleutelpaar genereren in CloudShell voor verificatie op servers of virtuele machines met SSH, en de openbare sleutel te publiceren `authorized_keys` op de externe computer, zoals `/home/user/.ssh/authorized_keys`.

> [!NOTE]
> U kunt SSH privé-openbare sleutels maken `ssh-keygen` en deze publiceren naar `$env:USERPROFILE\.ssh` in CloudShell.

### <a name="using-a-custom-profile-to-persist-git-and-ssh-settings"></a>Met behulp van een aangepast profiel voor het persistent maken van GIT en SSH-instellingen

Aangezien sessies blijven niet behouden bij het ze afmelden Sla uw `$env:USERPROFILE\.ssh` map `CloudDrive` of maak een symlink wanneer CloudShell wordt gestart.
Plaats de volgende code in uw profile.ps1 voor het maken van een symlink naar CloudDrive snipped.

``` Powershell
# Check if the ssh folder exists
if( -not (Test-Path $home\CloudDrive\.ssh){
    mkdir $home\CloudDrive\.ssh
}

# .ssh path relative to this script
$script:sshFolderPath = Join-Path $PSScriptRoot .ssh

# Create a symlink to .ssh in user's $home
if(Test-Path $script:sshFolderPath){
   if(-not (Test-Path (Join-Path $HOME .ssh ))){
        New-Item -ItemType SymbolicLink -Path $HOME -Name .ssh -Value $script:sshFolderPath
   }
}

```

### <a name="using-ssh"></a>Gebruik van SSH

Volg de instructies [hier](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-powershell) voor het maken van een nieuwe VM-configuratie met AzureRM-Cmdlets.
Vóór aanroep naar de `New-AzureRMVM` toevoegen voor het uitgangspunt voor de implementatie, openbare SSH-sleutel aan het VM-configuratie.
De zojuist gemaakte virtuele machine bevat de openbare sleutel in de `~\.ssh\authorized_keys` locatie, waardoor u referentie gratis ssh-sessie op de virtuele machine.

``` Powershell

# Create VM config object - $vmConfig using instructions on linked page above

# Generate SSH Keys in CloudShell
ssh-keygen -t rsa -b 2048 -f $HOME\.ssh\id_rsa 

# Ensure VM config is updated with SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmConfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

# Create a virtual machine
New-AzureRmVM -ResourceGroupName <yourResourceGroup> -Location <vmLocation> -VM $vmConfig

# ssh to the VM
ssh azureuser@MyVM.Domain.Com

```


## <a name="list-available-commands"></a>Lijst met beschikbare opdrachten

Onder `Azure` station `Get-AzureRmCommand` Contextspecifieke Azure opdrachten ophalen.

U kunt ook altijd gebruiken `Get-Command *azurerm* -Module AzureRM.*` om erachter te komen de beschikbare Azure opdrachten.

## <a name="install-custom-modules"></a>Aangepaste modules installeren

U kunt uitvoeren `Install-Module` voor het installeren van modules van de [PowerShell Gallery][gallery].

## <a name="get-help"></a>Get-Help

Type `Get-Help` voor informatie over PowerShell in Azure Cloud-Shell.

``` Powershell
PS Azure:\> Get-Help
```

U kunt nog steeds gevolgd door een cmdlet Get-Help voor een specifieke opdracht doen.

``` Powershell
PS Azure:\> Get-Help Get-AzureRmVM
```

## <a name="use-azure-files-to-store-your-data"></a>Azure-bestanden gebruiken voor het opslaan van uw gegevens

Kunt u een script spreek `helloworld.ps1`, en sla deze op uw `CloudDrive` gebruiken over de shell-sessies.

``` Powershell
cd C:\users\ContainerAdministrator\CloudDrive
PS C:\users\ContainerAdministrator\CloudDrive> vim .\helloworld.ps1
# Add the content, such as 'Hello World!'
PS C:\users\ContainerAdministrator\CloudDrive> .\helloworld.ps1
Hello World!
```

Wanneer u PowerShell in de Cloud-Shell gebruiken zodra de `helloworld.ps1` bestand blijft aanwezig in de `CloudDrive` map die uw Azure-bestanden-share koppelt.

## <a name="use-custom-profile"></a>Aangepast profiel gebruiken

U kunt uw PowerShell-omgeving aanpassen door het maken van PowerShell-profielen - `profile.ps1` of `Microsoft.PowerShell_profile.ps1`. Sla deze onder de `CloudDrive` zodat deze kan worden geladen in elke PowerShell-sessie wanneer het starten van de Cloud-Shell.

Voor het maken van een profiel, verwijzen naar [over profielen][profile].

## <a name="use-git"></a>Git gebruiken

Als u wilt een git-opslagplaats in de Cloud-Shell klonen, moet u maken een [persoonlijke toegangstoken] [ githubtoken] en deze gebruiken als de gebruikersnaam. Eenmaal hebt u uw token, kloon de opslagplaats als volgt:

 ``` PowerShell
  git clone https://<your-access-token>@github.com/username/repo.git

```
Aangezien sessies in de Cloud-Shell, blijven niet behouden ze wanneer u zich afmeldt of de sessietime-out optreedt, bestaat de Git-configuratiebestand niet bij de volgende aanmelding. Als u wilt dat uw Git-config behouden, moet u uw .gitconfig naar opslaan uw `CloudDrive` en kopieert u deze of een symlink te maken wanneer de Cloud-Shell wordt gestart. Gebruik het volgende codefragment in uw profile.ps1 voor het maken van een symlink naar `CloudDrive`.

 ``` PowerShell
 
# .gitconfig path relative to this script
$script:gitconfigPath = Join-Path $PSScriptRoot .gitconfig

# Create a symlink to .gitconfig in user's $home
if(Test-Path $script:gitconfigPath){

    if(-not (Test-Path (Join-Path $Home .gitconfig ))){
         New-Item -ItemType SymbolicLink -Path $home -Name .gitconfig -Value $script:gitconfigPath
    }
}

```
## <a name="exit-the-shell"></a>De shell afsluiten

Type `exit` om de sessie te beëindigen.

[bashqs]:quickstart.md
[gallery]:https://www.powershellgallery.com/
[customex]:https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript
[profile]: https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_profiles
[azmount]: https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows
[githubtoken]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
