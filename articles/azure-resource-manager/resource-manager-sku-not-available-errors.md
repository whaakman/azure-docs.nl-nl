---
title: Azure SKU niet beschikbaar fouten | Microsoft Docs
description: Hierin wordt beschreven hoe u problemen met de SKU niet beschikbaar-fout tijdens de implementatie.
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
ms.date: 03/09/2018
ms.author: tomfitz
ms.openlocfilehash: 490c912a6abd6570c9bc74de8b86a516a8e6f807
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358758"
---
# <a name="resolve-errors-for-sku-not-available"></a>Los de fouten voor de SKU is niet beschikbaar

Dit artikel wordt beschreven voor het oplossen van de **SkuNotAvailable** fout.

## <a name="symptom"></a>Symptoom

Bij het implementeren van een resource (meestal een virtuele machine), wordt de volgende foutcode en een foutbericht weergegeven:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

## <a name="cause"></a>Oorzaak

U ontvangt deze foutmelding wanneer de SKU die u hebt geselecteerd (zoals VM-grootte) van de resource is niet beschikbaar voor de locatie die u hebt geselecteerd.

## <a name="solution-1---powershell"></a>Oplossing 1 - PowerShell

Om te bepalen die SKU's beschikbaar zijn in een regio, gebruikt de [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) opdracht. Filter de resultaten worden op locatie. U kunt de nieuwste versie van PowerShell voor deze opdracht moet hebben.

```powershell
Get-AzureRmComputeResourceSku | where {$_.Locations -icontains "southcentralus"}
```

De resultaten bevatten een lijst van SKU's voor de locatie en beperkingen voor deze SKU.

```powershell
ResourceType                Name      Locations Restriction                      Capability Value
------------                ----      --------- -----------                      ---------- -----
availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
virtualMachines      Standard_A0 southcentralus
virtualMachines      Standard_A1 southcentralus
virtualMachines      Standard_A2 southcentralus
```

## <a name="solution-2---azure-cli"></a>Oplossing 2: Azure CLI

Om te bepalen die SKU's beschikbaar zijn in een regio, gebruikt de `az vm list-skus` opdracht. Vervolgens kunt u `grep` of een vergelijkbaar hulpprogramma voor het filteren van de uitvoer.

```bash
$ az vm list-skus --output table
ResourceType      Locations           Name                    Capabilities                       Tier      Size           Restrictions
----------------  ------------------  ----------------------  ---------------------------------  --------  -------------  ---------------------------
availabilitySets  eastus              Classic                 MaximumPlatformFaultDomainCount=3
avilabilitySets   eastus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Aligned                 MaximumPlatformFaultDomainCount=3
```

## <a name="solution-3---azure-portal"></a>Oplossing 3 - Azure-portal

Om te bepalen die SKU's beschikbaar zijn in een regio, gebruikt de [portal](https://portal.azure.com). Aanmelden bij de portal en voeg een bron via de interface. Als u de waarden instelt, ziet u de beschikbare SKU's voor die bron. U hoeft niet om de implementatie te vervolledigen.

![beschikbare SKU 's](./media/resource-manager-sku-not-available-errors/view-sku.png)

## <a name="solution-4---rest"></a>4 - REST-oplossing

Om te bepalen die SKU's beschikbaar zijn in een regio, gebruikt u de REST-API voor virtuele machines. De volgende aanvraag verzenden:

```HTTP 
GET
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
```

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

Als u niet een geschikte SKU gevonden in deze regio of een alternatieve regio die voldoet aan uw bedrijf moet, dienen een [SKU aanvraag](https://aka.ms/skurestriction) voor ondersteuning van Azure.
