---
title: Azure-SKU niet beschikbaar '-fouten | Microsoft Docs
description: Beschrijft hoe u om op te lossen van de SKU niet beschikbaar-fout tijdens de implementatie.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/19/2018
ms.author: tomfitz
ms.openlocfilehash: 4688acbb2742579e0f9f3fbb2604ffd8ef12bfd5
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55081038"
---
# <a name="resolve-errors-for-sku-not-available"></a>Los de fouten voor de SKU is niet beschikbaar

In dit artikel wordt beschreven hoe u om op te lossen de **SkuNotAvailable** fout. Als u bent niet een geschikte SKU gevonden in deze regio of een andere regio die voldoet aan uw bedrijf nodig heeft, dien een [SKU aanvraag](https://aka.ms/skurestriction) tot Azure Support.


## <a name="symptom"></a>Symptoom

Bij het implementeren van een resource (doorgaans een virtuele machine), ontvangt u de volgende code en het volgende foutbericht:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

## <a name="cause"></a>Oorzaak

U ontvangt deze foutmelding wanneer de SKU die u hebt geselecteerd (zoals VM-grootte) van de resource is niet beschikbaar voor de locatie die u hebt geselecteerd.

## <a name="solution-1---powershell"></a>Oplossing 1: PowerShell

Om te bepalen welke SKU's zijn beschikbaar in een regio, gebruikt u de [Get-AzComputeResourceSku](/powershell/module/az.compute/get-azcomputeresourcesku) opdracht. Filter de resultaten per locatie. U kunt de meest recente versie van PowerShell voor deze opdracht moet hebben.

```azurepowershell-interactive
Get-AzComputeResourceSku | where {$_.Locations -icontains "centralus"}
```

De resultaten bevatten een lijst van SKU's voor de locatie en beperkingen voor deze SKU. U ziet dat een SKU kan worden weergegeven als `NotAvailableForSubscription`.

```powershell
ResourceType          Name        Locations   Restriction                      Capability           Value
------------          ----        ---------   -----------                      ----------           -----
virtualMachines       Standard_A0 centralus   NotAvailableForSubscription      MaxResourceVolumeMB   20480
virtualMachines       Standard_A1 centralus   NotAvailableForSubscription      MaxResourceVolumeMB   71680
virtualMachines       Standard_A2 centralus   NotAvailableForSubscription      MaxResourceVolumeMB  138240
```

## <a name="solution-2---azure-cli"></a>Oplossing 2 - Azure CLI

Om te bepalen welke SKU's zijn beschikbaar in een regio, gebruikt u de `az vm list-skus` opdracht. Gebruik de `--location` parameter voor het filteren van uitvoer naar de locatie die u gebruikt. Gebruik de `--size` parameter om te zoeken door de groottenaam van een gedeeltelijke.

```azurecli-interactive
az vm list-skus --location southcentralus --size Standard_F --output table
```

De opdracht retourneert resultaten, zoals:

```azurecli
ResourceType     Locations       Name              Zones    Capabilities    Restrictions
---------------  --------------  ----------------  -------  --------------  --------------
virtualMachines  southcentralus  Standard_F1                ...             None
virtualMachines  southcentralus  Standard_F2                ...             None
virtualMachines  southcentralus  Standard_F4                ...             None
...
```


## <a name="solution-3---azure-portal"></a>Oplossing 3 - Azure portal

Om te bepalen welke SKU's zijn beschikbaar in een regio, gebruikt u de [portal](https://portal.azure.com). Aanmelden bij de portal en voeg een resource via de interface toe. Als u de waarden hebt ingesteld, ziet u de beschikbare SKU's voor die bron. U hoeft niet te voltooien van de implementatie.

Bijvoorbeeld: het proces voor het maken van een virtuele machine starten. Andere beschikbare grootte Selecteer **grootte wijzigen**.

![VM maken](./media/resource-manager-sku-not-available-errors/create-vm.png)

U kunt filteren en blader door de beschikbare grootten.

![Beschikbare SKU 's](./media/resource-manager-sku-not-available-errors/available-sizes.png)

## <a name="solution-4---rest"></a>Oplossing 4 - REST

Om te bepalen welke SKU's zijn beschikbaar in een regio, gebruikt u de [Resource-SKU's - lijst](/rest/api/compute/resourceskus/list) bewerking.

Deze retourneert beschikbare SKU's en regio's in de volgende indeling:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    ...
  ]
}
```

