---
title: Desired State Configuration voor een overzicht van Azure | Microsoft Docs
description: Informatie over het gebruik van de Microsoft Azure-extensie-handler voor PowerShell Desired State Configuration (DSC). Het artikel bevat vereisten, architectuur en cmdlets.
services: virtual-machines-windows
documentationcenter: ''
author: mgreenegit
manager: jeconnoc
editor: ''
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
ms.openlocfilehash: b6bfe48df685952d2b465d9549e2f1c086c1c490
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Inleiding tot de handler van de extensie Azure Desired State Configuration

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

De Azure VM-Agent en de bijbehorende extensies uitmaken deel van Microsoft Azure-infrastructuurservices. VM-extensies zijn softwareonderdelen die VM-functionaliteit uitbreiden en verschillende bewerkingen VM vereenvoudigen.

De primaire gebruiksvoorbeeld voor de extensie Azure Desired State Configuration (DSC) wordt bootstrap van een virtuele machine naar de [Azure Automation DSC-service](../../automation/automation-dsc-overview.md). Een virtuele machine uitvoeren van de bootstrap biedt [voordelen](https://docs.microsoft.com/powershell/dsc/metaconfig#pull-service) die voortdurend beheer van de VM-configuratie en integratie met andere operationele's, zoals Azure-bewaking bevatten.

U kunt de DSC-uitbreiding onafhankelijk van de service Automation DSC. Dit omvat echter een enkelvoud actie op die tijdens de implementatie plaatsvindt. Er zijn geen actieve rapportage of Configuratiebeheer is beschikbaar, anders dan lokaal in de virtuele machine.

In dit artikel bevat informatie over beide scenario's: met behulp van de DSC-uitbreiding voor het voorbereiden van Automation en het gebruik van de DSC-extensie als een hulpprogramma voor configuraties toewijzen aan virtuele machines met behulp van de Azure SDK.

## <a name="prerequisites"></a>Vereisten

- **Lokale machine**: om te kunnen communiceren met de Azure VM-extensie, moet u de Azure-portal of de SDK van Azure PowerShell gebruiken.
- **Gastagent**: de virtuele machine van Azure die geconfigureerd door de DSC-configuratie moet een besturingssysteem dat ondersteuning biedt voor Windows Management Framework (WMF) 4.0 of hoger. Zie voor een volledige lijst met ondersteunde versies van het besturingssysteem, de [versiegeschiedenis van DSC-uitbreiding](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Termen en begrippen

Deze handleiding wordt ervan uitgegaan dat bekend bent met de volgende concepten:

- **Configuratie**: een DSC-configuratie-document.
- **Knooppunt**: een doel voor een DSC-configuratie. In dit document, *knooppunt* altijd verwijst naar een Azure VM.
- **Configuratiegegevens**: een .psd1-bestand dat milieu gegevens voor een configuratie bevat.

## <a name="architecture"></a>Architectuur

De extensie Azure DSC maakt gebruik van het framework Azure VM-Agent te leveren, nemen en een rapport over DSC-configuraties die zijn uitgevoerd op Azure Virtual machines. De DSC-uitbreiding accepteert een configuratie-document en een aantal parameters. Als geen bestand is opgegeven, een [standaard configuratiescript](#default-configuration-script) met de extensie is ingesloten. Een script voor de configuratie van de standaard wordt alleen gebruikt om het instellen van metagegevens in [Local Configuration Manager](https://docs.microsoft.com/powershell/dsc/metaconfig).

Als de extensie voor het eerst wordt aangeroepen, wordt een versie van WMF geïnstalleerd met behulp van de volgende logica:

* Als het besturingssysteem van de Azure-virtuele machine Windows Server 2016 is, wordt geen actie ondernomen. Windows Server 2016 heeft al de meest recente versie van PowerShell is geïnstalleerd.
* Als de **wmfVersion** eigenschap is opgegeven, die versie van WMF is geïnstalleerd, tenzij u die versie is niet compatibel met het besturingssysteem van de VM.
* Als er geen **wmfVersion** eigenschap is opgegeven, wordt de meest recente toepasselijke versie van WMF is geïnstalleerd.

Installeren van WMF opnieuw worden opgestart. Start opnieuw op en de uitbreiding downloadt het ZIP-bestand dat opgegeven in de **modulesUrl** eigenschap als opgegeven. Als deze locatie zich in Azure Blob-opslag, kunt u een SAS-token in de **sasToken** eigenschap voor toegang tot het bestand. Nadat de .zip is gedownload en uitgepakt, de configuratie-functie moet worden gedefinieerd **configurationFunction** wordt uitgevoerd om een MOF-bestand te genereren. De extensie voert **Start DscConfiguration-Force** met behulp van het gegenereerde MOF-bestand. De extensie uitvoer vastgelegd en schrijft deze naar de status van de Azure-kanaal.

### <a name="default-configuration-script"></a>Standaard-configuratiescript

De extensie Azure DSC bevat een standaard-configuratiescript die is bedoeld om te worden gebruikt wanneer u met het voorbereiden een virtuele machine met de service Azure Automation DSC. De scriptparameters zijn uitgelijnd met de configureerbare eigenschappen van [Local Configuration Manager](https://docs.microsoft.com/powershell/dsc/metaconfig). Zie voor scriptparameters [standaard configuratiescript](extensions-dsc-template.md#default-configuration-script) in [Desired State Configuration-extensie met Azure Resource Manager-sjablonen](extensions-dsc-template.md). Zie voor het volledige script de [Azure quickstart-sjabloon in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip?raw=true).

## <a name="dsc-extension-in-resource-manager-templates"></a>DSC-extensie in de Resource Manager-sjablonen

In de meeste gevallen zijn Resource Manager-implementatiesjablonen van de verwachte manier om te werken met de DSC-extensie. Zie voor meer informatie en voorbeelden van het opnemen van de DSC-uitbreiding in de Resource Manager-implementatiesjablonen [Desired State Configuration-extensie met Azure Resource Manager-sjablonen](extensions-dsc-template.md).

## <a name="dsc-extension-powershell-cmdlets"></a>DSC-extensie PowerShell-cmdlets

De PowerShell-cmdlets die worden gebruikt voor het beheren van de DSC-uitbreiding zijn best gebruiken in interactieve probleemoplossing en scenario's de gegevens te verzamelen. De cmdlets kunt u het pakket, publiceren en DSC-extensie-implementaties controleren. Houd er rekening mee dat-cmdlets voor de DSC-uitbreiding nog niet zijn bijgewerkt om te werken met de [configuratiescript standaard](#default-configuration-script).

De **publiceren AzureRMVMDscConfiguration** cmdlet duurt in een configuratiebestand voor afhankelijke DSC-resources wordt gescand en maakt vervolgens een ZIP-bestand. Het ZIP-bestand bevat de configuratie en DSC-resources die nodig zijn voor het nemen van de configuratie. De cmdlet kunt ook het pakket maken lokaal met behulp van de *- OutputArchivePath* parameter. Anders wordt de cmdlet publiceert van het ZIP-bestand naar de blob storage en beveiligt het met een SAS-token.

Het configuratiescript .ps1 die de cmdlet maakt is in het ZIP-bestand in de hoofdmap van de archiefmap. De modulemap wordt geplaatst in de archiefmap in resources.

De **Set AzureRMVMDscExtension** cmdlet injects de instellingen die de PowerShell DSC-uitbreiding vereist in een VM-configuratie-object.

De **Get-AzureRMVMDscExtension** cmdlet haalt de status van de DSC-extensie van een specifieke virtuele machine.

De **Get-AzureRMVMDscExtensionStatus** cmdlet haalt de status van de DSC-configuratie die wordt gepubliceerd door de handler van DSC-uitbreiding. Deze actie kan worden uitgevoerd op een enkele virtuele machine of op een groep VM's.

De **verwijderen AzureRMVMDscExtension** cmdlet verwijdert de extensie-handler van een specifieke virtuele machine. Deze cmdlet biedt *niet* verwijderen van de configuratie, WMF verwijderen of wijzigen van de toegepaste instellingen op de virtuele machine. Alleen de handler voor de uitbreiding wordt verwijderd. 

Belangrijke informatie over cmdlets van Resource Manager DSC-extensie:

- Azure Resource Manager-cmdlets worden synchroon.
- De *ResourceGroupName*, *VMName*, *ArchiveStorageAccountName*, *versie*, en *locatie*parameters zijn vereist.
- *ArchiveResourceGroupName* is een optionele parameter. U kunt deze parameter opgeven wanneer uw storage-account hoort bij een andere resourcegroep dan die waarop de virtuele machine is gemaakt.
- Gebruik de **AutoUpdate** overschakelen naar het automatisch bijwerken van de handler uitbreiding naar de nieuwste versie wanneer deze beschikbaar is. Deze parameter heeft als opnieuw opstarten op de virtuele machine wanneer een nieuwe versie van WMF veroorzaken dat wordt uitgegeven.

### <a name="get-started-with-cmdlets"></a>Aan de slag met cmdlets

De extensie Azure DSC kunt documenten van DSC-configuratie gebruiken om rechtstreeks Azure VM's configureren tijdens de implementatie. Deze stap registreren niet het knooppunt om automatisering te. Het knooppunt is *niet* centraal beheerde.

Het volgende voorbeeld toont een eenvoudig voorbeeld van een configuratie. De configuratie van de lokaal opslaan als IisInstall.ps1.

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

De volgende opdrachten wordt het script IisInstall.ps1 plaatsen op de opgegeven virtuele machine. De opdrachten uitvoeren ook de configuratie en vervolgens rapporteren over de status.

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script to user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.72 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"
```

## <a name="azure-portal-functionality"></a>Azure portal-functionaliteit

DSC instellen in de portal:

1. Ga naar een virtuele machine. 
2. Onder **instellingen** > **algemene**, selecteer **extensies**. 
3. Selecteer in het nieuwe deelvenster dat gemaakt, **toevoegen**, en selecteer vervolgens **PowerShell DSC**.

De volgende invoer is vereist in de portal:

* **Configuratie-Modules of Script**: dit veld is verplicht (het formulier is niet bijgewerkt voor de [standaard configuratiescript](#default-configuration-script)). Configuratie-modules en -scripts moeten een .ps1-bestand met een configuratiescript of een ZIP-bestand met een .ps1-configuratiescript in de hoofdmap. Als u een ZIP-bestand gebruikt, moeten alle afhankelijke resources worden opgenomen in de mappen in het ZIP-module. U kunt het ZIP-bestand maken met behulp van de **publiceren AzureVMDscConfiguration - OutputArchivePath** opgenomen in de SDK van Azure PowerShell-cmdlet. Het ZIP-bestand is geüpload naar de gebruiker blob-opslag en beveiligd door een SAS-token.

* **Bestand met configuratiegegevens PSD1**: dit veld is optioneel. Als uw configuratie een configuratiebestand voor de gegevens in .psd1 vereist, moet u dit veld gebruiken om te selecteren van het gegevensveld en uploaden naar de gebruiker blob-opslag. Het configuratiebestand van de gegevens wordt beveiligd door een SAS-token in de blob-opslag.

* **Naam van configuratie Modulegekwalificeerde**: U kunt meerdere configuratie-functies opnemen in een .ps1-bestand. Voer de naam van het configuratiescript .ps1 gevolgd door \\ en de naam van de configuratie-functie. Bijvoorbeeld, als het script .ps1 de naam configuration.ps1 en de configuratie heeft is **IisInstall**, voer **configuration.ps1\IisInstall**.

* **Configuratie argumenten**: als de functie configuratie argumenten aanneemt, voert u deze hier in de indeling **argumentName1 = value1, argumentName2 = value2**. Dit is een andere indeling waarin configuratie-argumenten in PowerShell-cmdlets of de Resource Manager-sjablonen worden geaccepteerd.

## <a name="logs"></a>Logboeken
Plaats logboeken op deze locatie:

```powerShell
C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\<version number>
```

## <a name="next-steps"></a>Volgende stappen
* Voor meer informatie over PowerShell DSC, gaat u naar de [PowerShell-documentatiecentrum](https://msdn.microsoft.com/powershell/dsc/overview). 
* Bekijk de [Resource Manager-sjabloon voor de uitbreiding van DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
* Ga voor meer functionaliteit die u beheren kunt met behulp van PowerShell DSC, en voor meer DSC-resources, het [PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).
* Zie voor meer informatie over gevoelige parameters doorgeven in configuraties [beheren van referenties veilig met de DSC-uitbreiding handler](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

