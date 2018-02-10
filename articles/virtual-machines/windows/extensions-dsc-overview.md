---
title: Desired State Configuration voor een overzicht van Azure | Microsoft Docs
description: Overzicht voor het gebruik van de Microsoft Azure-extensie-handler voor PowerShell Desired State Configuration. Vereisten, architectuur, cmdlets zoals..
services: virtual-machines-windows
documentationcenter: 
author: mgreenegit
manager: timlt
editor: 
tags: azure-resource-manager
keywords: dsc
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 02/02/2018
ms.author: migreene
ms.openlocfilehash: ed8b5bc54baa3a5abfb596b202f0af58e1b6c74f
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Inleiding tot de handler van de extensie Azure Desired State Configuration

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

De Azure VM-Agent en de bijbehorende extensies uitmaken deel van de Microsoft Azure Infrastructure Services.
VM-extensies zijn software-onderdelen die de VM-functionaliteit uitbreiden en verschillende bewerkingen VM vereenvoudigen.

De primaire gebruiksvoorbeeld voor de Desired State Configuration-uitbreiding wordt bootstrap van een virtuele machine naar de [Azure Automation DSC-service](../../automation/automation-dsc-overview.md) biedt [voordelen](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig.md#pull-service) inclusief voortdurend beheer van de VM-configuratie en integratie met andere operationele hulpprogramma's zoals Azure-bewaking.

Het is ook mogelijk om te gebruiken de DSC-uitbreiding onafhankelijk van de Azure Automation DSC-service, maar dit is een enkelvoud actie op die tijdens de implementatie plaatsvindt en er geen actieve reporting of beheer van de configuratie van de andere dan lokaal vanuit de virtuele machine is.

In dit artikel bevat informatie voor scenario's, voor de voorbereiding op Azure Automation DSC-extensie en het gebruik van de DSC-extensie als een hulpprogramma voor het toewijzen van configuraties voor virtuele machines met behulp van de Azure SDK.

## <a name="prerequisites"></a>Vereisten

- **Lokale machine** : om te communiceren met de Azure VM-extensie, moet u de Azure portal of de SDK van Azure PowerShell gebruiken.
- **Gastagent** -de virtuele machine van Azure die is geconfigureerd door de DSC-configuratie moet worden van een besturingssysteem dat ondersteuning biedt voor Windows Management Framework (WMF) 4.0 of hoger. De volledige lijst met ondersteunde versies van het besturingssysteem kan worden gevonden op de pagina [versiegeschiedenis van DSC-uitbreiding](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Termen en begrippen

Deze handleiding wordt ervan uitgegaan bekend bent met de volgende concepten:

- **Configuratie** -document van een DSC-configuratie.
- **Knooppunt** -doel voor een DSC-configuratie. In dit document verwijst 'knooppunt' altijd naar een Azure VM.
- **Configuratiegegevens** : een .psd1 gegevensbestand omgeving voor een configuratie.

## <a name="architectural-overview"></a>Overzicht van architectuur

De extensie Azure DSC maakt gebruik van het framework Azure VM-Agent te leveren, nemen en een rapport over DSC-configuraties die zijn uitgevoerd op Azure Virtual machines.
De DSC-uitbreiding accepteert een configuratie-document en een aantal parameters.
Als geen bestand is opgegeven, een [standaard configuratiescript](###default-configuration-script) is ingesloten met de extensie die alleen gebruikt wordt voor het instellen van metagegevens in [Local Configuration Manager](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig).

Als de extensie voor het eerst wordt aangeroepen, wordt er een versie van de Windows Management Framework (WMF) met behulp van de volgende logica:

1. Als het besturingssysteem van de Azure-virtuele machine Windows Server 2016 is, wordt geen actie ondernomen. Windows Server 2016 heeft al de meest recente versie van PowerShell is geïnstalleerd.
2. Als de `wmfVersion` eigenschap is opgegeven, die versie van de WMF is geïnstalleerd, tenzij deze is niet compatibel met het besturingssysteem van de VM.
3. Als er geen `wmfVersion` eigenschap is opgegeven, wordt de meest recente toepasselijke versie van de WMF is geïnstalleerd.

Installatie van de WMF moet opnieuw worden opgestart.
Na opnieuw opstarten, downloadt u de extensie het ZIP-bestand dat is opgegeven in de `modulesUrl` eigenschap als opgegeven.
Als deze locatie zich in Azure blob-opslag, een SAS-token kan worden opgegeven in de `sasToken` eigenschap voor toegang tot het bestand.
Nadat de .zip is gedownload en uitgepakt, de configuratie-functie moet worden gedefinieerd `configurationFunction` wordt uitgevoerd voor het genereren van een MOF-bestand.
De extensie voert `Start-DscConfiguration -Force` met behulp van de gegenereerde MOF-bestand.
De extensie uitvoer vastgelegd en schrijft deze naar het Azure-Status-kanaal.

### <a name="default-configuration-script"></a>Standaard-configuratiescript

De Azure DSC-uitbreiding bevat een standaardconfiguratie script bedoeld voor gebruik als voorbereiding op een virtuele machine met Azure Automation DSC-service.
De scriptparameters zijn uitgelijnd met de configureerbare eigenschappen van [Local Configuration Manager](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig).
De scriptparameters zijn [gedocumenteerd](extensions-dsc-template.md##default-configuration-script) en het volledige script is beschikbaar in [GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip?raw=true).

## <a name="dsc-extension-in-arm-templates"></a>DSC-extensie in de ARM-sjablonen

Azure Resource Manager (ARM) implementatiesjablonen zijn de verwachte manier om te werken met de DSC-uitbreiding voor de meeste scenario's.
Voor informatie over en voorbeelden van het opnemen van de DSC-uitbreiding in ARM-sjablonen voor implementatie, raadpleegt u de specifieke documentatiepagina [State Configuration-uitbreiding gewenst met Azure Resource Manager-sjablonen](extensions-dsc-template.md).

## <a name="dsc-extension-powershell-cmdlets"></a>PowerShell-Cmdlets voor DSC-uitbreiding

De PowerShell-cmdlets voor het beheren van DSC-extensie worden best voor interactieve probleemoplossing en informatie verzamelen van scenario's gebruikt.
De cmdlets kunnen worden gebruikt voor het pakket, publiceren en DSC-extensie-implementaties controleren.
Houd er rekening mee dat-cmdlets voor DSC-extensie hebben nog niet is bijgewerkt om te werken met de [configuratiescript standaard](###default-configuration-script).

`Publish-AzureRMVMDscConfiguration`wordt in een configuratiebestand, voor afhankelijke DSC-resources wordt gescand en maakt een ZIP-bestand met de configuratie en DSC-resources die nodig zijn voor het nemen van de configuratie.
Maak ook het pakket lokaal via de `-ConfigurationArchivePath` parameter.
Anders wordt het ZIP-bestand wordt gepubliceerd naar Azure blob storage en beveiligt het met een SAS-token.

Het ZIP-bestand gemaakt met deze cmdlet heeft een script voor de configuratie van de .ps1 in de hoofdmap van de archiefmap.
Bronnen hebben de modulemap die in de archiefmap geplaatst.

`Set-AzureRMVMDscExtension`de instellingen die nodig zijn voor de PowerShell DSC-uitbreiding in een VM-configuratie-object injects.

`Get-AzureRMVMDscExtension`Hiermee wordt de status van de DSC-extensie van een bepaalde virtuele machine opgehaald.

`Get-AzureRMVMDscExtensionStatus`haalt de status van de DSC-configuratie door de handler van DSC-uitbreiding van kracht.
Deze actie kan worden uitgevoerd op een enkele virtuele machine of een groep VM's.

`Remove-AzureRMVMDscExtension`Hiermee verwijdert u de extensie-handler van een bepaalde virtuele machine.
Deze cmdlet biedt **niet** verwijderen van de configuratie, de WMF verwijderen of wijzigen van de toegepaste instellingen op de virtuele machine.
Alleen de handler voor de uitbreiding wordt verwijderd. 

Belangrijke informatie over de cmdlets AzureRM DSC-extensie:

- Azure Resource Manager-cmdlets worden synchroon.
- Zijn alle vereiste parameters ResourceGroupName, VMName ArchiveStorageAccountName, versie en locatie.
- ArchiveResourceGroupName is een optionele parameter. U kunt deze parameter opgeven wanneer uw storage-account hoort bij een andere resourcegroep dan die waarop de virtuele machine is gemaakt.
- De AutoUpdate schakelt automatisch bijwerken van de handler uitbreiding naar de meest recente versie als en wanneer deze beschikbaar is. Deze parameter heeft veroorzaken opnieuw wordt opgestart op de virtuele machine wanneer er een nieuwe versie van de WMF wordt uitgebracht.

### <a name="getting-started-with-cmdlets"></a>Aan de slag met Cmdlets

De extensie Azure DSC is geschikt is voor DSC-configuratiedocumenten rechtstreeks naar de Azure VM's configureren tijdens de implementatie, hoewel dit niet het knooppunt voor Azure Automation wordt geregistreerd zodat het knooppunt wordt **niet*-centraal worden beheerd.

Hier volgt een eenvoudig voorbeeld van een configuratie.
Dit lokaal opslaan als 'IisInstall.ps1':

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

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.72 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"
```

## <a name="azure-portal-functionality"></a>Azure Portal-functionaliteit

Blader naar een virtuele machine. -Onder Instellingen > Algemeen klik "-extensies."
Een nieuw deelvenster wordt gemaakt.
Klik op "Toevoegen" en selecteer PowerShell DSC.

De portal moet invoer.
**Configuratie-Modules of Script**: dit veld is verplicht (het formulier is niet bijgewerkt voor de [configuratiescript standaard](###default-configuration-script).
Vereist een ps1-bestand met een configuratiescript of een ZIP-bestand met een .ps1-configuratiescript op de hoofdmap en alle afhankelijke resources in mappen in het ZIP-module.
Kan worden gemaakt met de `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` opgenomen in de SDK van Azure PowerShell cmdlet.
Het ZIP-bestand is geüpload naar de gebruiker blob-opslag beveiligd door een SAS-token.

**Bestand met configuratiegegevens PSD1**: dit veld is optioneel.
Als uw configuratie een configuratiebestand voor de gegevens in .psd1 vereist, dit veld gebruiken om te selecteren en dit uploaden naar de gebruiker blob-opslag waar deze wordt beveiligd door een SAS-token.

**Naam van configuratie Modulegekwalificeerde**: .ps1-bestanden kunnen meerdere functies van de configuratie hebben.
Voer de naam van het configuratiescript .ps1 gevolgd door een '\' en de naam van de configuratie-functie.
Bijvoorbeeld, als uw .ps1-script heeft de naam 'configuration.ps1' en de configuratie is 'IisInstall', dus voert u:`configuration.ps1\IisInstall`

**Configuratie argumenten**: als de functie configuratie argumenten aanneemt, voert u deze hier in de notatie `argumentName1=value1,argumentName2=value2`.
Opmerking: deze indeling is een andere indeling dan hoe configuratie argumenten via PowerShell-cmdlets of de Resource Manager-sjablonen worden geaccepteerd.

## <a name="logging"></a>Logboekregistratie
Logboekbestanden worden opgeslagen:

```powerShell
C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Version Number]
```

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over PowerShell DSC [gaat u naar de PowerShell-documentatiecentrum](https://msdn.microsoft.com/powershell/dsc/overview). 

Bekijk de [Azure Resource Manager-sjabloon voor de uitbreiding van DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Aanvullende functionaliteit die u met PowerShell DSC beheren kunt vinden [de PowerShell-galerie bladeren](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) voor meer DSC-resources.

Zie voor meer informatie over gevoelige parameters doorgeven in configuraties [beheren van referenties veilig met de DSC-uitbreiding handler](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
