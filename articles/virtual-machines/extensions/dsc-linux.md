---
title: Azure DSC-extensie voor Linux
description: OMI en DSC-pakketten om toe te staan een virtuele Azure Linux-machine moet worden geconfigureerd met behulp van Desired State Configuration installeert.
services: virtual-machines-linux
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: robreed
ms.openlocfilehash: 34b70b1a6a77a20a034a7822d9c4961c36cdd51c
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55663958"
---
# <a name="dsc-extension-for-linux-microsoftostcextensionsdscforlinux"></a>DSC-extensie voor Linux (Microsoft.OSTCExtensions.DSCForLinux)

## <a name="overview"></a>Overzicht

Desired State Configuration (DSC) is een beheerplatform waarmee u voor het beheren van uw IT-afdeling en infrastructuur voor ontwikkeling met configuratie als code.

DSCForLinux uitbreiding is gepubliceerd en ondersteund door Microsoft. De uitbreiding is de OMI en DSC-agent geïnstalleerd op Azure virtual machines. DSC-extensie kunt ook de volgende acties uitvoeren


- Registreren van de Linux-VM in Azure Automation-account om te kunnen pull-configuraties van Azure Automation-service (ExtensionAction registreren)
- MOF-configuraties push naar de Linux-VM (ExtensionAction Push)
- MOF-Meta-configuratie toepassen op de Linux-VM Pull-Server configureren om het pull-knooppuntconfiguratie (Pull ExtensionAction)
- Aangepaste DSC-modules installeren op de Linux-VM (ExtensionAction installeren)
- Verwijderen van aangepaste DSC-modules voor de Linux-VM (ExtensionAction verwijderen)

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De DSC-Linux-extensie biedt ondersteuning voor alle de [op Azure onderschreven Linux-distributies](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/endorsed-distros) , met uitzondering:

| Distributie | Versie |
|---|---|
| Debian | Alle versies |
| Ubuntu| 18.04 |
 
### <a name="internet-connectivity"></a>Internetconnectiviteit

De extensie DSCForLinux is vereist dat de virtuele doelmachine is verbonden met internet. Bijvoorbeeld: Register extensie heeft verbinding met de Automation-service. Voor andere vereiste acties, zoals Pull, Pull, vereist installatie een verbinding met azure storage en/of github. Dit is afhankelijk van instellingen die zijn opgegeven door de klant.

## <a name="extension-schema"></a>Extensieschema

### <a name="11-public-configuration"></a>1.1 openbare configuratie

Hier vindt u alle ondersteunde configuratie voor een openbare-parameters:

* `FileUri`: (optioneel, string) de uri van het ZIP-bestand voor MOF-bestand/Meta MOF-bestand/aangepaste resource.
* `ResourceName`: (optioneel, string) de naam van de aangepaste resource-module
* `ExtensionAction`: (optioneel, string) geeft aan wat een extensie doet. Geldige waarden: Registreren, pushen, ophalen, installeren, verwijderen. Indien niet opgegeven, wordt deze beschouwd als actie Push-standaard.
* `NodeConfigurationName`: (optioneel, string) de naam van de knooppuntconfiguratie van een om toe te passen.
* `RefreshFrequencyMins`: (optioneel, int) geeft aan hoe vaak (in minuten) DSC probeert te verkrijgen van de configuratie van de pull-server. 
       Als de configuratie op de pull-server van de huidige versie van het doelknooppunt verschilt, wordt het gekopieerd naar de store in behandeling en toegepast.
* `ConfigurationMode`: (optioneel, string) geeft aan hoe de configuratie moet worden toegepast door DSC. Geldige waarden zijn: ApplyOnly, ApplyAndMonitor, ApplyAndAutoCorrect.
* `ConfigurationModeFrequencyMins`: (optioneel, int) geeft aan hoe vaak (in minuten) DSC zorgt ervoor dat de configuratie in de gewenste status.

> [!NOTE]
> Als u een versie < 2.3, is modusparameter hetzelfde als ExtensionAction. Modus lijkt te zijn van een overbelaste term. Daarom de om verwarring te voorkomen, wordt ExtensionAction gebruikt vanaf versie 2.3 en hoger. Voor compatibiliteit met eerdere versies ondersteunt de extensie modus en de ExtensionAction. 
>

### <a name="12-protected-configuration"></a>1.2 beveiligde configuratie

Hier vindt u alle ondersteunde beveiligde configuratieparameters:

* `StorageAccountName`: (optioneel, string) de naam van het opslagaccount waarin het bestand
* `StorageAccountKey`: (optioneel, string) de sleutel van het opslagaccount waarin het bestand
* `RegistrationUrl`: (optioneel, string) de URL van de Azure Automation-account
* `RegistrationKey`: (optioneel, string) de toegangssleutel van het Azure Automation-account


## <a name="scenarios"></a>Scenario's

### <a name="register-to-azure-automation-account"></a>Registreren bij Azure Automation-account
protected.json
```json
{
  "RegistrationUrl": "<azure-automation-account-url>",
  "RegistrationKey": "<azure-automation-account-key>"
}
```
public.json
```json
{
  "ExtensionAction" : "Register",
  "NodeConfigurationName" : "<node-configuration-name>",
  "RefreshFrequencyMins" : "<value>",
  "ConfigurationMode" : "<ApplyAndMonitor | ApplyAndAutoCorrect | ApplyOnly>",
  "ConfigurationModeFrequencyMins" : "<value>"
}
```

PowerShell-indeling
```powershell
$privateConfig = '{
  "RegistrationUrl": "<azure-automation-account-url>",
  "RegistrationKey": "<azure-automation-account-key>"
}'

$publicConfig = '{
  "ExtensionAction" : "Register",
  "NodeConfigurationName": "<node-configuration-name>",
  "RefreshFrequencyMins": "<value>",
  "ConfigurationMode": "<ApplyAndMonitor | ApplyAndAutoCorrect | ApplyOnly>",
  "ConfigurationModeFrequencyMins": "<value>"
}'
```

### <a name="apply-a-mof-configuration-file-in-azure-storage-account-to-the-vm"></a>Een MOF-configuratiebestand (in Azure Storage-Account) van toepassing op de virtuele machine

protected.json
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```

public.json
```json
{
  "FileUri": "<mof-file-uri>",
  "ExtensionAction": "Push"
}
```

PowerShell-indeling
```powershell
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'

$publicConfig = '{
  "FileUri": "<mof-file-uri>",
  "ExtensionAction": "Push"
}'
```


### <a name="apply-a-mof-configuration-file-in-public-storage-to-the-vm"></a>Een MOF-configuratiebestand (in openbare opslag) van toepassing op de virtuele machine

public.json
```json
{
  "FileUri": "<mof-file-uri>"
}
```

PowerShell-indeling
```powershell
$publicConfig = '{
  "FileUri": "<mof-file-uri>"
}'
```

### <a name="apply-a-meta-mof-configuration-file-in-azure-storage-account-to-the-vm"></a>Een meta MOF-configuratiebestand (in Azure Storage-Account) van toepassing op de virtuele machine

protected.json
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```

public.json
```json
{
  "ExtensionAction": "Pull",
  "FileUri": "<meta-mof-file-uri>"
}
```

PowerShell-indeling
```powershell
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'

$publicConfig = '{
  "ExtensionAction": "Pull",
  "FileUri": "<meta-mof-file-uri>"
}'
```

### <a name="apply-a-meta-mof-configuration-file-in-public-storage-to-the-vm"></a>Een meta MOF-configuratiebestand (in openbare opslag) van toepassing op de virtuele machine
public.json
```json
{
  "FileUri": "<meta-mof-file-uri>",
  "ExtensionAction": "Pull"
}
```
PowerShell-indeling
```powershell
$publicConfig = '{
  "FileUri": "<meta-mof-file-uri>",
  "ExtensionAction": "Pull"
}'
```

### <a name="install-a-custom-resource-module-zip-file-in-azure-storage-account-to-the-vm"></a>Een aangepaste resource-module (ZIP-bestand in Azure Storage-Account) installeren op de virtuele machine
protected.json
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```
public.json
```json
{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}
```

PowerShell-indeling
```powershell
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'

$publicConfig = '{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}'
```

### <a name="install-a-custom-resource-module-zip-file-in-public-storage-to-the-vm"></a>Een aangepaste resource-module (ZIP-bestand in de openbare opslag) installeren op de virtuele machine
public.json
```json
{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}
```
PowerShell-indeling
```powershell
$publicConfig = '{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}'
```

### <a name="remove-a-custom-resource-module-from-the-vm"></a>Een aangepaste resource-module uit de virtuele machine verwijderen
public.json
```json
{
  "ResourceName": "<resource-name>",
  "ExtensionAction": "Remove"
}
```
PowerShell-indeling
```powershell
$publicConfig = '{
  "ResourceName": "<resource-name>",
  "ExtensionAction": "Remove"
}'
```

## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. Sjablonen zijn ideaal bij het implementeren van een of meer virtuele machines waarvoor de post-implementatieconfiguratie zoals onboarding voor Azure Automation. 

De Resource Manager-voorbeeldsjabloon is [201-dsc-linux-azure-storage-on-ubuntu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-dsc-linux-azure-storage-on-ubuntu) en [201-dsc-linux-public-storage-on-ubuntu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-dsc-linux-public-storage-on-ubuntu).

Voor meer informatie over Azure Resource Manager-sjabloon, gaat u naar [Authoring Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-authoring-templates.md).


## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

### <a name="21-using-azure-cliazure-cli"></a>2.1. Using [**Azure CLI**][azure-cli]
Voordat u de extensie DSCForLinux implementeert, moet u uw `public.json` en `protected.json`, op basis van de verschillende scenario's in sectie 3.

#### <a name="211-classic"></a>2.1.1. Klassiek
Azure Service Management-modus is een afkorting voor de klassieke modus. U kunt overschakelen naar deze door te voeren:
```
$ azure config mode asm
```

U kunt DSCForLinux extensie implementeren door uit te voeren:
```
$ azure vm extension set <vm-name> DSCForLinux Microsoft.OSTCExtensions <version> \
--private-config-path protected.json --public-config-path public.json
```

Meer informatie over de nieuwste extensie-versie beschikbaar is, voert u de volgende uit:
```
$ azure vm extension list
```

#### <a name="212-resource-manager"></a>2.1.2. Resource Manager
U kunt overschakelen naar Azure Resource Manager-modus door uit te voeren:
```
$ azure config mode arm
```

U kunt DSCForLinux extensie implementeren door uit te voeren:
```
$ azure vm extension set <resource-group> <vm-name> \
DSCForLinux Microsoft.OSTCExtensions <version> \
--private-config-path protected.json --public-config-path public.json
```
> [!NOTE]
> In de Azure Resource Manager-modus, `azure vm extension list` is niet beschikbaar voor deze oefening.
>

### <a name="22-using-azure-powershellazure-powershell"></a>2.2. Met behulp van [**Azure PowerShell**] [azure powershell]

#### <a name="221-classic"></a>2.2.1 klassiek

U kunt zich aanmelden bij uw Azure-account (Azure Service Management-modus) door te voeren:

```powershell>
Add-AzureAccount
```

En DSCForLinux extensie implementeren door uit te voeren:

```powershell>
$vmname = '<vm-name>'
$vm = Get-AzureVM -ServiceName $vmname -Name $vmname
$extensionName = 'DSCForLinux'
$publisher = 'Microsoft.OSTCExtensions'
$version = '< version>'
```

U moet de inhoud van de $privateConfig en $publicConfig op basis van verschillende scenario's in bovenstaande sectie wijzigen 
```
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'
```

```
$publicConfig = '{
  "ExtensionAction": "Push",
  "FileUri": "<mof-file-uri>"
}'
```

```
Set-AzureVMExtension -ExtensionName $extensionName -VM $vm -Publisher $publisher `
  -Version $version -PrivateConfiguration $privateConfig `
  -PublicConfiguration $publicConfig | Update-AzureVM
```

#### <a name="222resource-manager"></a>2.2.2.Resource Manager

U kunt zich aanmelden bij uw Azure-account (Azure Resource Manager-modus) door te voeren:

```powershell>
Login-AzureRmAccount
```

Klik op [ **hier** ](../../azure-resource-manager/powershell-azure-resource-manager.md) voor meer informatie over het gebruik van Azure PowerShell met Azure Resource Manager.

U kunt DSCForLinux extensie implementeren door uit te voeren:

```powershell>
$rgName = '<resource-group-name>'
$vmName = '<vm-name>'
$location = '< location>'
$extensionName = 'DSCForLinux'
$publisher = 'Microsoft.OSTCExtensions'
$version = '< version>'
```

U moet de inhoud van de $privateConfig en $publicConfig op basis van verschillende scenario's in bovenstaande sectie wijzigen 
```
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'
```

```
$publicConfig = '{
  "ExtensionAction": "Push",
  "FileUri": "<mof-file-uri>"
}'
```

```
Set-AzureRmVMExtension -ResourceGroupName $rgName -VMName $vmName -Location $location `
  -Name $extensionName -Publisher $publisher -ExtensionType $extensionName `
  -TypeHandlerVersion $version -SettingString $publicConfig -ProtectedSettingString $privateConfig
```

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van de Azure CLI. Als wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, voert u de volgende opdracht uit met de Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Extensie uitvoering uitvoer wordt vastgelegd in het volgende bestand:

```
/var/log/azure/<extension-name>/<version>/extension.log file.
```

Foutcode: 51 voorstelt niet-ondersteunde distributie of niet-ondersteunde extensie actie.
In sommige gevallen bestaat DSC-Linux-uitbreiding niet voor het installeren van OMI wanneer hogere versie van OMI al is in de machine. [foutbericht: (000003) De Downgrade is niet toegestaan]



### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/community/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over extensies [extensies en functies voor Linux virtuele machines](features-linux.md).
