---
title: Desired State Configuration voor een overzicht van Azure | Microsoft Docs
description: Overzicht voor het gebruik van de Microsoft Azure-extensie-handler voor PowerShell Desired State Configuration. Vereisten, architectuur, cmdlets zoals..
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/09/2017
ms.author: zachal
ms.openlocfilehash: c05c2d541a5f526f362f9cd72fe6d878374112b6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Inleiding tot de handler van de extensie Azure Desired State Configuration
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

De Azure VM-Agent en de bijbehorende extensies uitmaken deel van de Microsoft Azure Infrastructure Services. VM-extensies zijn software-onderdelen die de VM-functionaliteit uitbreiden en verschillende bewerkingen VM vereenvoudigen. Bijvoorbeeld, de VMAccess-extensie kan worden gebruikt om een administrator wachtwoord opnieuw instellen of de extensie voor aangepaste scripts kan worden gebruikt voor het uitvoeren van een script op de virtuele machine.

Dit artikel bevat de extensie PowerShell Desired State Configuration (DSC) voor Azure Virtual machines als onderdeel van de SDK van Azure PowerShell. U kunt nieuwe cmdlets gebruiken om te uploaden en een PowerShell DSC-configuratie toepassen op een Azure-VM met de PowerShell DSC-extensie is ingeschakeld. De PowerShell DSC-uitbreiding aanroepen voor PowerShell DSC op te nemen van de ontvangen DSC-configuratie op de virtuele machine. Deze functionaliteit is ook beschikbaar via de Azure-portal.

## <a name="prerequisites"></a>Vereisten
**Lokale machine** voor interactie met de Azure VM-extensie, moet u de Azure portal of de SDK van Azure PowerShell gebruiken. 

**Gastagent** de virtuele machine van Azure die is geconfigureerd door de DSC-configuratie moet worden van een besturingssysteem dat ondersteuning biedt voor Windows Management Framework (WMF) 4.0 ofwel 5.0. De volledige lijst met ondersteunde versies van het besturingssysteem kan worden gevonden op de [versiegeschiedenis van DSC-uitbreiding](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Termen en begrippen
Deze handleiding wordt ervan uitgegaan bekend bent met de volgende concepten:

Configuratie: een document DSC-configuratie. 

Knooppunt - doel voor een DSC-configuratie. In dit document verwijst 'knooppunt' altijd naar een Azure VM.

Configuratie - een .psd1 gegevensbestand met milieu gegevens voor een configuratie

## <a name="architectural-overview"></a>Overzicht van architectuur
De extensie Azure DSC maakt gebruik van het framework Azure VM-Agent te leveren, nemen en een rapport over DSC-configuraties die zijn uitgevoerd op Azure Virtual machines. De DSC-extensie verwacht een ZIP-bestand met ten minste een configuratie-document en een set parameters die worden geleverd via de SDK van Azure PowerShell of via de Azure-portal.

Als de extensie voor het eerst wordt aangeroepen, voert deze een installatieproces. Dit proces wordt een versie van de Windows Management Framework (WMF) met behulp van de volgende logica geïnstalleerd:

1. Als het besturingssysteem van de Azure-virtuele machine Windows Server 2016 is, wordt geen actie ondernomen. Windows Server 2016 heeft al de meest recente versie van PowerShell is geïnstalleerd.
2. Als de `wmfVersion` eigenschap is opgegeven, die versie van de WMF is geïnstalleerd, tenzij deze is niet compatibel met het besturingssysteem van de VM.
3. Als er geen `wmfVersion` eigenschap is opgegeven, wordt de meest recente toepasselijke versie van de WMF is geïnstalleerd.

Installatie van de WMF moet opnieuw worden opgestart. Na opnieuw opstarten, downloadt u de extensie het ZIP-bestand dat is opgegeven in de `modulesUrl` eigenschap. Als deze locatie zich in Azure blob-opslag, een SAS-token kan worden opgegeven in de `sasToken` eigenschap voor toegang tot het bestand. Nadat de .zip is gedownload en uitgepakt, de configuratie-functie moet worden gedefinieerd `configurationFunction` wordt uitgevoerd voor het genereren van het MOF-bestand. De extensie voert `Start-DscConfiguration -Force` op het gegenereerde MOF-bestand. De uitbreiding wordt vastgelegd uitvoer- en schrijfbewerkingen naar het Azure Status kanaal weer uit. Vanaf dit punt verwerkt de DSC-LCM bewaking en correctie als normaal. 

## <a name="powershell-cmdlets"></a>PowerShell-cmdlets
PowerShell-cmdlets kunnen worden gebruikt met Azure Resource Manager of het klassieke implementatiemodel van het pakket, publiceren en DSC-extensie-implementaties controleren. De volgende cmdlets die worden vermeld, zijn de modules voor klassieke implementatie, maar 'Azure' kan worden vervangen door "AzureRm" het Azure Resource Manager-model gebruiken. Bijvoorbeeld: `Publish-AzureVMDscConfiguration` maakt gebruik van het klassieke implementatiemodel waar `Publish-AzureRmVMDscConfiguration` maakt gebruik van Azure Resource Manager. 

`Publish-AzureVMDscConfiguration`wordt in een configuratiebestand, voor afhankelijke DSC-resources wordt gescand en maakt een ZIP-bestand met de configuratie en DSC-resources die nodig zijn voor het nemen van de configuratie. Maak ook het pakket lokaal via de `-ConfigurationArchivePath` parameter. Anders wordt het ZIP-bestand wordt gepubliceerd naar Azure blob storage en beveiligt het met een SAS-token.

Het ZIP-bestand gemaakt met deze cmdlet heeft een script voor de configuratie van de .ps1 in de hoofdmap van de archiefmap. Bronnen hebben de modulemap die in de archiefmap geplaatst. 

`Set-AzureVMDscExtension`de instellingen die nodig zijn voor de PowerShell DSC-uitbreiding in een VM-configuratie-object injects. In het klassieke implementatiemodel, de wijzigingen van de virtuele machine moeten worden toegepast op een Azure-VM met `Update-AzureVM`. 

`Get-AzureVMDscExtension`Hiermee wordt de status van de DSC-extensie van een bepaalde virtuele machine opgehaald. 

`Get-AzureVMDscExtensionStatus`haalt de status van de DSC-configuratie door de handler van DSC-uitbreiding van kracht. Deze actie kan worden uitgevoerd op een enkele virtuele machine of een groep VM's.

`Remove-AzureVMDscExtension`Hiermee verwijdert u de extensie-handler van een bepaalde virtuele machine. Deze cmdlet biedt **niet** verwijderen van de configuratie, de WMF verwijderen of wijzigen van de toegepaste instellingen op de virtuele machine. Alleen de handler voor de uitbreiding wordt verwijderd. 

**Belangrijke verschillen in ASM en Azure Resource Manager-cmdlets**

* Azure Resource Manager-cmdlets worden synchroon. ASM-cmdlets zijn asynchroon.
* ResourceGroupName, VMName ArchiveStorageAccountName, versie en locatie zijn alle vereiste parameters in Azure Resource Manager.
* ArchiveResourceGroupName is een nieuwe optionele parameter voor Azure Resource Manager. U kunt deze parameter opgeven wanneer uw storage-account hoort bij een andere resourcegroep dan die waarop de virtuele machine is gemaakt.
* ConfigurationArchive wordt ArchiveBlobName aangeroepen in Azure Resource Manager
* ContainerName wordt ArchiveContainerName aangeroepen in Azure Resource Manager
* StorageEndpointSuffix wordt ArchiveStorageEndpointSuffix aangeroepen in Azure Resource Manager
* De schakeloptie AutoUpdate is toegevoegd in Azure Resource Manager om in te schakelen automatisch bijwerken van de handler uitbreiding naar de meest recente versie als en wanneer deze beschikbaar is. Deze parameter heeft veroorzaken opnieuw wordt opgestart op de virtuele machine wanneer er een nieuwe versie van de WMF wordt uitgebracht. 

## <a name="azure-portal-functionality"></a>Azure portal-functionaliteit
Blader naar een virtuele machine. -Onder Instellingen > Algemeen klik "-extensies." Een nieuw deelvenster wordt gemaakt. Klik op "Toevoegen" en selecteer PowerShell DSC.

De portal moet invoer.
**Configuratie-Modules of Script**: dit veld is verplicht. Vereist een ps1-bestand met een configuratiescript of een ZIP-bestand met een .ps1-configuratiescript op de hoofdmap en alle afhankelijke resources in mappen in het ZIP-module. Kan worden gemaakt met de `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` opgenomen in de SDK van Azure PowerShell cmdlet. Het ZIP-bestand is geüpload naar de gebruiker blob-opslag beveiligd door een SAS-token. 

**Bestand met configuratiegegevens PSD1**: dit veld is optioneel. Als uw configuratie een configuratiebestand voor de gegevens in .psd1 vereist, dit veld gebruiken om te selecteren en dit uploaden naar de gebruiker blob-opslag waar deze wordt beveiligd door een SAS-token. 

**Naam van configuratie Modulegekwalificeerde**: .ps1-bestanden kunnen meerdere functies van de configuratie hebben. Voer de naam van het configuratiescript .ps1 gevolgd door een '\' en de naam van de configuratie-functie. Bijvoorbeeld, als uw .ps1-script heeft de naam 'configuration.ps1' en de configuratie is 'IisInstall', dus voert u:`configuration.ps1\IisInstall`

**Configuratie argumenten**: als de functie configuratie argumenten aanneemt, voert u deze hier in de notatie `argumentName1=value1,argumentName2=value2`. Opmerking: deze indeling is een andere indeling dan hoe configuratie argumenten via PowerShell-cmdlets of de Resource Manager-sjablonen worden geaccepteerd. 

## <a name="getting-started"></a>Aan de slag
De extensie Azure DSC duurt in documenten van DSC-configuratie en ze enacts op Azure Virtual machines. Hier volgt een eenvoudig voorbeeld van een configuratie. Dit lokaal opslaan als 'IisInstall.ps1':

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

De volgende stappen uit het script IisInstall.ps1 plaats op de opgegeven virtuele machine, uitvoeren van de configuratie en rapporteren over de status.
###<a name="classic-model"></a>Klassieke model
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "IisInstall.ps1.zip" -StorageContext $storageContext -ConfigurationName "IisInstall" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```
###<a name="azure-resource-manager-model"></a>Azure Resource Manager-model

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.21 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"

```

## <a name="logging"></a>Logboekregistratie
Logboekbestanden worden opgeslagen:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[versienummer]

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over PowerShell DSC [gaat u naar de PowerShell-documentatiecentrum](https://msdn.microsoft.com/powershell/dsc/overview). 

Bekijk de [Azure Resource Manager-sjabloon voor de uitbreiding van DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Aanvullende functionaliteit die u met PowerShell DSC beheren kunt vinden [de PowerShell-galerie bladeren](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) voor meer DSC-resources.

Zie voor meer informatie over gevoelige parameters doorgeven in configuraties [beheren van referenties veilig met de DSC-uitbreiding handler](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

