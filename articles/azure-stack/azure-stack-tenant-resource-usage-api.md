---
title: Resourcegebruik-API voor de tenantsleutel | Microsoft Docs
description: Referentie voor Resourcegebruik-API, die informatie over het gebruik van Azure Stack ophalen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: alfredop
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: db0cdfce5903b8866985b0545494351e2796d8e2
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55244620"
---
# <a name="tenant-resource-usage-api"></a>Resourcegebruik-API voor de tenantsleutel

Een tenant kunt u de Tenant-API gebruiken om de gegevens over brongebruik van tenant weer te geven. Deze API is consistent met de API voor het gebruik van Azure (momenteel in private preview).

U kunt de Windows PowerShell-cmdlet **Get-UsageAggregates** ophalen van gegevens over gebruik, zoals in Azure.

## <a name="api-call"></a>API-aanroep
### <a name="request"></a>Aanvraag
De aanvraag haalt de details van het verbruik voor de aangevraagde abonnementen en voor het aangevraagde tijdsbestek. Er is geen aanvraagtekst.

| **Methode** | **Aanvraag-URI** |
| --- | --- |
| GET |https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version= 2015-06-01-preview & continuationToken = {token-waarde} |

### <a name="arguments"></a>Argumenten
| **Argument** | **Beschrijving** |
| --- | --- |
| *armendpoint* |Azure Resource Manager-eindpunt van uw Azure Stack-omgeving. De Azure Stack-conventie is dat de naam van Azure Resource Manager-eindpunt in de indeling wordt `https://management.{domain-name}`. Bijvoorbeeld, voor de development kit, is de naam van het domein local.azurestack.external en vervolgens het Resource Manager-eindpunt is `https://management.local.azurestack.external`. |
| *subId* |Abonnements-ID van de gebruiker die ervoor dat de aanroep zorgt. U kunt deze API alleen voor query gebruiken voor het gebruik van één abonnement. Providers kunnen u de Provider Resource Usage API aan het querygebruik van de gebruiken voor alle tenants. |
| *reportedStartTime* |Begintijd van de query. De waarde voor *datum-/* moet in UTC en aan het begin van het uur, bijvoorbeeld: 13:00 uur. Voor dagelijkse aggregatie, moet u deze waarde ingesteld op middernacht UTC. De indeling is *escape* ISO 8601, bijvoorbeeld 2015-06-16T18% 3a53% 3a11% 2b00% 3a00Z, waarbij dubbele punt naar % 3a is escape en plus is aan % 2b escape zodat deze beschrijvende URI. |
| *reportedEndTime* |Eindtijd van de query. De beperkingen die betrekking hebben op *reportedStartTime* gelden ook voor dit argument. De waarde voor *reportedEndTime* mag niet in de toekomst. |
| *aggregationGranularity* |Optionele parameter waarmee twee aparte mogelijke waarden heeft: dag en uur. Als de waarden voorstellen, een geeft als resultaat de gegevens in de dagelijkse granulariteit en de andere is een oplossing die per uur. De dagelijkse is de standaardoptie. |
| *API-versie* |De versie van het protocol dat wordt gebruikt om deze aanvraag te doen. U moet 2015-06-01-preview. |
| *continuationToken* |Token opgehaald van de laatste aanroep aan het gebruik van API-provider. Dit token is vereist wanneer een antwoord groter dan 1000 regels is en het fungeert als een bladwijzer voor de voortgang. Als deze niet aanwezig is, de gegevens worden opgehaald vanaf het begin van de dag of uur, op basis van de granulatie doorgegeven. |

### <a name="response"></a>Antwoord
HAAL /subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 en reportedEndTime = 2015-06-01T00% 3a00% 3a00% 2b00% 3a00 & aggregationGranularity dagelijks = & api-versie 1.0 =

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
| *id* |De unieke ID van het totaal gebruik |
| *De naam* |Naam van het totaal gebruik |
| *type* |Resource-uitbreiding |
| *subscriptionId* |Abonnements-id van de Azure-gebruiker |
| *usageStartTime* |UTC-begintijd van de gebruik bucket waartoe dit gebruik statistische functie behoort |
| *usageEndTime* |UTC-eindtijd van de gebruik bucket waartoe dit gebruik statistische functie behoort |
| *instanceData* |Sleutel / waarde-paren van exemplaardetails (in een nieuwe indeling):<br>  *resourceUri*: Volledig gekwalificeerde resource-ID, zoals resourcegroepen en exemplaarnaam <br>  *Locatie*: Regio waarin deze service is uitgevoerd <br>  *tags*: Resourcetags die de gebruiker opgeeft <br>  *additionalInfo*: Typ voor meer informatie over de resource die is verbruikt, bijvoorbeeld de versie van het besturingssysteem of de afbeelding |
| *Hoeveelheid* |Hoeveelheid van het gebruik van resources die in deze periode zijn opgetreden |
| *meterId* |Unieke ID voor de resource die is verbruikt (ook wel genoemd *ResourceID*) |


## <a name="next-steps"></a>Volgende stappen
[Resourcegebruik-API voor providers](azure-stack-provider-resource-api.md)

[Veelgestelde vragen over het gebruik gerelateerde](azure-stack-usage-related-faq.md)

