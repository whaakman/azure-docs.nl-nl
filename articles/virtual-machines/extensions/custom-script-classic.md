---
title: Aangepaste scriptextensie op een Windows-VM | Microsoft Docs
description: Configuratietaken voor virtuele Azure-machine automatiseren met behulp van de aangepaste scriptextensie voor de PowerShell-scripts uitvoeren op een externe Windows-VM
services: virtual-machines-windows
documentationcenter: ''
author: zroiy
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: ebb7340a-8f61-4d3c-a290-d7bf8de2d0bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: roiyz
ms.openlocfilehash: 1331eb311ca629253b4c4c28c9326e290887a752
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42055461"
---
# <a name="custom-script-extension-for-windows-using-the-classic-deployment-model"></a>Aangepast Script-extensie voor Windows met behulp van het klassieke implementatiemodel

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Lees [meer informatie over het uitvoeren van deze stappen met het Resource Manager-model](custom-script-windows.md).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../includes/virtual-machines-classic-portal.md)]

De aangepaste Scriptextensie downloadt en scripts uitvoeren op Azure virtual machines. Deze uitbreiding is handig voor post-implementatieconfiguraties, software-installaties of andere configuratie-/beheertaken. Scripts kunnen worden gedownload uit Azure Storage of GitHub, of worden geleverd in Azure Portal tijdens de uitvoering van extensies. De aangepaste scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen en kan ook worden uitgevoerd met Azure CLI, PowerShell, Azure Portal of de REST API van Azure Virtual Machine.

Dit document wordt uitgelegd hoe u de aangepaste Scriptextensie met behulp van de Azure PowerShell-module, Azure Resource Manager-sjablonen en details van stappen voor probleemoplossing in Windows-systemen.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De aangepaste Scriptextensie versies voor Windows kan worden uitgevoerd op basis van Windows Server 2008 R2, 2012, 2012 R2 en 2016.

### <a name="script-location"></a>Locatie van het script

Het script moet worden opgeslagen in Azure storage, of een andere locatie die toegankelijk zijn via een geldige URL.

### <a name="internet-connectivity"></a>Verbinding met Internet

De Custom Script Extension voor Windows is vereist dat de virtuele doelmachine is verbonden met internet. 

## <a name="extension-schema"></a>Extensieschema

De volgende JSON bevat het schema voor de aangepaste Scriptextensie. De extensie is vereist voor de locatie van een script (Azure Storage of een andere locatie met een geldige URL) en een opdracht uit te voeren. Als u Azure Storage als de scriptbron, wordt een Azure storage-account de naam en sleutel is vereist. Deze items moeten worden beschouwd als vertrouwelijke gegevens en opgegeven in de configuratie van de instelling voor beveiligde extensies. Azure-VM-extensie beveiligde instellingsgegevens versleuteld en alleen op de virtuele doelmachine worden ontsleuteld.

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

### <a name="property-values"></a>Waarden van eigenschappen

| Naam | Waarde / voorbeeld |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Uitgever | Microsoft.Compute |
| Extensie | Customscriptextension gebruiken |
| typeHandlerVersion | 1.8 |
| fileUris (bijvoorbeeld) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 |
| commandToExecute (bijvoorbeeld) | PowerShell - ExecutionPolicy Unrestricted - File configureren muziek app.ps1 |

## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie beschreven kan worden gebruikt in een Azure Resource Manager-sjabloon om uit te voeren van de aangepaste Scriptextensie tijdens de sjabloonimplementatie van een Azure Resource Manager. Een voorbeeldsjabloon met de Custom Script Extension vindt u hier [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell-implementatie

De `Set-AzureVMCustomScriptExtension` opdracht kan worden gebruikt om de aangepaste scriptextensie toevoegen aan een bestaande virtuele machine. Zie voor meer informatie, [Set AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmcustomscriptextension).

```powershell
# create vm object
$vm = Get-AzureVM -Name 2016clas -ServiceName 2016clas1313

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri myFileUri -Run 'create-file.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van de Azure PowerShell-module. Als u wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, moet u de volgende opdracht uitvoeren.

```powershell
Get-AzureVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Extensie uitvoering uitvoer ons vastgelegd op bestanden die zijn gevonden in de volgende map op de virtuele doelmachine.

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Het script zelf wordt gedownload naar de volgende map op de virtuele doelmachine.

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).
