---
title: Geef de resourcegroep voor virtuele machines in Azure DevTest Labs | Microsoft Docs
description: Informatie over het opgeven van een resourcegroep voor virtuele machines in een lab in Azure DevTest Labs.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2019
ms.author: spelluru
ms.openlocfilehash: 2903228a3257abe9c454c6e0720a1e228b905290
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56300037"
---
# <a name="specify-a-resource-group-for-lab-virtual-machines-in-azure-devtest-labs"></a>Geef een resourcegroep voor de virtuele machines in Azure DevTest Labs
Als de eigenaar van een lab, kunt u uw lab-virtuele machines worden gemaakt in een specifieke resourcegroep configureren. Gebruik deze functie zodat u geen resource group limieten op uw Azure-abonnement bereiken. Deze functie kunt u uw labresources binnen een enkele resourcegroep consolideren. Het vereenvoudigt ook de resources tracking en toepassen van [beleid](../governance/policy/overview.md) moeten worden beheerd op het niveau van de resource.

Met deze functie kunt u een script om op te geven van een nieuwe of een bestaande resourcegroep gebruiken binnen uw Azure-abonnement voor alle uw lab VM's. DevTest Labs ondersteunt momenteel deze functie via een API. 

## <a name="api-to-configure-a-resource-group-for-labs-vms"></a>API voor het configureren van een resourcegroep voor labs VM 's
Nu gaan we stapsgewijs door de opties die u als de eigenaar van een lab hebt tijdens het gebruik van deze API: 

- U kunt ervoor kiezen de **lab van resourcegroep** voor alle virtuele machines.
- U kunt ervoor kiezen een **bestaande resourcegroep** dan de resourcegroep van de testomgeving voor alle virtuele machines.
- U kunt een **nieuwe resourcegroep** naam voor alle virtuele machines.
- U kunt doorgaan met het bestaande gedrag, dat wil zeggen, een resourcegroep is gemaakt voor elke virtuele machine in het lab.
 
Deze instelling is van toepassing op nieuwe virtuele machines die zijn gemaakt in het lab. Oudere VM's in uw testomgeving die zijn gemaakt in hun eigen resourcegroepen blijven ongewijzigd blijven. Omgevingen die zijn gemaakt in uw testomgeving gaan om te blijven in hun eigen resourcegroepen.

### <a name="how-to-use-this-api"></a>Het gebruik van deze API:
- Gebruik de API-versie **2018_10_15_preview** tijdens het gebruik van deze API. 
- Als u een nieuwe resourcegroep opgeeft, zorgt u ervoor dat u hebt **schrijfmachtigingen op resourcegroepen** binnen uw abonnement. Zonder schrijfmachtigingen, het maken van nieuwe virtuele machines in het resultaat van de groep opgegeven bron in een fout. 
- Tijdens het gebruik van de API, doorgeven de **volledige resourcegroep-ID**. Bijvoorbeeld: `/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>`. Zorg ervoor dat de resourcegroep bevindt zich in hetzelfde abonnement als die van het testlab. 

## <a name="use-powershell"></a>PowerShell gebruiken 
Volgende voorbeeld wordt beschreven hoe u alle virtuele machines maken in een nieuwe resourcegroep met behulp van een PowerShell-script.

```PowerShell
[CmdletBinding()]
Param(
    $subId,
    $labRg,
    $labName,
    $vmRg
)

az login | out-null

az account set --subscription $subId | out-null

$rgId = "/subscriptions/"+$subId+"/resourceGroups/"+$vmRg

"Updating lab '$labName' with vm rg '$rgId'..."

az resource update -g $labRg -n $labName --resource-type "Microsoft.DevTestLab/labs" --api-version 2018-10-15-preview --set properties.vmCreationResourceGroupId=$rgId

"Done. New virtual machines will now be created in the resource group '$vmRg'."
```

Het script met de volgende opdracht (ResourceGroup.ps1 is het bestand met de voorgaande script) aanroepen: 

```PowerShell
.\ResourceGroup.ps1 -subId <subscriptionID> -labRg <labRGNAme> -labName <LanName> -vmRg <RGName> 
```

## <a name="use-azure-resource-manager-template"></a>Azure Resource Manager-sjabloon gebruiken
Als u van Azure Resource Manager-sjabloon gebruikmaakt om een lab te maken, gebruikt u de **vmCreationResourceGroupId** eigenschap in het gedeelte lab eigenschappen van de Resource Manager-sjabloon zoals in het volgende voorbeeld:

```json
        {
            "type": "microsoft.devtestlab/labs",
            "name": "[parameters('lab_name')]",
            "apiVersion": "2018_10_15_preview",
            "location": "eastus",
            "tags": {},
            "scale": null,
            "properties": {
                "vmCreationResourceGroupId": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>",
                "labStorageType": "Premium",
                "premiumDataDisks": "Disabled",
                "provisioningState": "Succeeded",
                "uniqueIdentifier": "6e6f668f-992b-435c-bac3-d328b745cd25"
            },
            "dependsOn": []
        },
```


## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen: 

- [Beleidsregels op te stellen voor een lab](devtest-lab-get-started-with-lab-policies.md)
- [Veelgestelde vragen](devtest-lab-faq.md)
