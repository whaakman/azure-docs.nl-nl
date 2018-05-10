---
title: Azure Desired State Configuration extensie Handler | Microsoft Docs
description: Uploaden en een PowerShell DSC-configuratie toepassen op een virtuele machine in Azure met behulp van de DSC-extensie
services: virtual-machines-windows
documentationcenter: ''
author: eshaparmar
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: windows
ms.workload: ''
ms.date: 03/26/2018
ms.author: esparmar
ms.openlocfilehash: b34314951980f7dbe2269119883dec52a90a0587
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="powershell-dsc-extension"></a>PowerShell DSC-uitbreiding

## <a name="overview"></a>Overzicht

De PowerShell DSC-uitbreiding voor Windows is gepubliceerd en ondersteund door Microsoft. De extensie uploadt en een PowerShell DSC-configuratie is van toepassing op een Azure VM. De DSC-uitbreiding aanroepen in PowerShell DSC op te nemen van de ontvangen DSC-configuratie op de virtuele machine. In dit document worden de ondersteunde platforms, configuraties en implementatie-opties voor de extensie van de DSC-virtuele machine voor Windows.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De DSC-uitbreiding ondersteunt de volgende OS

WindowsServer 2016, Windows Server 2012R2, WindowsServer 2012, Windows Server 2008 R2 SP1, Windows 7/8.1-Client

### <a name="internet-connectivity"></a>Internetconnectiviteit

De DSC-uitbreiding voor Windows is vereist dat de virtuele doelmachine is verbonden met internet. 

## <a name="extension-schema"></a>Extensieschema

De volgende JSON geeft het schema voor het gedeelte instellingen van de DSC-uitbreiding in een Azure Resource Manager-sjabloon. 

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

### <a name="property-values"></a>Eigenschapswaarden

| Naam | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | datum |
| Uitgever | Microsoft.Powershell.DSC | tekenreeks |
| type | DSC | tekenreeks |
| typeHandlerVersion | 2,73 | int |

### <a name="settings-property-values"></a>Waarden van eigenschap

| Naam | Gegevenstype | Beschrijving
| ---- | ---- | ---- |
| settings.wmfVersion | tekenreeks | Hiermee geeft u de versie van het Windows Management Framework die op de virtuele machine moet worden geïnstalleerd. Als deze eigenschap ingesteld op 'nieuwste', wordt de meest recente versie van WMF geïnstalleerd. De alleen huidige mogelijke waarden voor deze eigenschap zijn 4.0, '5.0' en 'nieuwste'. Deze mogelijke waarden zijn onderworpen aan updates. De standaardwaarde is 'nieuwste'. |
| settings.configuration.url | tekenreeks | Hiermee geeft u de URL-locatie van waaruit u uw DSC configuration zip-bestand te downloaden. Als de opgegeven URL een SAS-token om toegang te krijgen vereist, moet u de eigenschap protectedSettings.configurationUrlSasToken ingesteld op de waarde van de SAS-token. Deze eigenschap is vereist als settings.configuration.script en/of settings.configuration.function zijn gedefinieerd.
| settings.configuration.script | tekenreeks | Hiermee geeft u de bestandsnaam van het script met de definitie van de DSC-configuratie. Dit script moet zich in de hoofdmap van het zip-bestand van de URL die is opgegeven door de eigenschap configuration.url gedownload. Deze eigenschap is vereist als settings.configuration.url en/of settings.configuration.script zijn gedefinieerd.
| settings.configuration.function | tekenreeks | Geeft de naam van de DSC-configuratie. De configuratie met de naam moet worden opgenomen in het script dat is gedefinieerd door configuration.script. Deze eigenschap is vereist als settings.configuration.url en/of settings.configuration.function zijn gedefinieerd.
| settings.configurationArguments | Verzameling | Hiermee definieert u de parameters die u wilt doorgeven aan uw DSC-configuratie. Deze eigenschap wordt niet versleuteld.
| settings.configurationData.url | tekenreeks | Geeft de URL waaruit u uw gegevens (.pds1) configuratiebestand te downloaden om te gebruiken als invoer voor uw DSC-configuratie. Als de opgegeven URL een SAS-token om toegang te krijgen vereist, moet u de eigenschap protectedSettings.configurationDataUrlSasToken ingesteld op de waarde van de SAS-token.
| settings.privacy.dataEnabled | tekenreeks | Schakelt verzamelen van telemetriegegevens in of uit. De enige mogelijke waarden voor deze eigenschap zijn 'Inschakelen', 'Uitschakelen', ', of $null. Als u deze eigenschap leeg of null, wordt telemetrie inschakelen
| settings.advancedOptions.forcePullAndApply | BOOL | Kan de DSC-extensie bij te werken en DSC-configuraties vast wanneer de vernieuwingsmodus Pull.
| settings.advancedOptions.downloadMappings | Verzameling | Alternatieve locaties voor het downloaden van afhankelijkheden, zoals WMF en .NET definieert

### <a name="protected-settings-property-values"></a>Beveiligde instellingen eigenschapswaarden

| Naam | Gegevenstype | Beschrijving
| ---- | ---- | ---- |
| protectedSettings.configurationArguments | tekenreeks | Hiermee definieert u de parameters die u wilt doorgeven aan uw DSC-configuratie. Deze eigenschap wordt versleuteld. |
| protectedSettings.configurationUrlSasToken | tekenreeks | Hiermee geeft u de SAS-token voor toegang tot de URL die is gedefinieerd door configuration.url. Deze eigenschap wordt versleuteld. |
| protectedSettings.configurationDataUrlSasToken | tekenreeks | Hiermee geeft u de SAS-token voor toegang tot de URL die is gedefinieerd door configurationData.url. Deze eigenschap wordt versleuteld. |


## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. Sjablonen zijn ideaal bij het implementeren van een of meer virtuele machines die moeten worden geconfigureerd voor post-implementatie. Een voorbeeld Resource Manager-sjabloon met de OMS-Agent VM-extensie vindt u op de [galerie van Azure Quick Start](https://github.com/Azure/azure-quickstart-templates/tree/052db5feeba11f85d57f170d8202123511f72044/dsc-extension-iis-server-windows-vm). 

De JSON-configuratie voor de extensie van een virtuele machine worden genest in de bron van de virtuele machine, of aan de basis- of bovenste niveau van een Resource Manager JSON-sjabloon geplaatst. De plaatsing van de JSON-configuratie is van invloed op de waarde van de resourcenaam en het type. 

Wanneer het nesten van de extensie-resource, de JSON wordt geplaatst in de `"resources": []` object van de virtuele machine. Bij het plaatsen van de JSON-extensie in de hoofdmap van de sjabloon, de naam van de bron bevat een verwijzing naar de bovenliggende virtuele machine en het type reflecteert de geneste configuratie.  


## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

De Azure CLI kan worden gebruikt voor het implementeren van de OMS-Agent VM-extensie op een bestaande virtuele machine. Vervangen door de OMS-sleutel en de OMS-ID die uit de OMS-werkruimte. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name Microsoft.Powershell.DSC \
  --publisher Microsoft.Powershell \
  --version 2.73 --protected-settings '{}' \
  --settings '{}'
```

## <a name="troubleshoot-and-support"></a>Oplossen van problemen en ondersteunen

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure portal en met behulp van de Azure CLI. Overzicht van de implementatiestatus van uitbreidingen voor een bepaalde virtuele machine, voer de volgende opdracht met de Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uitbreidingspakket is gedownload en geïmplementeerd op deze locatie op de Azure VM
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}
```

Status van extensiebestand bevat de substatus en geslaagde of mislukte statuscodes samen met de gedetailleerde fout en beschrijving voor elke uitbreiding die is uitgevoerd.
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}\Status\{0}.Status  -> {0} being the sequence number
```

Extensie uitvoer logboeken worden geregistreerd in de volgende map:

```
C:\WindowsAzure\Logs\Plugins\{Extension_Name}\{Extension_Version}
```

### <a name="error-codes-and-their-meanings"></a>Foutcodes en hun betekenis

| Foutcode | Betekenis | Mogelijke actie |
| :---: | --- | --- |
| 1000 | Algemene fout | Het bericht voor deze fout wordt geleverd door de specifieke uitzondering in Logboeken van de extensie |
| 52 | Fout bij het installeren van extensie | Het bericht voor deze fout wordt geleverd door de specifieke uitzondering |
| 1002 | Fout bij installeren van WMF | Fout bij het installeren van WMF. |
| 1004 | Ongeldige Zip-pakket | Ongeldige zip; Fout bij het uitpakken van het ZIP-bestand |
| 1100 | Argumentfout | Geeft een probleem in de invoer is opgegeven door de gebruiker. Het bericht voor de fout wordt geleverd door de specifieke uitzondering|


### <a name="support"></a>Ondersteuning

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op de [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer de Get-ondersteuning. Voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/support/faq/).