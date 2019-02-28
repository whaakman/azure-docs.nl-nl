---
title: Azure Desired State Configuration Extension Handler | Microsoft Docs
description: Uploaden en een PowerShell DSC-configuratie toepassen op een Azure-VM met behulp van DSC-extensie
services: virtual-machines-windows
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: windows
ms.workload: ''
ms.date: 03/26/2018
ms.author: robreed
ms.openlocfilehash: 4f4793e18185c16ef144de33b4f116eff89a9969
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960153"
---
# <a name="powershell-dsc-extension"></a>PowerShell DSC-extensie

## <a name="overview"></a>Overzicht

De PowerShell DSC-extensie voor Windows is gepubliceerd en ondersteund door Microsoft. De extensie wordt geüpload en een PowerShell DSC-configuratie is van toepassing op een Azure-VM. De DSC-extensie aanroepen naar PowerShell DSC op te nemen van de ontvangen DSC-configuratie op de virtuele machine. In dit document worden de ondersteunde platforms, configuraties en implementatie-opties voor de extensie van de DSC-virtuele machine voor Windows.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De DSC-extensie biedt ondersteuning voor het volgende besturingssysteem

Windows Server 2019, Windows Server 2016, Windows Server 2012R2, Windows Server 2012, Windows Server 2008 R2 SP1, Windows Client 7/8.1/10

### <a name="internet-connectivity"></a>Internetconnectiviteit

De DSC-extensie voor Windows is vereist dat de virtuele doelmachine communiceren met Azure en de locatie van de configuratiepakket (.zip-bestand) als deze is opgeslagen op een locatie buiten Azure. 

## <a name="extension-schema"></a>Extensieschema

De volgende JSON ziet u het schema voor het gedeelte instellingen van de DSC-extensie in een Azure Resource Manager-sjabloon. 

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "Microsoft.Powershell.DSC",
  "apiVersion": "2018-10-01",
  "location": "<location>",
  "properties": {
    "publisher": "Microsoft.Powershell",
    "type": "DSC",
    "typeHandlerVersion": "2.77",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "wmfVersion": "latest",
        "configuration": {
            "url": "http://validURLToConfigLocation",
            "script": "ConfigurationScript.ps1",
            "function": "ConfigurationFunction"
        },
        "configurationArguments": {
            "argument1": "Value1",
            "argument2": "Value2"
        },
        "configurationData": {
            "url": "https://foo.psd1"
        },
        "privacy": {
            "dataCollection": "enable"
        },
        "advancedOptions": {
            "forcePullAndApply": false
            "downloadMappings": {
                "specificDependencyKey": "https://myCustomDependencyLocation"
            }
        } 
    },
    "protectedSettings": {
        "configurationArguments": {
            "parameterOfTypePSCredential1": {
                "userName": "UsernameValue1",
                "password": "PasswordValue1"
            },
            "parameterOfTypePSCredential2": {
                "userName": "UsernameValue2",
                "password": "PasswordValue2"
            }
        },
        "configurationUrlSasToken": "?g!bber1sht0k3n",
        "configurationDataUrlSasToken": "?dataAcC355T0k3N"
    }
  }
}
```

### <a name="property-values"></a>Waarden van eigenschappen

| Name | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 10-01-2018 | date |
| Uitgever | Microsoft.Powershell.DSC | string |
| type | DSC | string |
| typeHandlerVersion | 2.77 | int |

### <a name="settings-property-values"></a>Waarden voor de eigenschap instellingen

| Name | Gegevenstype | Description
| ---- | ---- | ---- |
| settings.wmfVersion | string | Hiermee geeft u de versie van Windows Management Framework die moet worden geïnstalleerd op de virtuele machine. Als deze eigenschap ingesteld op 'nieuwste', wordt de meest recente versie van WMF installeren. De alleen huidige mogelijke waarden voor deze eigenschap zijn 4.0, '5.0' en 'nieuwste'. Deze mogelijke waarden zijn afhankelijk van updates. De standaardwaarde is 'nieuwste'. |
| settings.configuration.url | string | Hiermee geeft u de URL-locatie van waaruit het zip-bestand van uw DSC-configuratie gedownload. Als de opgegeven URL is een SAS-token om toegang te krijgen vereist, moet u de eigenschap protectedSettings.configurationUrlSasToken instellen op de waarde van uw SAS-token. Deze eigenschap is vereist als settings.configuration.script en/of settings.configuration.function zijn gedefinieerd.
| settings.configuration.script | string | Hiermee geeft u de bestandsnaam van het script dat de definitie van de DSC-configuratie bevat. Met dit script moet zich in de hoofdmap van het zip-bestand van de URL die is opgegeven door de eigenschap configuration.url gedownload. Deze eigenschap is vereist als settings.configuration.url en/of settings.configuration.script zijn gedefinieerd.
| settings.configuration.function | string | Hiermee geeft u de naam van de DSC-configuratie. De configuratie met de naam moet worden opgenomen in het script dat is gedefinieerd door configuration.script. Deze eigenschap is vereist als settings.configuration.url en/of settings.configuration.function zijn gedefinieerd.
| settings.configurationArguments | Verzameling | Hiermee definieert u de parameters die u wilt doorgeven aan uw DSC-configuratie. Deze eigenschap wordt niet versleuteld.
| settings.configurationData.url | string | Geeft de URL waaruit u uw gegevens (.pds1)-configuratiebestand te downloaden om te gebruiken als invoer voor uw DSC-configuratie. Als de opgegeven URL is een SAS-token om toegang te krijgen vereist, moet u de eigenschap protectedSettings.configurationDataUrlSasToken instellen op de waarde van uw SAS-token.
| settings.privacy.dataEnabled | string | Hiermee of verzamelen van telemetriegegevens uitgeschakeld. De enige mogelijke waarden voor deze eigenschap zijn 'Inschakelen', 'Uitschakelen', ', of $null. Deze eigenschap verlaten leeg of null wordt telemetrie inschakelen
| settings.advancedOptions.forcePullAndApply | Bool | Deze instelling is ontworpen voor het verbeteren van de ervaring van het werken met de extensie voor het registreren van knooppunten met Azure Automation DSC.  Als de waarde `$true`, de uitbreiding wordt gewacht op de eerste uitvoering van de configuratie die is opgehaald uit de service alvorens geslaagde/mislukte.  Als de waarde is ingesteld op $false, de status die wordt geretourneerd door de extensie wordt alleen verwijzen naar of het knooppunt is geregistreerd met de configuratie van Azure Automation-status en de configuratie van de knooppunten wordt niet uitgevoerd tijdens de registratie.
| settings.advancedOptions.downloadMappings | Verzameling | Definieert de alternatieve locaties voor het downloaden van afhankelijkheden, zoals WMF en .NET

### <a name="protected-settings-property-values"></a>Beveiligde instellingen eigenschapswaarden

| Name | Gegevenstype | Description
| ---- | ---- | ---- |
| protectedSettings.configurationArguments | string | Hiermee definieert u de parameters die u wilt doorgeven aan uw DSC-configuratie. Deze eigenschap worden, versleuteld. |
| protectedSettings.configurationUrlSasToken | string | Hiermee geeft u de SAS-token voor toegang tot de URL die is gedefinieerd door configuration.url. Deze eigenschap worden, versleuteld. |
| protectedSettings.configurationDataUrlSasToken | string | Hiermee geeft u de SAS-token voor toegang tot de URL die is gedefinieerd door configurationData.url. Deze eigenschap worden, versleuteld. |


## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen.
Sjablonen zijn ideaal bij het implementeren van een of meer virtuele machines waarvoor de post-implementatieconfiguratie.
Een voorbeeld van Resource Manager-sjabloon, met inbegrip van de DSC-extensie voor Windows kunt u vinden op de [Azure Quick Start-galerie](https://github.com/Azure/azure-quickstart-templates/blob/master/101-automation-configuration/nested/provisionServer.json#L91).

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van de Azure CLI. Als wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, voert u de volgende opdracht uit met de Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

-Uitbreidingspakket is gedownload en naar deze locatie op de Azure VM is geïmplementeerd
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}
```

Status van extensiebestand bevat de substatus en geslaagde of mislukte statuscodes samen met de gedetailleerde fout en een beschrijving voor elke uitvoering van extensies.
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}\Status\{0}.Status  -> {0} being the sequence number
```

Logboeken met uitvoergegevens extensie worden geregistreerd in de volgende map:

```
C:\WindowsAzure\Logs\Plugins\{Extension_Name}\{Extension_Version}
```

### <a name="error-codes-and-their-meanings"></a>Foutcodes en hun betekenis

| Foutcode | Betekenis | Mogelijke actie |
| :---: | --- | --- |
| 1000 | Algemene fout | Het bericht voor deze fout wordt geleverd door de specifieke uitzondering opgetreden in de logboeken van de extensie |
| 52 | Fout bij het installeren van extensie | Het bericht voor deze fout wordt geleverd door de specifieke uitzondering |
| 1002 | Fout van WMF installeren | Fout bij het installeren van WMF. |
| 1004 | Ongeldige Zip-pakket | Ongeldige zip; Fout bij het uitpakken van het ZIP-bestand |
| 1100 | Argument Error | Geeft een probleem in de invoer die is opgegeven door de gebruiker. Het bericht voor de fout die wordt geleverd door de specifieke uitzondering|


### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).
