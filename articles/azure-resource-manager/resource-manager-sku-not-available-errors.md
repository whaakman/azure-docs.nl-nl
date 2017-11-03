---
title: Azure SKU niet beschikbaar fouten | Microsoft Docs
description: Hierin wordt beschreven hoe u problemen met de SKU niet beschikbaar-fout tijdens de implementatie.
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: 25cea4ae23471d182105ca3f720aaf74f81bf8c4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
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

## <a name="solution"></a>Oplossing

U lost dit probleem, moet u bepalen welke SKU's zijn beschikbaar in een regio. U kunt PowerShell, de portal of REST-bewerking gebruiken om te zoeken naar beschikbare SKU's.

### <a name="solution-1"></a>Oplossing 1

Gebruik de [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) opdracht in PowerShell. Filter de resultaten worden op locatie. U kunt de nieuwste versie van PowerShell voor deze opdracht moet hebben.

```powershell
Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("southcentralus")}
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

### <a name="solution-2"></a>Oplossing 2

In de Azure CLI, gebruikt u de `az vm list-skus` opdracht. Vervolgens kunt u `grep` of een vergelijkbaar hulpprogramma voor het filteren van de uitvoer.

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

### <a name="solution-3"></a>Oplossing 3

Gebruik de [portal](https://portal.azure.com). Aanmelden bij de portal en voeg een bron via de interface. Als u de waarden instelt, ziet u de beschikbare SKU's voor die bron. U hoeft niet om de implementatie te vervolledigen.

![beschikbare SKU 's](./media/resource-manager-sku-not-available-errors/view-sku.png)

### <a name="solution-4"></a>Oplossing 4

De REST-API gebruiken voor virtuele machines. De volgende aanvraag verzenden:

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