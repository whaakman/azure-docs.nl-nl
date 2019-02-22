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
ms.date: 02/15/2019
ms.author: spelluru
ms.openlocfilehash: e4e2a01bbac7aebb70852b93c51c32933cc75eec
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56652175"
---
# <a name="specify-a-resource-group-for-lab-virtual-machines-in-azure-devtest-labs"></a>Geef een resourcegroep voor de virtuele machines in Azure DevTest Labs

Als de eigenaar van een lab, kunt u uw lab-virtuele machines worden gemaakt in een specifieke resourcegroep configureren. Deze functie kunt u in de volgende scenario's:

- Minder resourcegroepen die zijn gemaakt door labs in uw abonnement hebt.
- Zijn uw labs werken binnen een vaste set van resourcegroepen die u configureert.
- Tijdelijke oplossing voor beperkingen en -goedkeuringen vereist voor het maken van resourcegroepen binnen uw Azure-abonnement.
- Consolideren van uw labresources binnen een enkele resourcegroep voor het vereenvoudigen van deze resources tracking en toepassen van [beleid](../governance/policy/overview.md) om resources op het niveau van de resource te beheren.

Met deze functie kunt u een script gebruiken om op te geven van een nieuwe of bestaande resourcegroep in uw Azure-abonnement voor alle uw lab VM's. Azure DevTest Labs ondersteunt momenteel deze functie via een API.

## <a name="api-to-configure-a-resource-group-for-lab-vms"></a>API voor het configureren van een resourcegroep voor lab-virtuele machines
U hebt de volgende opties als de eigenaar van een lab bij het gebruik van deze API:

- Kies de **lab van resourcegroep** voor alle virtuele machines.
- Kies een **bestaande resourcegroep** dan de resourcegroep van de testomgeving voor alle virtuele machines.
- Voer een **nieuwe resourcegroep** naam voor alle virtuele machines.
- Ga door met behulp van het bestaande gedrag, waarin een resourcegroep is gemaakt voor elke virtuele machine in het lab.
 
Deze instelling is van toepassing op nieuwe virtuele machines die zijn gemaakt in het lab. Oudere VM's in uw testomgeving die zijn gemaakt in hun eigen resourcegroepen blijven ongewijzigd. Omgevingen die zijn gemaakt in uw testomgeving gaan om te blijven in hun eigen resourcegroepen.

Het gebruik van deze API:
- Gebruik API-versie **2018_10_15_preview**.
- Als u een nieuwe resourcegroep opgeeft, zorgt u ervoor dat u hebt **schrijfmachtigingen op resourcegroepen** in uw abonnement. Als u schrijfmachtigingen ontbreken, mislukken het maken van nieuwe virtuele machines in de opgegeven resourcegroep.
- Tijdens het gebruik van de API, doorgeven de **volledige resourcegroep-ID**. Bijvoorbeeld: `/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>`. Zorg ervoor dat de resourcegroep bevindt zich in hetzelfde abonnement bevinden als het lab. 

## <a name="use-powershell"></a>PowerShell gebruiken 
Het volgende voorbeeld ziet hoe u een PowerShell-script gebruiken om te maken van alle virtuele machines in een nieuwe resourcegroep.

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

Het script worden aangeroepen met behulp van de volgende opdracht uit. ResourceGroup.ps1 is het bestand dat het vorige script bevat:

```PowerShell
.\ResourceGroup.ps1 -subId <subscriptionID> -labRg <labRGNAme> -labName <LanName> -vmRg <RGName> 
```

## <a name="use-an-azure-resource-manager-template"></a>Een Azure Resource Manager-sjabloon gebruiken
Als u een Azure Resource Manager-sjabloon gebruikt om een lab te maken, gebruikt u de **vmCreationResourceGroupId** eigenschap in het gedeelte lab eigenschappen van uw sjabloon, zoals wordt weergegeven in het volgende voorbeeld:

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
                "uniqueIdentifier": "000000000f-0000-0000-0000-00000000000000"
            },
            "dependsOn": []
        },
```


## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen: 

- [Beleidsregels op te stellen voor een lab](devtest-lab-get-started-with-lab-policies.md)
- [Veelgestelde vragen](devtest-lab-faq.md)
