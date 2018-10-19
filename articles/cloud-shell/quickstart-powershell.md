---
title: PowerShell in de Snelstartgids voor Azure Cloud Shell | Microsoft Docs
description: Snelstartgids voor PowerShell in Cloudshell
services: Azure
documentationcenter: ''
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: damaerte
ms.openlocfilehash: 0bce9f50bdc3ac8fb4675a7ac2a3fb300036973f
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49404356"
---
# <a name="quickstart-for-powershell-in-azure-cloud-shell"></a>Snelstartgids voor PowerShell in Azure Cloudshell

Dit document wordt uitgelegd hoe het gebruik van PowerShell in Cloud Shell in de [Azure-portal](https://portal.azure.com/).

> [!NOTE]
> Een [Bash in Azure Cloud Shell](quickstart.md) Quick Start is ook beschikbaar.

## <a name="start-cloud-shell"></a>Cloudshell starten

1. Klik op **Cloud Shell** knop in de bovenste navigatiebalk van de Azure-portal

  ![](media/quickstart-powershell/shell-icon.png)

2. Selecteer de PowerShell-omgeving in de vervolgkeuzelijst en kunt u zich in de Azure-station `(Azure:)`

  ![](media/quickstart-powershell/environment-ps.png)

## <a name="run-powershell-commands"></a>Voer PowerShell-opdrachten

Uitvoeren van reguliere PowerShell-opdrachten in de Cloud Shell, zoals:

```azurepowershell-interactive
PS Azure:\> Get-Date

# Expected Output
Friday, July 27, 2018 7:08:48 AM

PS Azure:\> Get-AzureRmVM -Status

# Expected Output
ResourceGroupName       Name       Location                VmSize   OsType     ProvisioningState  PowerState
-----------------       ----       --------                ------   ------     -----------------  ----------
MyResourceGroup2        Demo        westus         Standard_DS1_v2  Windows    Succeeded           running
MyResourceGroup         MyVM1       eastus            Standard_DS1  Windows    Succeeded           running
MyResourceGroup         MyVM2       eastus   Standard_DS2_v2_Promo  Windows    Succeeded           deallocated
```

## <a name="navigate-azure-resources"></a>Azure-resources navigeren

 1. Lijst met al uw abonnementen van `Azure` station

    ```azurepowershell-interactive
    PS Azure:\> dir
    ```

 2. `cd` op het gewenste abonnement

    ```azurepowershell-interactive
    PS Azure:\> cd MySubscriptionName
    PS Azure:\MySubscriptionName>
    ```

 3. Al uw Azure-resources onder het huidige abonnement weergeven

    Type `dir` om meerdere weergaven van uw Azure-resources weer te geven.

    ```azurepowershell-interactive
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

```azurepowershell-interactive
PS Azure:\MySubscriptionName> dir AllResources
```

### <a name="explore-resource-groups"></a>Resourcegroepen verkennen

 Gaat u naar de `ResourceGroups` Active directory en binnen een specifieke resourcegroep kunt u virtuele machines vinden.

```azureowershell-interactive
PS Azure:\MySubscriptionName> cd ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines

PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines> dir


    Directory: Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines


VMName    Location   ProvisioningState VMSize          OS            SKU             OSVersion AdminUserName  NetworkInterfaceName
------    --------   ----------------- ------          --            ---             --------- -------------  --------------------
TestVm1   westus     Succeeded         Standard_DS2_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo371
TestVm2   westus     Succeeded         Standard_DS1_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo271
```

> [!NOTE]
> U merkt dat de tweede keer wanneer u typt `dir`, kan de Cloud Shell om weer te geven van de items die veel sneller.
> Dit is omdat de onderliggende items in cache zijn opgeslagen in het geheugen voor een betere gebruikerservaring.
U kunt echter altijd gebruiken `dir -Force` nieuwe gegevens op te halen.

### <a name="navigate-storage-resources"></a>Storage-resources navigeren

Door te voeren in de `StorageAccounts` Active directory, kunt u eenvoudig uw opslagresources navigeren

```azureowershell-interactive
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> dir

    Directory: Azure:\MySubscriptionNameStorageAccounts\MyStorageAccountName\Files

Name          ConnectionString
----          ----------------
MyFileShare1  \\MyStorageAccountName.file.core.windows.net\MyFileShare1;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare2  \\MyStorageAccountName.file.core.windows.net\MyFileShare2;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare3  \\MyStorageAccountName.file.core.windows.net\MyFileShare3;AccountName=MyStorageAccountName AccountKey=<key>
```

Met de verbindingsreeks, kunt u de volgende opdracht uit om te koppelen van de Azure-bestandsshare.

```azurepowershell-interactive
net use <DesiredDriveLetter>: \\<MyStorageAccountName>.file.core.windows.net\<MyFileShareName> <AccountKey> /user:Azure\<MyStorageAccountName>
```

Zie voor meer informatie, [een Azure-bestandsshare koppelen en de share openen in Windows][azmount].

U kunt ook de mappen onder de Azure-bestandsshare als volgt navigeren:

```azurepowershell-interactive
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> cd .\MyFileShare1\
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files\MyFileShare1> dir

Mode  Name
----  ----
+     TestFolder
.     hello.ps1
```

### <a name="interact-with-virtual-machines"></a>Werken met virtuele machines

U vindt alle virtuele machines weergegeven onder het huidige abonnement via `VirtualMachines` directory.

```azurepowershell-interactive
PS Azure:\MySubscriptionName\VirtualMachines> dir

    Directory: Azure:\MySubscriptionName\VirtualMachines


Name       ResourceGroupName  Location  VmSize          OsType              NIC ProvisioningState  PowerState
----       -----------------  --------  ------          ------              --- -----------------  ----------
TestVm1    MyResourceGroup1   westus    Standard_DS2_v2 Windows       my2008r213         Succeeded     stopped
TestVm2    MyResourceGroup1   westus    Standard_DS1_v2 Windows          jpstest         Succeeded deallocated
TestVm10   MyResourceGroup2   eastus    Standard_DS1_v2 Windows           mytest         Succeeded     running
```

#### <a name="invoke-powershell-script-across-remote-vms"></a>PowerShell-script aanroepen naar externe VM 's

 > [!WARNING]
 > Raadpleeg [het oplossen van extern beheer van virtuele Azure-machines](troubleshooting.md#troubleshooting-remote-management-of-azure-vms).

  Ervan uitgaande dat u hebt een virtuele machine, MyVM1, gebruiken we `Invoke-AzVMCommand` om aan te roepen, een scriptblok PowerShell op de externe computer.

  ```azurepowershell-interactive
  Invoke-AzVMCommand -Name MyVM1 -ResourceGroupName MyResourceGroup -Scriptblock {Get-ComputerInfo} -EnableRemoting
  ```

  U kunt ook eerst navigeren naar de map van de virtuele machines en uitvoeren `Invoke-AzVMCommand` als volgt.

  ```azurepowershell-interactive
  PS Azure:\> cd MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines
  PS Azure:\MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Invoke-AzVMCommand -Scriptblock {Get-ComputerInfo}

  # You will see output similar to the following:

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

#### <a name="interactively-log-on-to-a-remote-vm"></a>Interactief aanmelden bij een externe VM

U kunt `Enter-AzVM` interactief aanmelden bij een virtuele machine die wordt uitgevoerd in Azure.

  ```azurepowershell-interactive
  PS Azure:\> Enter-AzVM -Name MyVM1 -ResourceGroupName MyResourceGroup -EnableRemoting
  ```

U kunt ook navigeren naar de `VirtualMachines` directory eerste en werken met `Enter-AzVM` als volgt

  ```azurepowershell-interactive
 PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Enter-AzVM
 ```

### <a name="discover-webapps"></a>Web-Apps detecteren

Door te voeren in de `WebApps` Active directory, kunt u eenvoudig uw web-apps resources navigeren

```azurepowershell-interactive
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

Als u wilt verifiëren op servers of virtuele machines met behulp van SSH, het openbaar / persoonlijk sleutelpaar genereren in Cloud Shell en publiceren van de openbare sleutel toe `authorized_keys` op de externe computer, zoals `/home/user/.ssh/authorized_keys`.

> [!NOTE]
> U kunt SSH met behulp van privé-openbare-sleutels maken `ssh-keygen` en deze publiceren naar `$env:USERPROFILE\.ssh` in Cloud Shell.

### <a name="using-ssh"></a>Met behulp van SSH

Volg de instructies [hier](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-powershell) te maken van een nieuwe VM-configuratie met behulp van de AzureRM-cmdlets.
Voordat aanroepen in `New-AzureRmVM` als u wilt een vliegende start de implementatie, openbare SSH-sleutel aan de VM-configuratie toevoegen.
De zojuist gemaakte virtuele machine bevat de openbare sleutel in de `~\.ssh\authorized_keys` locatie, waardoor u SSH-sessie referentie gratis aan de virtuele machine.

```azurepowershell-interactive
# Create VM config object - $vmConfig using instructions on linked page above

# Generate SSH keys in Cloud Shell
ssh-keygen -t rsa -b 2048 -f $HOME\.ssh\id_rsa 

# Ensure VM config is updated with SSH keys
$sshPublicKey = Get-Content "$HOME\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmConfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

# Create a virtual machine
New-AzureRmVM -ResourceGroupName <yourResourceGroup> -Location <vmLocation> -VM $vmConfig

# SSH to the VM
ssh azureuser@MyVM.Domain.Com
```

## <a name="list-available-commands"></a>Lijst met beschikbare opdrachten

Onder `Azure` station, typt u `Get-AzureRmCommand` om op te halen van context-specifieke Azure-opdrachten.

U kunt ook altijd gebruiken `Get-Command *azurerm* -Module AzureRM.*` om erachter te komen de beschikbare Azure-opdrachten.

## <a name="install-custom-modules"></a>Aangepaste modules installeren

U kunt uitvoeren `Install-Module` voor het installeren van modules van de [PowerShell Gallery][gallery].

## <a name="get-help"></a>Get-Help

Type `Get-Help` voor informatie over PowerShell in Azure Cloud Shell.

```azurepowershell-interactive
Get-Help
```

Voor een specifieke opdracht u nog steeds kunt doen `Get-Help` gevolgd door een cmdlet.

```azurepowershell-interactive
Get-Help Get-AzureRmVM
```

## <a name="use-azure-files-to-store-your-data"></a>Azure Files gebruiken voor het opslaan van uw gegevens

U kunt bijvoorbeeld een script maken `helloworld.ps1`, en sla deze op uw `clouddrive` te gebruiken in shell-sessies.

```azurepowershell-interactive
cd $HOME\clouddrive
code .\helloworld.ps1
# Add the content, such as 'Hello World!'
.\helloworld.ps1
Hello World!
```

Volgende keer wanneer u PowerShell in Cloud Shell gebruiken de `helloworld.ps1` bestand blijft aanwezig in de `$HOME\clouddrive` directory dat uw Azure Files-share koppelt.

## <a name="use-custom-profile"></a>Aangepast profiel gebruiken

U kunt uw PowerShell-omgeving, aanpassen door het maken van PowerShell-profielen - `profile.ps1` (of `Microsoft.PowerShell_profile.ps1`).
Sla het onder `$profile.CurrentUserAllHosts` (of `$profile.CurrentUserAllHosts`), zodat deze kan worden geladen in elke PowerShell in Cloud Shell-sessie.

Voor informatie over het maken van een profiel, raadpleeg dan [over profielen][profile].

## <a name="use-git"></a>Werken met Git

Als u wilt klonen van een Git-opslagplaats in de Cloud Shell, moet u maken een [persoonlijk toegangstoken] [ githubtoken] en deze gebruiken als de gebruikersnaam. Nadat u hebt uw token, kloon de opslagplaats als volgt:

```azurepowershell-interactive
  git clone https://<your-access-token>@github.com/username/repo.git
```

## <a name="exit-the-shell"></a>De shell afsluiten

Type `exit` om de sessie te beëindigen.

[bashqs]:quickstart.md
[gallery]:https://www.powershellgallery.com/
[customex]:https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript
[profile]: https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_profiles
[azmount]: https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows
[githubtoken]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
