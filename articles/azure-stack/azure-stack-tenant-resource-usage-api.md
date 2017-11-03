---
title: Resourcegebruik API voor de tenantsleutel | Microsoft Docs
description: Verwijzing voor Resourcegebruik API, waarna de informatie over het gebruik van Azure-Stack opgehaald.
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: b9d7c7ee-e906-4978-92a3-a2c52df16c36
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2016
ms.author: alfredop
ms.openlocfilehash: f2eaf1c766d6c86741cf0fd561c131eacb34d782
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="tenant-resource-usage-api"></a>Tenant-Resourcegebruik API
Een tenant kunt u de Tenant-API gebruiken om de gegevens over brongebruik van tenant weer te geven. Deze API is consistent met de API voor het gebruik van Azure (momenteel in een afgeschermd voorbeeld).

U kunt de Windows PowerShell-cmdlet **Get-UsageAggregates** ophalen van gebruiksgegevens zoals in Azure.

## <a name="api-call"></a>API-aanroep
### <a name="request"></a>Aanvraag
De aanvraag opgehaald verbruik details voor de aangevraagde abonnementen en voor de aangevraagde periode. Er is geen aanvraaginhoud.

| **Methode** | **Aanvraag-URI** |
| --- | --- |
| TOEVOEGEN |https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version= 2015-06-01-preview & continuationToken = {token waarde} |

### <a name="arguments"></a>Argumenten
| **Argument** | **Beschrijving** |
| --- | --- |
| *Armendpoint* |Azure Resource Manager-eindpunt van uw Azure-Stack-omgeving. Een conventie voor de Azure-Stack is dat de naam van Azure Resource Manager-eindpunt in de notatie `https://management.{domain-name}`. Bijvoorbeeld: voor de development kit, de domeinnaam local.azurestack.external is, wordt het Resource Manager-eindpunt is `https://management.local.azurestack.external`. |
| *subId* |Abonnements-ID van de gebruiker die de oproep aan te brengen. U kunt deze API alleen aan de query voor het gebruik van één abonnement gebruiken. Providers kunnen u de Provider Resource gebruik API aan het gebruik van de query gebruiken voor alle tenants. |
| *reportedStartTime* |Begintijd van de query. De waarde voor *DateTime* moet in UTC en aan het begin van het uur, bijvoorbeeld: 13:00. Voor dagelijkse aggregatie, moet u deze waarde ingesteld op middernacht UTC. De indeling is *escape-teken* ISO 8601, bijvoorbeeld 2015-06-16T18% 3a53% 3a11% 2b00% 3a00Z, waarbij dubbele punt naar % 3a is ontsnapt en plus is ontsnapt aan % 2b zodat deze beschrijvende URI. |
| *reportedEndTime* |Eindtijd van de query. De beperkingen die betrekking hebben op *reportedStartTime* ook van toepassing op dit argument. De waarde voor *reportedEndTime* kan niet in de toekomst. |
| *aggregationGranularity* |Optionele parameter waarmee twee aparte mogelijke waarden heeft: dag en uur. Als de waarden worden voorgesteld, een retourneert de gegevens in dagelijkse samenvattingen en de andere is een oplossing die per uur. De dagelijkse is de standaardoptie. |
| *API-versie* |De versie van het protocol dat wordt gebruikt voor het maken van deze aanvraag. U moet 2015-06-01-preview. |
| *continuationToken* |Token opgehaald uit de laatste aanroep aan de gebruik API-provider. Dit token is vereist wanneer een antwoord groter dan 1000 regels is en het fungeert als een bladwijzer voor voortgang. Als deze niet aanwezig is, de gegevens worden opgehaald van het begin van de dag of uur, op basis van de granulatie doorgegeven. |

### <a name="response"></a>Antwoord
/Subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & reportedEndTime ophalen = 2015-06-01T00% 3a00% 3a00% 2b00% 3a00 & aggregationGranularity dagelijks = & api-versie 1.0 =

```json
{
"value": [
{

"id":
"/subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",
"name": "sub1-meterID1",
"type": "Microsoft.Commerce/UsageAggregate",

"properties": {
"subscriptionId":"sub1",
"usageStartTime": "2015-03-03T00:00:00+00:00",
"usageEndTime": "2015-03-04T00:00:00+00:00",
"instanceData":"{\"Microsoft.Resources\":{\"resourceUri\":\"resourceUri1\",\"location\":\"Alaska\",\"tags\":null,\"additionalInfo\":null}}",
"quantity":2.4000000000,
"meterId":"meterID1"

}
},

…
```

### <a name="response-details"></a>Details van de reactie
| **Argument** | **Beschrijving** |
| --- | --- |
| *ID* |Unieke ID van de statistische functie van gebruik |
| *naam* |Naam van de statistische functie van gebruik |
| *type* |Resource-uitbreiding |
| *abonnements-id* |Abonnement-id van de Azure-gebruiker |
| *usageStartTime* |UTC-begintijd van de informatie over het gebruik bucket waartoe deze aggregatie gebruik behoort |
| *usageEndTime* |UTC-eindtijd van de informatie over het gebruik bucket waartoe deze aggregatie gebruik behoort |
| *instanceData* |Sleutel-waardeparen van het exemplaardetails (in een nieuwe indeling):<br>  *resourceUri*: volledig gekwalificeerde resource-ID, met inbegrip van resourcegroepen en exemplaarnaam <br>  *locatie*: regio waarin deze service is uitgevoerd <br>  *labels*: resourcetags waarmee de gebruiker <br>  *aanvullende informatie*: meer details over de resource die is verbruikt, bijvoorbeeld, de versie- of image type besturingssysteem |
| *hoeveelheid* |Hoeveelheid resourceverbruik dat is opgetreden in deze periode |
| *meterId* |Unieke ID voor de resource die is verbruikt (ook wel *ResourceID*) |

## <a name="next-steps"></a>Volgende stappen
[Resourcegebruik-API voor providers](azure-stack-provider-resource-api.md)

[Veelgestelde vragen over het gebruik van gerelateerde](azure-stack-usage-related-faq.md)

