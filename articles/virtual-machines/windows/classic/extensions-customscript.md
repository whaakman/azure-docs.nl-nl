---
title: Aangepaste scriptextensie op een virtuele machine van Windows | Microsoft Docs
description: Virtuele machine van Azure-configuratietaken automatiseren met behulp van de aangepaste scriptextensie PowerShell-scripts uitvoeren op een externe Windows-VM
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: ebb7340a-8f61-4d3c-a290-d7bf8de2d0bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: nepeters
ms.openlocfilehash: 986ab1025dc188cd7fa1cf8b131a9d4b859be8f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="custom-script-extension-for-windows-using-the-classic-deployment-model"></a>Aangepast Script uitbreiding voor Windows met het klassieke implementatiemodel

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Lees [meer informatie over het uitvoeren van deze stappen met het Resource Manager-model](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

De aangepaste Scriptextensie downloads en scripts op virtuele machines in Azure wordt uitgevoerd. Deze uitbreiding is nuttig voor post-implementatieconfiguratie, software-installatie of een andere configuratie / beheertaak. Scripts kunnen worden gedownload van Azure storage of GitHub, of naar de Azure portal op extensie uitvoeringstijd. De aangepaste scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen en kan ook worden uitgevoerd met de Azure CLI, PowerShell, Azure-portal of de REST-API van Azure virtuele Machine.

Dit document details over het gebruik van de aangepaste Scriptextensie met behulp van de Azure PowerShell-module, Azure Resource Manager-sjablonen en details stappen voor probleemoplossing in Windows-systemen.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De aangepaste Scriptextensie versies voor Windows kan worden uitgevoerd op basis van Windows Server 2008 R2, 2012, 2012 R2 en 2016.

### <a name="script-location"></a>De locatie van script

Het script moet worden opgeslagen in Azure-opslag of een andere locatie die toegankelijk zijn via een geldige URL.

### <a name="internet-connectivity"></a>Verbinding met Internet

De aangepaste Script uitbreiding voor Windows is vereist dat de virtuele doelmachine is verbonden met internet. 

## <a name="extension-schema"></a>Uitbreidingsschema

De volgende JSON geeft het schema voor de aangepaste Scriptextensie. De extensie is vereist voor de locatie van een script (Azure Storage of een andere locatie met een geldige URL) en een opdracht uit te voeren. Als u Azure-opslag gebruikt als de scriptbron, wordt een Azure storage-account en -account sleutel is vereist. Deze items moeten worden behandeld als gevoelige gegevens en opgegeven in de configuratie van de instelling voor beveiligde uitbreidingen. Azure VM-extensie beveiligde instellingsgegevens is versleuteld en alleen op de virtuele doelmachine worden ontsleuteld.

```json
{
    "name": "config-app",
    "type": "Microsoft.ClassicCompute/virtualMachines/extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-01",
    "properties": {
        "publisher": "Microsoft.Compute",
        "extension": "CustomScriptExtension",
        "version": "1.8",
        "parameters": {
            "public": {
                "fileUris": "[myScriptLocation]"
            },
            "private": {
                "commandToExecute": "[myExecutionString]"
            }
        }
    }
}
```

### <a name="property-values"></a>Eigenschapswaarden

| Naam | Waarde / voorbeeld |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Uitgever | Microsoft.Compute |
| De extensie | CustomScriptExtension |
| typeHandlerVersion | 1.8 |
| fileUris (bijvoorbeeld) | https://RAW.githubusercontent.com/Microsoft/DotNet-Core-sample-templates/master/DotNet-Core-Music-Windows/scripts/Configure-Music-App.ps1 |
| commandToExecute (bijvoorbeeld) | PowerShell - ExecutionPolicy Unrestricted - bestand configureren muziek app.ps1 |

## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie wordt beschreven, kan de aangepaste Scriptextensie uitvoeren tijdens de sjabloonimplementatie van een Azure Resource Manager-in een Azure Resource Manager-sjabloon worden gebruikt. Een voorbeeldsjabloon met de aangepaste Scriptextensie vindt u hier, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell-implementatie

De `Set-AzureVMCustomScriptExtension` opdracht kan worden gebruikt voor de extensie voor aangepaste scripts toevoegen aan een bestaande virtuele machine. Zie voor meer informatie [Set AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).

```powershell
# create vm object
$vm = Get-AzureVM -Name 2016clas -ServiceName 2016clas1313

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri myFileUri -Run 'create-file.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a>Oplossen van problemen en ondersteunen

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure portal en met behulp van de Azure PowerShell-module. Overzicht van de implementatiestatus van uitbreidingen voor een bepaalde virtuele machine, voer de volgende opdracht.

```powershell
Get-AzureVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Uitvoering van de extensie uitvoer ons aangemeld bestanden gevonden in de volgende map op de virtuele doelmachine.

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Het script zelf is gedownload naar de volgende map op de virtuele doelmachine.

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a>Ondersteuning

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op de [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/en-us/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/en-us/support/options/) en selecteer de Get-ondersteuning. Voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/en-us/support/faq/).