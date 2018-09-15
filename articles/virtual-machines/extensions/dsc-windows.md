---
title: Azure-extensie-Handler voor Desired State Configuration | Microsoft Docs
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
ms.openlocfilehash: b9e96473a6f66dcbc675da1553deaed4ad61b249
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45630922"
---
# <a name="powershell-dsc-extension"></a>PowerShell DSC-extensie

## <a name="overview"></a>Overzicht

De PowerShell DSC-extensie voor Windows is gepubliceerd en ondersteund door Microsoft. De extensie wordt geüpload en een PowerShell DSC-configuratie is van toepassing op een Azure-VM. De DSC-extensie aanroepen naar PowerShell DSC op te nemen van de ontvangen DSC-configuratie op de virtuele machine. In dit document worden de ondersteunde platforms, configuraties en implementatie-opties voor de extensie van de DSC-virtuele machine voor Windows.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De DSC-extensie biedt ondersteuning voor het volgende besturingssysteem

WindowsServer 2016, Windows Server 2012R2, WindowsServer 2012, Windows Server 2008 R2 SP1, Windows-Client 7/8.1

### <a name="internet-connectivity"></a>Internetconnectiviteit

De DSC-extensie voor Windows is vereist dat de virtuele doelmachine is verbonden met internet. 

## <a name="extension-schema"></a>Extensieschema

De volgende JSON ziet u het schema voor het gedeelte instellingen van de DSC-extensie in een Azure Resource Manager-sjabloon. 

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "Microsoft.Powershell.DSC",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "properties": {
    "publisher": "Microsoft.Powershell",
    "type": "DSC",
    "typeHandlerVersion": "2.73",
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

| Naam | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | datum |
| Uitgever | Microsoft.Powershell.DSC | tekenreeks |
| type | DSC | tekenreeks |
| typeHandlerVersion | 2,73 | int |

### <a name="settings-property-values"></a>Waarden voor de eigenschap instellingen

| Naam | Gegevenstype | Beschrijving
| ---- | ---- | ---- |
| settings.wmfVersion | tekenreeks | Hiermee geeft u de versie van Windows Management Framework die moet worden geïnstalleerd op de virtuele machine. Als deze eigenschap ingesteld op 'nieuwste', wordt de meest recente versie van WMF installeren. De alleen huidige mogelijke waarden voor deze eigenschap zijn 4.0, '5.0' en 'nieuwste'. Deze mogelijke waarden zijn afhankelijk van updates. De standaardwaarde is 'nieuwste'. |
| settings.configuration.url | tekenreeks | Hiermee geeft u de URL-locatie van waaruit het zip-bestand van uw DSC-configuratie gedownload. Als de opgegeven URL is een SAS-token om toegang te krijgen vereist, moet u de eigenschap protectedSettings.configurationUrlSasToken instellen op de waarde van uw SAS-token. Deze eigenschap is vereist als settings.configuration.script en/of settings.configuration.function zijn gedefinieerd.
| settings.configuration.script | tekenreeks | Hiermee geeft u de bestandsnaam van het script dat de definitie van de DSC-configuratie bevat. Met dit script moet zich in de hoofdmap van het zip-bestand van de URL die is opgegeven door de eigenschap configuration.url gedownload. Deze eigenschap is vereist als settings.configuration.url en/of settings.configuration.script zijn gedefinieerd.
| settings.configuration.function | tekenreeks | Hiermee geeft u de naam van de DSC-configuratie. De configuratie met de naam moet worden opgenomen in het script dat is gedefinieerd door configuration.script. Deze eigenschap is vereist als settings.configuration.url en/of settings.configuration.function zijn gedefinieerd.
| settings.configurationArguments | Verzameling | Hiermee definieert u de parameters die u wilt doorgeven aan uw DSC-configuratie. Deze eigenschap wordt niet versleuteld.
| settings.configurationData.url | tekenreeks | Geeft de URL waaruit u uw gegevens (.pds1)-configuratiebestand te downloaden om te gebruiken als invoer voor uw DSC-configuratie. Als de opgegeven URL is een SAS-token om toegang te krijgen vereist, moet u de eigenschap protectedSettings.configurationDataUrlSasToken instellen op de waarde van uw SAS-token.
| settings.privacy.dataEnabled | tekenreeks | Hiermee of verzamelen van telemetriegegevens uitgeschakeld. De enige mogelijke waarden voor deze eigenschap zijn 'Inschakelen', 'Uitschakelen', ', of $null. Deze eigenschap verlaten leeg of null wordt telemetrie inschakelen
| settings.advancedOptions.forcePullAndApply | BOOL | Kan de DSC-extensie bij te werken en DSC-configuraties gerapporteerd als de vernieuwingsmodus voor het Pull is.
| settings.advancedOptions.downloadMappings | Verzameling | Definieert de alternatieve locaties voor het downloaden van afhankelijkheden, zoals WMF en .NET

### <a name="protected-settings-property-values"></a>Beveiligde instellingen eigenschapswaarden

| Naam | Gegevenstype | Beschrijving
| ---- | ---- | ---- |
| protectedSettings.configurationArguments | tekenreeks | Hiermee definieert u de parameters die u wilt doorgeven aan uw DSC-configuratie. Deze eigenschap worden, versleuteld. |
| protectedSettings.configurationUrlSasToken | tekenreeks | Hiermee geeft u de SAS-token voor toegang tot de URL die is gedefinieerd door configuration.url. Deze eigenschap worden, versleuteld. |
| protectedSettings.configurationDataUrlSasToken | tekenreeks | Hiermee geeft u de SAS-token voor toegang tot de URL die is gedefinieerd door configurationData.url. Deze eigenschap worden, versleuteld. |


## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. Sjablonen zijn ideaal bij het implementeren van een of meer virtuele machines waarvoor de post-implementatieconfiguratie. Een voorbeeld van Resource Manager-sjabloon met de OMS-Agent-VM-extensie kunt u vinden op de [Azure Quick Start-galerie](https://github.com/Azure/azure-quickstart-templates/tree/052db5feeba11f85d57f170d8202123511f72044/dsc-extension-iis-server-windows-vm). 

De JSON-configuratie voor een VM-extensie worden genest in de bron van de virtuele machine of geplaatst op de hoofdmap of het hoogste niveau van een Resource Manager JSON-sjabloon. De plaatsing van de JSON-configuratie is van invloed op de waarde van de resourcenaam en het type. 

Wanneer het nesten van de extensie-resource, de JSON wordt geplaatst in de `"resources": []` object van de virtuele machine. Bij het plaatsen van de JSON-extensie in de hoofdmap van de sjabloon, naam van de resource bevat een verwijzing naar de bovenliggende virtuele machine en het type weerspiegelt de geneste configuratie.  


## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

De Azure CLI kan worden gebruikt om de OMS-Agent-VM-extensie om een bestaande virtuele machine te implementeren. Vervangen door de OMS-sleutel en de OMS-ID van uw OMS-werkruimte. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name Microsoft.Powershell.DSC \
  --publisher Microsoft.Powershell \
  --version 2.73 --protected-settings '{}' \
  --settings '{}'
```

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
| 1100 | Argumentfout | Geeft een probleem in de invoer die is opgegeven door de gebruiker. Het bericht voor de fout die wordt geleverd door de specifieke uitzondering|


### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).