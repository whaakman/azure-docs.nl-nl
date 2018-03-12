---
title: De extensie Azure aangepaste scripts voor Windows | Microsoft Docs
description: Configuratietaken voor Windows-VM automatiseren met behulp van de aangepaste scriptextensie
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/16/2017
ms.author: danis
ms.openlocfilehash: 454bfde69248538a92d2fc447c54509afb08129a
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="custom-script-extension-for-windows"></a>Extensie voor aangepaste scripts voor Windows

De aangepaste Scriptextensie downloads en scripts op virtuele machines in Azure wordt uitgevoerd. Deze uitbreiding is handig voor post-implementatieconfiguraties, software-installaties of andere configuratie-/beheertaken. Scripts kunnen worden gedownload uit Azure Storage of GitHub, of worden geleverd in Azure Portal tijdens de uitvoering van extensies. De aangepaste scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen en kan ook worden uitgevoerd met Azure CLI, PowerShell, Azure Portal of de REST API van Azure Virtual Machine.

Dit document details over het gebruik van de aangepaste Scriptextensie met behulp van de Azure PowerShell-module, Azure Resource Manager-sjablonen en details stappen voor probleemoplossing in Windows-systemen.

## <a name="prerequisites"></a>Vereisten

> [!NOTE]  
> Gebruik geen extensie voor aangepaste scripts uit te voeren Update-AzureRmVM met dezelfde virtuele machine als de parameter, omdat het wacht op zichzelf.  
>   
> 

### <a name="operating-system"></a>Besturingssysteem

De aangepaste Script uitbreiding voor Windows kan worden uitgevoerd op basis van de Client voor Windows 10, Windows Server 2008 R2, 2012, 2012 R2 en 2016 worden vrijgegeven.

### <a name="script-location"></a>De locatie van script

Het script moet worden opgeslagen in Azure Blob-opslag of een andere locatie die toegankelijk zijn via een geldige URL.

### <a name="internet-connectivity"></a>Verbinding met Internet

De aangepaste Script uitbreiding voor Windows is vereist dat de virtuele doelmachine is verbonden met internet. 

## <a name="extension-schema"></a>Uitbreidingsschema

De volgende JSON geeft het schema voor de aangepaste Scriptextensie. De extensie is vereist voor de locatie van een script (Azure Storage of een andere locatie met een geldige URL) en een opdracht uit te voeren. Als u Azure-opslag gebruikt als de scriptbron, wordt een Azure storage-account en -account sleutel is vereist. Deze items moeten worden behandeld als gevoelige gegevens en opgegeven in de configuratie van de instelling voor beveiligde uitbreidingen. Azure VM-extensie beveiligde instellingsgegevens is versleuteld en alleen op de virtuele doelmachine worden ontsleuteld.

```json
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
        "[variables('musicstoresqlName')]"
    ],
    "tags": {
        "displayName": "config-app"
    },
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```

### <a name="property-values"></a>Eigenschapswaarden

| Naam | Waarde / voorbeeld |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| publisher | Microsoft.Compute |
| type | Uitbreidingen |
| typeHandlerVersion | 1.9 |
| fileUris (bijvoorbeeld) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 |
| commandToExecute (bijvoorbeeld) | PowerShell - ExecutionPolicy Unrestricted - bestand configureren muziek app.ps1 |
| storageAccountName (bijvoorbeeld) | examplestorageacct |
| storageAccountKey (bijvoorbeeld) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== |

**Opmerking** -de namen van deze eigenschappen zijn hoofdlettergevoelig. Gebruik de namen zoals hierboven om te voorkomen dat problemen bij de implementatie.

## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie wordt beschreven, kan de aangepaste Scriptextensie uitvoeren tijdens de sjabloonimplementatie van een Azure Resource Manager-in een Azure Resource Manager-sjabloon worden gebruikt. Een voorbeeldsjabloon met de aangepaste Scriptextensie vindt u hier, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell-implementatie

De `Set-AzureRmVMCustomScriptExtension` opdracht kan worden gebruikt voor de extensie voor aangepaste scripts toevoegen aan een bestaande virtuele machine. Zie voor meer informatie [Set AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a>Oplossen van problemen en ondersteunen

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure portal en met behulp van de Azure PowerShell-module. Overzicht van de implementatiestatus van uitbreidingen voor een bepaalde virtuele machine, voer de volgende opdracht.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

De uitvoer van de extensie-uitvoering is geregistreerd in bestanden gevonden in de volgende map op de virtuele doelmachine.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

De opgegeven bestanden zijn gedownload naar de volgende map op de virtuele doelmachine.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
waar `<n>` is een decimal integer die tussen uitvoeringen van de uitbreiding mag wijzigen.  De `1.*` waarde komt overeen met de werkelijke, huidige `typeHandlerVersion` waarde van de extensie.  Bijvoorbeeld: de werkelijke map kan niet `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Bij het uitvoeren van de `commandToExecute` opdracht, de uitbreiding voor deze map hebt ingesteld (bijvoorbeeld `...\Downloads\2`) als de huidige werkmap. Dit maakt het gebruik van relatieve paden te vinden van de bestanden gedownload de `fileURIs` eigenschap. Zie de onderstaande tabel voor voorbeelden.

Omdat het absolute downloadpad na verloop van tijd variëren kan, is het beter om te kiezen voor relatieve scriptbestand paden in de `commandToExecute` tekenreeks, indien mogelijk. Bijvoorbeeld:
```json
    "commandToExecute": "powershell.exe . . . -File \"./scripts/myscript.ps1\""
```

Informatie over het pad na het eerste segment van de URI wordt bewaard voor bestanden die worden gedownload de `fileUris` eigenschappenlijst.  Zoals u in de onderstaande tabel, gedownloade bestanden zijn toegewezen in submappen downloaden in overeenstemming met de structuur van de `fileUris` waarden.  

#### <a name="examples-of-downloaded-files"></a>Voorbeelden van gedownloade bestanden

| URI in fileUris | Relatieve locatie voor gedownloade | Absolute locatie gedownload * |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\* Als verandert hierboven, de absolute paden gedurende de levensduur van de virtuele machine, maar niet binnen één uitvoering van de extensie CustomScript.

### <a name="support"></a>Ondersteuning

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts in de [MSDN Azure en Stack Overflow forums] raadplegen (https://azure.microsoft.com/en-us/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/en-us/support/options/) en selecteer de Get-ondersteuning. Voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/en-us/support/faq/).
