---
title: Desired State Configuration voor een overzicht van Azure
description: Informatie over het gebruik van de Microsoft Azure-extensie-handler voor PowerShell Desired State Configuration (DSC). Het artikel bevat vereisten, architectuur en -cmdlets.
services: virtual-machines-windows
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: dsc
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: robreed
ms.openlocfilehash: 18d6478763fd6551cc8baac6ea54e8d91f1a28e6
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45629965"
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Inleiding tot de handler Azure Desired State Configuration-extensie

De Azure VM-Agent en de bijbehorende extensies zijn onderdeel van Microsoft Azure infrastructuurservices. VM-extensies zijn software-onderdelen die breiden de functionaliteit van de virtuele machine en verschillende bewerkingen voor VM-beheer te vereenvoudigen.

De primaire use-case voor de extensie Azure Desired State Configuration (DSC) is een virtuele machine opstarten de [service van Azure Automation DSC](../../automation/automation-dsc-overview.md). Opstarten van een virtuele machine biedt [voordelen](/powershell/dsc/metaconfig#pull-service) die voortdurend beheer van de VM-configuratie en integratie met andere operationele's, zoals Azure Monitoring bevatten.

U kunt de DSC-extensie onafhankelijk van de service Automation DSC kunt gebruiken. Dit omvat echter een enkel actie op die tijdens de implementatie plaatsvindt. Er zijn geen doorlopende rapport of Configuratiebeheer is beschikbaar, anders dan lokaal in de virtuele machine.

In dit artikel bevat informatie over beide scenario's: de DSC-extensie gebruiken voor het voorbereiden van Automation en de DSC-extensie gebruiken als een hulpprogramma voor het toewijzen van configuraties aan virtuele machines met behulp van de Azure SDK.

## <a name="prerequisites"></a>Vereisten

- **Lokale computer**: om te communiceren met de Azure VM-extensie, moet u de Azure-portal of de SDK van Azure PowerShell gebruiken.
- **Gastagent**: de Azure-VM die geconfigureerd door de DSC-configuratie moet een besturingssysteem dat ondersteuning biedt voor Windows Management Framework (WMF) 4.0 of hoger. Zie voor de volledige lijst van ondersteunde versies van het besturingssysteem, de [versiegeschiedenis van DSC-extensie](/powershell/dsc/azuredscexthistory).

## <a name="terms-and-concepts"></a>Termen en begrippen

Deze handleiding wordt ervan uitgegaan dat bekend zijn met de volgende concepten:

- **Configuratie**: een DSC-configuratie-document.
- **Knooppunt**: een doel voor een DSC-configuratie. In dit document, *knooppunt* altijd verwijst naar een Azure-VM.
- **Configuratiegegevens**: een .psd1-bestand met uitwerking gegevens voor een configuratie.

## <a name="architecture"></a>Architectuur

De Azure-DSC-extensie gebruikmaakt van het framework van de Azure VM-Agent te leveren, gerapporteerd en rapporteren van DSC-configuraties die worden uitgevoerd op virtuele Azure-machines. De DSC-extensie accepteert een configuratiedocument en een aantal parameters. Als er geen bestand is opgegeven, een [standaard configuratiescript](#default-configuration-script) met de extensie is ingesloten. Een script voor de configuratie van de standaard wordt alleen gebruikt om het instellen van metagegevens in [Local Configuration Manager](/powershell/dsc/metaconfig).

Als de extensie voor het eerst wordt aangeroepen, wordt een versie van WMF geïnstalleerd met behulp van de volgende logica:

- Als de Azure VM-besturingssysteem Windows Server 2016 is, wordt geen actie ondernomen. Windows Server 2016 is al de meest recente versie van PowerShell is geïnstalleerd.
- Als de **wmfVersion** eigenschap is opgegeven, die versie van WMF is geïnstalleerd, tenzij die versie is niet compatibel met het besturingssysteem van de virtuele machine.
- Als er geen **wmfVersion** eigenschap is opgegeven, wordt de meest recente toepasselijke versie van WMF is geïnstalleerd.

Installeren van WMF moet opnieuw worden opgestart. Na het opnieuw is opgestart, worden de extensie gedownload het ZIP-bestand dat opgegeven in de **modulesUrl** eigenschap, indien opgegeven. Als deze locatie zich in Azure Blob-opslag, kunt u een SAS-token in de **sasToken** eigenschap voor toegang tot het bestand. Nadat de ZIP is gedownload en uitgepakt, de configuratie van de functie moet worden gedefinieerd in **configurationFunction** wordt uitgevoerd voor het genereren van een MOF-bestand. De extensie vervolgens uitgevoerd `Start-DscConfiguration -Force` met behulp van het gegenereerde MOF-bestand. De extensie wordt uitvoer vastgelegd en schrijft deze naar de status van Azure-kanaal.

### <a name="default-configuration-script"></a>Standaard-configuratiescript

De Azure-DSC-extensie bevat een standaard-configuratiescript die is bedoeld om te worden gebruikt wanneer u met het voorbereiden een virtuele machine met de service Azure Automation DSC. De scriptparameters zijn uitgelijnd met de configureerbare eigenschappen van [Local Configuration Manager](/powershell/dsc/metaconfig). Zie voor de scriptparameters, [standaard configuratiescript](dsc-template.md#default-configuration-script) in [Desired State Configuration-extensie met Azure Resource Manager-sjablonen](dsc-template.md). Zie voor het volledige script de [Azure quickstart-sjabloon in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip?raw=true).

## <a name="dsc-extension-in-resource-manager-templates"></a>DSC-extensie in Resource Manager-sjablonen

In de meeste gevallen zijn sjablonen van Resource Manager-implementatie de verwachte manier om te werken met de DSC-extensie. Zie voor meer informatie en voorbeelden van de DSC-extensie opnemen in Resource Manager-sjablonen voor implementatie, [Desired State Configuration-extensie met Azure Resource Manager-sjablonen](dsc-template.md).

## <a name="dsc-extension-powershell-cmdlets"></a>DSC-extensie PowerShell-cmdlets

De PowerShell-cmdlets die worden gebruikt voor het beheren van de DSC-extensie worden best gebruikt in de interactieve oplossen van problemen en scenario's voor verzamelen van informatie. U kunt de cmdlets kunnen worden verpakt, publiceren en DSC-extensie-implementaties controleren. Cmdlets voor de DSC-extensie nog niet zijn bijgewerkt om te werken met de [configuratiescript standaard](#default-configuration-script).

De **publiceren AzureRmVMDscConfiguration** cmdlet maakt in een configuratiebestand, scant deze voor afhankelijke DSC-resources en maakt vervolgens een ZIP-bestand. Het ZIP-bestand bevat de configuratie en DSC-resources die nodig zijn voor de configuratie wordt gerapporteerd. De cmdlet Maak ook het pakket lokaal met behulp van de *- OutputArchivePath* parameter. Anders wordt de cmdlet publiceert het ZIP-bestand voor de blob-opslag en beveiligt het met een SAS-token.

Het configuratiescript .ps1 die de cmdlet maakt is in het ZIP-bestand in de hoofdmap van de archiefmap. De modulemap wordt geplaatst in de archiefmap van resources.

De **Set AzureRmVMDscExtension** cmdlet injects de instellingen die de PowerShell DSC-extensie is vereist in een VM-configuratieobject.

De **Get-AzureRmVMDscExtension** cmdlet haalt de status van de DSC-extensie van een specifieke virtuele machine.

De **Get-AzureRmVMDscExtensionStatus** cmdlet wordt de status van de DSC-configuratie die wordt gepubliceerd door de handler voor DSC-extensie opgehaald. Deze actie kan worden uitgevoerd op een enkele virtuele machine of op een groep virtuele machines.

De **Remove-AzureRmVMDscExtension** cmdlet wordt de extensie-handler van een specifieke virtuele machine. Deze cmdlet biedt *niet* verwijderen van de configuratie, WMF verwijderen of wijzigen van de toegepaste instellingen op de virtuele machine. Alleen de handler extensie wordt verwijderd. 

Belangrijke informatie over het Resource Manager-DSC-extensie-cmdlets:

- Azure Resource Manager-cmdlets worden synchroon.
- De *ResourceGroupName*, *VMName*, *ArchiveStorageAccountName*, *versie*, en *locatie*parameters zijn alle vereiste.
- *ArchiveResourceGroupName* is een optionele parameter. U kunt deze parameter wanneer uw storage-account behoort tot een andere resourcegroep dan de versie waarin de virtuele machine wordt gemaakt.
- Gebruik de **AutoUpdate** overschakelen naar de extensie-handler automatisch wordt bijgewerkt naar de meest recente versie wanneer deze beschikbaar is. Deze parameter heeft de mogelijkheid ervoor zorgen dat opnieuw wordt gestart op de virtuele machine wanneer er een nieuwe versie van WMF wordt uitgebracht.

### <a name="get-started-with-cmdlets"></a>Aan de slag met cmdlets

De Azure-DSC-extensie kunt documenten voor DSC-configuratie gebruiken om Azure-VM's rechtstreeks configureren tijdens de implementatie. Deze stap registreren niet het knooppunt om Automation te. Het knooppunt is *niet* centraal beheerd.

Het volgende voorbeeld ziet een eenvoudig voorbeeld van een configuratie. De configuratie van de lokaal opslaan als IisInstall.ps1.

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

De volgende opdrachten wordt het script IisInstall.ps1 plaats op de opgegeven virtuele machine. De opdrachten uitgevoerd ook de configuratie en vervolgens rapporteren over de status.

```powershell
$resourceGroup = 'dscVmDemo'
$location = 'westus'
$vmName = 'myVM'
$storageName = 'demostorage'
#Publish the configuration script to user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVMDscExtension -Version '2.76' -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName 'iisInstall.ps1.zip' -AutoUpdate $true -ConfigurationName 'IISInstall'
```

## <a name="azure-portal-functionality"></a>Functionaliteit van Azure portal

DSC instellen in de portal:

1. Ga naar een virtuele machine.
2. Selecteer onder **Instellingen** de optie **Extensies**.
3. Selecteer in de nieuwe pagina die wordt gemaakt, **+ toevoegen**, en selecteer vervolgens **PowerShell Desired State Configuration**.
4. Klik op **maken** onderaan de pagina van de gegevens van extensie.

De portal worden de volgende invoer verzameld:

- **Configuratie van Modules of Script**: dit veld is verplicht (het formulier is niet bijgewerkt voor de [standaard configuratiescript](#default-configuration-script)). Configuratie-modules en -scripts vereist een .ps1-bestand met een configuratiescript of een ZIP-bestand met een .ps1-configuratiescript in de hoofdmap. Als u een ZIP-bestand gebruikt, moeten alle afhankelijke resources worden opgenomen in de mappen in het ZIP-module. U kunt het ZIP-bestand maken met behulp van de **publiceren AzureVMDscConfiguration - OutputArchivePath** opgenomen in de SDK van Azure PowerShell-cmdlet. Het ZIP-bestand is geüpload naar uw blob-opslag van gebruiker en beveiligd door een SAS-token.

- **Naam van de configuratie modulegekwalificeerde**: U kunt meerdere configuratiefuncties opnemen in een .ps1-bestand. Voer de naam van het configuratiescript .ps1 gevolgd door \\ en de naam van de configuratie-functie. Bijvoorbeeld, als uw .ps1-script de naam configuration.ps1 en de configuratie heeft is **IisInstall**, voer **configuration.ps1\IisInstall**.

- **Configuratie van argumenten**: als de functie configuratie argumenten kunt gebruiken, voert u deze hier in de notatie **argumentName1 = value1, argumentName2 = value2**. Deze indeling is een andere indeling waarin de configuratie argumenten in PowerShell-cmdlets of Resource Manager-sjablonen worden geaccepteerd.

- **PSD1 Configuratiegegevensbestand**: dit veld is optioneel. Als uw configuratie een configuratiebestand van de gegevens in het .psd1 vereist, moet u dit veld gebruiken voor het selecteren van het gegevensveld en uploaden naar de gebruiker blob-opslag. Het configuratiebestand van de gegevens wordt beveiligd door een SAS-token in blob-opslag.

- **Versie van WMF**: Hiermee geeft u de versie van Windows Management Framework (WMF) die moet worden geïnstalleerd op de virtuele machine. Als u deze eigenschap instelt op nieuwste installeert de meest recente versie van WMF. Op dit moment de enige mogelijke waarden voor deze eigenschap worden 4.0, 5.0, 5.1, en de nieuwste. Deze mogelijke waarden zijn afhankelijk van updates. De standaardwaarde is **nieuwste**.

- **Het verzamelen van gegevens**: Hiermee bepaalt u als de extensie de telemetrie verzamelt. Zie voor meer informatie, [Azure DSC-extensie gegevensverzameling](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/).

- **Versie**: Hiermee geeft u de versie van de DSC-extensie te installeren. Zie voor meer informatie over versies [versiegeschiedenis van DSC-extensie](/powershell/dsc/azuredscexthistory).

- **Automatische Upgrade van secundaire versie**: dit veld wordt toegewezen aan de **AutoUpdate** -switch in de cmdlets en de extensie voor het automatisch wordt bijgewerkt naar de meest recente versie tijdens de installatie maakt. **Ja** instructies voor gebruik van de meest recente beschikbare versie van de handler extensie en **Nee** forceert de **versie** opgegeven om te worden geïnstalleerd. Geen van beide selecteren **Ja** noch **Nee** is hetzelfde als het selecteren van **Nee**.

## <a name="logs"></a>Logboeken

Logboeken voor de extensie worden opgeslagen in de volgende locatie: `C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\<version number>`

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over PowerShell DSC, gaat u naar de [PowerShell-documentatiecentrum](/powershell/dsc/overview).
- Bekijk de [Resource Manager-sjabloon voor de DSC-extensie](dsc-template.md).
- Ga voor meer functionaliteit die u beheren kunt met behulp van PowerShell DSC, evenals meer DSC-resources, het [PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).
- Zie voor meer informatie over gevoelige parameters doorgeven in configuraties [beheren van referenties veilig met de DSC-extensie-handler](dsc-credentials.md).