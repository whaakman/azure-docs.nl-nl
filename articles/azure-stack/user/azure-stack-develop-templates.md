---
title: Sjablonen ontwikkelen voor Azure Stack | Microsoft Docs
description: Informatie over aanbevolen procedures voor Azure Stack-sjabloon
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 8a5bc713-6f51-49c8-aeed-6ced0145e07b
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: 21fd3a33181542d86eccc4292ae68f7ce25e0a05
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46366723"
---
# <a name="azure-resource-manager-template-considerations"></a>Overwegingen met betrekking tot Azure Resource Manager-sjabloon

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Als u uw toepassing ontwikkelt, is het belangrijk om te controleren of de sjabloon overdraagbaarheid tussen Azure en Azure Stack. Dit artikel bevat overwegingen voor het ontwikkelen van Azure Resource Manager [sjablonen](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf), zodat u kunt prototype van uw toepassing en test deze implementatie in Azure zonder toegang tot een Azure Stack-omgeving.

## <a name="resource-provider-availability"></a>Provider voor resourcebeschikbaarheid

De sjabloon die u van plan bent om te implementeren moet alleen gebruiken voor Microsoft Azure-services die al beschikbaar of in de Preview-versie in Azure Stack zijn.

## <a name="public-namespaces"></a>Openbare-naamruimten

Omdat Azure Stack wordt gehost in uw datacenter, heeft andere service-eindpunt naamruimten dan de openbare cloud van Azure. Als gevolg hiervan niet vastgelegd openbare eindpunten in Azure Resource Manager-sjablonen wanneer u probeert te implementeren voor Azure Stack. U kunt dynamisch bouwen met service-eindpunten met behulp van de *verwijzing* en *samenvoegen* functies voor het ophalen van waarden van de resourceprovider tijdens de implementatie. Bijvoorbeeld, in plaats van hardcoderen *blob.core.windows.net* in uw sjabloon, haalt de [primaryEndpoints.blob](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/101-vm-windows-create/azuredeploy.json#L175) dynamisch instellen de *osDisk.URI* eindpunt:

```json
"osDisk": {"name": "osdisk","vhd": {"uri":
"[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2015-06-15').primaryEndpoints.blob, variables('vmStorageAccountContainerName'),
 '/',variables('OSDiskName'),'.vhd')]"}}
```

## <a name="api-versioning"></a>API-versiebeheer

Azure-versies kunnen verschillen tussen Azure en Azure Stack. Elke resource vereist de **apiVersion** kenmerk, waarin de mogelijkheden die zijn gedefinieerd. De volgende API-versies zijn om te controleren of de compatibiliteit van de API-versie in Azure Stack, geldig zijn voor elke resourceprovider:

| Resourceprovider | apiVersion |
| --- | --- |
| Compute |`'2015-06-15'` |
| Netwerk |`'2015-06-15'`, `'2015-05-01-preview'` |
| Storage |`'2016-01-01'`, `'2015-06-15'`, `'2015-05-01-preview'` |
| KeyVault | `'2015-06-01'` |
| App Service |`'2015-08-01'` |

## <a name="template-functions"></a>Sjabloonfuncties

Azure Resource Manager [functies](../../azure-resource-manager/resource-group-template-functions.md) bieden mogelijkheden voor het ontwikkelen van dynamische sjablonen. Een voorbeeld: u kunt functies voor taken zoals:

* Samenvoegen of bijsnijden tekenreeksen.
* Verwijzen naar waarden van andere bronnen.
* Herhalen op resources om te implementeren van meerdere exemplaren.

Deze functies zijn niet beschikbaar in Azure Stack:

* Overslaan
* toets maken

## <a name="resource-location"></a>Resourcelocatie

Azure Resource Manager-sjablonen gebruiken een `location` kenmerk voor het plaatsen van bronnen tijdens de implementatie. In Azure verwijzen locaties naar een regio, zoals VS-West of Zuid-Amerika. In Azure Stack zijn locaties verschillend, omdat Azure Stack in uw datacenter is. U moet verwijzen naar de locatie voor resourcegroep als u afzonderlijke resources implementeren om ervoor te zorgen sjablonen worden overgedragen tussen Azure en Azure Stack. U kunt dit doen met `[resourceGroup().Location]` om te controleren of alle resources overnemen locatie voor de resourcegroep. De volgende code is een voorbeeld van het gebruik van deze functie tijdens de implementatie van een storage-account:

```json
"resources": [
{
  "name": "[variables('storageAccountName')]",
  "type": "Microsoft.Storage/storageAccounts",
  "apiVersion": "[variables('apiVersionStorage')]",
  "location": "[resourceGroup().location]",
  "comments": "This storage account is used to store the VM disks",
  "properties": {
  "accountType": "Standard_GRS"
  }
}
]
```

## <a name="next-steps"></a>Volgende stappen

* [Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)
* [Sjablonen implementeren met Azure CLI](azure-stack-deploy-template-command-line.md)
* [Sjablonen implementeren met Visual Studio](azure-stack-deploy-template-visual-studio.md)
