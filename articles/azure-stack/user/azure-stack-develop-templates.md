---
title: Sjablonen voor Azure-Stack ontwikkelen | Microsoft Docs
description: Informatie over aanbevolen procedures voor Azure-Stack-sjabloon
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 8a5bc713-6f51-49c8-aeed-6ced0145e07b
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: bbd6cc68f1c16d48380cf498d6b089abe923e95a
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2018
---
# <a name="azure-resource-manager-template-considerations"></a>Overwegingen met betrekking tot Azure Resource Manager-sjabloon

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Tijdens het ontwikkelen van uw toepassing, is het belangrijk om ervoor te zorgen sjabloon draagbaarheid tussen Azure en Azure-Stack. Dit artikel vindt u overwegingen voor het ontwikkelen van Azure Resource Manager [sjablonen](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf), zodat u kunt prototype van uw toepassing en test-implementatie in Azure zonder toegang tot een Azure-Stack-omgeving.

## <a name="resource-provider-availability"></a>Provider beschikbaarheid
De sjabloon die u van plan bent om te implementeren moet alleen gebruiken voor Microsoft Azure-services die al beschikbaar of in het voorbeeld in de Azure-Stack zijn.

## <a name="public-namespaces"></a>Openbare naamruimten
Omdat Azure Stack wordt gehost in uw datacenter, heeft andere service-eindpunt naamruimten dan de openbare Azure-cloud. Als gevolg hiervan mislukken hardcoded openbare eindpunten in Azure Resource Manager-sjablonen wanneer u probeert te implementeren naar Azure-Stack. In plaats daarvan kunt u de *verwijzing* en *samenvoegen* functie voor het dynamisch opbouwen van het service-eindpunt op basis van waarden opgehaald van de resourceprovider tijdens de implementatie. Bijvoorbeeld, in plaats van het opgeven van *blob.core.windows.net* ophalen in de sjabloon de [primaryEndpoints.blob](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/101-simple-windows-vm/azuredeploy.json#L201) dynamisch instellen de *osDisk.URI* eindpunt:

     "osDisk": {"name": "osdisk","vhd": {"uri": 
     "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2015-06-15').primaryEndpoints.blob, variables('vmStorageAccountContainerName'),
      '/',variables('OSDiskName'),'.vhd')]"}}

## <a name="api-versioning"></a>API-versies
Versies van Azure service kunnen verschillen tussen Azure en Azure-Stack. Elke bron is vereist voor het kenmerk apiVersion de mogelijkheden die worden aangeboden definieert. Compatibiliteit van de API-versie in Azure-Stack zijn de volgende geldige API-versies voor elke Resource Provider:

| Resourceprovider | apiVersion |
| --- | --- |
| Compute |`'2015-06-15'` |
| Netwerk |`'2015-06-15'`, `'2015-05-01-preview'` |
| Storage |`'2016-01-01'`, `'2015-06-15'`, `'2015-05-01-preview'` |
| KeyVault | `'2015-06-01'` |
| App Service |`'2015-08-01'` |

## <a name="template-functions"></a>Sjabloonfuncties
Azure Resource Manager [functies](../../azure-resource-manager/resource-group-template-functions.md) bieden mogelijkheden die zijn vereist voor het bouwen van dynamische sjablonen. Als u bijvoorbeeld kunt u functies voor taken, zoals:

* Cookievalidatie of tekenreeksen bijsnijden 
* Referentiewaarden van andere bronnen
* Sequentieel op bronnen voor het implementeren van meerdere exemplaren 

Deze functies zijn niet beschikbaar in de Azure-Stack:

* Overslaan
* duren

## <a name="resource-location"></a>Resourcelocatie
Azure Resource Manager-sjablonen gebruiken een locatiekenmerk om resources plaats tijdens de implementatie. In Azure verwijzen locaties naar een regio VS-West of Zuid-Amerika. In Azure-Stack zijn locaties verschillend, omdat Azure-Stack in uw datacenter. U moet verwijzen naar de locatie van de resourcegroep als u afzonderlijke resources implementeren zodat sjablonen zijn naar tussen Azure en Azure-Stack. U kunt dit doen met behulp van `[resourceGroup().Location]` om te controleren of alle bronnen overnemen locatie van de resourcegroep. Het volgende fragment volgt een voorbeeld van het gebruik van deze functie tijdens de implementatie van een opslagaccount:

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

## <a name="next-steps"></a>Volgende stappen
* [Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)
* [Sjablonen met Azure CLI implementeren](azure-stack-deploy-template-command-line.md)
* [Sjablonen implementeren met Visual Studio](azure-stack-deploy-template-visual-studio.md)

