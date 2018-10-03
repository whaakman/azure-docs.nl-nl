---
title: Oplossen van problemen met beperking van fouten in Azure | Microsoft Docs
description: Beperking van fouten, nieuwe pogingen en uitstel in het Azure Compute.
services: virtual-machines
documentationcenter: ''
author: changov
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.service: virtual-machines
ms.devlang: na
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: vashan, rajraj, changov
ms.openlocfilehash: 7a1c283820b1ddef0c85899d9b56b6dcc3ea4b95
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043132"
---
# <a name="troubleshooting-api-throttling-errors"></a>Oplossen van problemen met API beperkingsfouten 

Azure Compute-aanvragen kunnen worden beperkt op een abonnement en op basis van per regio om te helpen bij de algehele prestaties van de service. Wij garanderen dat alle aanroepen naar de Azure Compute Resourceprovider (CRP), die u beheert, resources onder Microsoft.Compute-naamruimte niet langer zijn dan de maximale toegestane snelheid voor API. Dit document beschrijft de beperking, meer informatie over het oplossen van problemen, beperking, API en de aanbevolen procedures om te voorkomen dat wordt beperkt.  

## <a name="throttling-by-azure-resource-manager-vs-resource-providers"></a>Beperking van Azure Resource Manager vs Resourceproviders  

Als de voordeur naar Azure biedt Azure Resource Manager de verificatie en de volgorde van de eerste validatie en beperking van alle binnenkomende API-aanvragen. Azure Resource Manager aanroep frequentielimieten toe en gerelateerde diagnostische HTTP-antwoordheaders worden beschreven [hier](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-request-limits).
 
Wanneer een Azure API App client een beperking-fout ontvangt, wordt de HTTP-status 429 te veel aanvragen. Om te begrijpen als de aanvraagbeperking wordt gedaan door Azure Resource Manager of een onderliggende resourceprovider, zoals CRP, Controleer de `x-ms-ratelimit-remaining-subscription-reads` voor GET-aanvragen en `x-ms-ratelimit-remaining-subscription-writes` antwoordheaders voor niet-GET-aanvragen. Als het resterende aantal aanroepen 0 nadert is van het abonnement algemene aanroep limiet is gedefinieerd door Azure Resource Manager bereikt. Activiteiten door alle Abonnementclients worden samen geteld. De beperking anders afkomstig is van de doel-resourceprovider (de verholpen door de `/providers/<RP>` segment van de aanvraag-URL). 

## <a name="call-rate-informational-response-headers"></a>Tarief informatief antwoordheaders aanroepen 

| Koptekst                            | Waarde-indeling                           | Voorbeeld                               | Beschrijving                                                                                                                                                                                               |
|-----------------------------------|----------------------------------------|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-ms-ratelimit-resterende-resource |```<source RP>/<policy or bucket>;<count>```| Microsoft.Compute/HighCostGet3Min;159 | Resterend aantal van de API-aanroepen voor het beperkingsbeleid die betrekking hebben op de bucket of bewerking resourcegroep met inbegrip van het doel van deze aanvraag                                                                   |
| x-ms-request-kosten               | ```<count>   ```                             | 1                                     | Het aantal oproep telt "kosten in rekening gebracht" voor deze HTTP-aanvraag voor de limiet van het beleid van toepassing. Dit is doorgaans 1. Batchaanvragen, bijvoorbeeld voor het schalen van een virtuele-machineschaalset, kunnen kosten in rekening gebracht meerdere aantallen. |


Houd er rekening mee dat een API-aanvraag kan worden onderworpen aan beleid voor meerdere aanvragen worden beperkt. Er is een afzonderlijke `x-ms-ratelimit-remaining-resource` koptekst voor elk beleid. 

Hier volgt een voorbeeld-reactie op de virtual machine scale set aanvraag verwijderen.

```
x-ms-ratelimit-remaining-resource: Microsoft.Compute/DeleteVMScaleSet3Min;107 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/DeleteVMScaleSet30Min;587 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/VMScaleSetBatchedVMRequests5Min;3704 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/VmssQueuedVMOperations;4720 
```

##<a name="throttling-error-details"></a>Beperking van de details van fout

De 429 HTTP-status wordt meestal gebruikt om een aanvraag weigeren omdat een aanroep van limiet is bereikt. Een typische bandbreedtebeperking foutbericht van Compute Resource Provider eruit als in het onderstaande voorbeeld (alleen relevante headers worden weergegeven):

```
HTTP/1.1 429 Too Many Requests
x-ms-ratelimit-remaining-resource: Microsoft.Compute/HighCostGet3Min;46
x-ms-ratelimit-remaining-resource: Microsoft.Compute/HighCostGet30Min;0
Retry-After: 1200
Content-Type: application/json; charset=utf-8
{
  "code": "OperationNotAllowed",
  "message": "The server rejected the request because too many requests have been received for this subscription.",
  "details": [
    {
      "code": "TooManyRequests",
      "target": "HighCostGet30Min",
      "message": "{\"operationGroup\":\"HighCostGet30Min\",\"startTime\":\"2018-06-29T19:54:21.0914017+00:00\",\"endTime\":\"2018-06-29T20:14:21.0914017+00:00\",\"allowedRequestCount\":800,\"measuredRequestCount\":1238}"
    }
  ]
}

```

Het beleid met het aantal resterende aanroepen van 0 is dit de bandbreedteregeling fout wordt geretourneerd. In dit geval is dat `HighCostGet30Min`. De algemene indeling van de antwoordtekst is de algemene indeling Azure Resource Manager API-fout (in overeenstemming met OData). De belangrijkste foutcode `OperationNotAllowed`, is een Compute Resource Provider wordt gebruikt voor het rapporteren van beperkingsfouten optreden (onder andere typen clientfouten). De `message` eigenschap van de interne fouten bevat een geserialiseerde JSON-structuur met de details van de schending van beperking.

Zoals hierboven geïllustreerd, elke beperking fout bevat de `Retry-After` koptekst, die voorziet de client van het minimum aantal seconden moet worden gewacht voordat opnieuw wordt geprobeerd de aanvraag. 

## <a name="best-practices"></a>Aanbevolen procedures 

- Probeer Azure-service API fouten niet opnieuw onvoorwaardelijk en/of direct. Vaak het geval is voor de clientcode toegang te krijgen tot een snelle herhalingslus wanneer er een fout optreedt die is niet mogelijk zijn voor het opnieuw proberen. Nieuwe pogingen wordt uiteindelijk tot uitputting van de aanroep van de toegestane limiet voor de doel-bewerking groep en van invloed zijn op andere clients van het abonnement. 
- In grote volumes API automation gevallen kunt u proactieve clientzijde zelf beperking implementeren wanneer het aantal beschikbare aanroep voor een bewerking doelgroep onder sommige lage drempelwaarde komt. 
- Wanneer het bijhouden van asynchrone bewerkingen, met inachtneming van de hints Retry-After-header. 
- Als de clientcode informatie over een bepaalde virtuele Machine moet, een query die virtuele machine rechtstreeks in plaats van alle virtuele machines in de resourcegroep die of het hele abonnement weergeven en vervolgens de benodigde VM verzamelen op de client. 
- Als clientcode nodig heeft voor virtuele machines, schijven en momentopnamen van een specifieke Azure-locatie, op basis van locatie vorm van de query gebruiken in plaats van query's alle abonnement VM's en vervolgens te filteren op locatie aan de clientzijde: `GET /subscriptions/<subId>/providers/Microsoft.Compute/locations/<location>/virtualMachines?api-version=2017-03-30` query naar de landinstellingen van de Resourceprovider voor Compute eindpunten. 
-   Bij het maken of bijwerken van de API-resources in het bijzonder, virtuele machines en virtuele-machineschaalsets, het is veel efficiënter om bij te houden van de geretourneerde asynchrone bewerking voltooid dan polling voor gebruikersbeleid op de bron-URL zelf (op basis van de `provisioningState`).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over richtlijnen voor opnieuw proberen voor andere services in Azure, [richtlijnen voor specifieke services voor opnieuw proberen](https://docs.microsoft.com/en-us/azure/architecture/best-practices/retry-service-specific)
