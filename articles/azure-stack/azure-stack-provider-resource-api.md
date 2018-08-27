---
title: Provider Resourcegebruik-API | Microsoft Docs
description: Verwijzing voor het Resourcegebruik-API, die worden opgehaald van de informatie over het gebruik van Azure Stack
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
ms.date: 08/24/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: aedaa729ec51d7b60b2c242239935f7b3e41794f
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918184"
---
# <a name="provider-resource-usage-api"></a>Resourcegebruik-API voor providers
De term *provider* geldt voor de servicebeheerder en voor elke gedelegeerde providers. Azure Stack-operators en gedelegeerde providers kunnen u de API voor het providergebruik gebruiken om het gebruik van hun directe tenants weer te geven. Bijvoorbeeld, zoals weergegeven in het diagram, P0 de API voor het ophalen van informatie over het gebruik van de P1-provider kan aanroepen en rechtstreeks gebruik van P2 en P1 voor informatie over het gebruik van P3 en P4 kunt aanroepen.

![Conceptuele model van de provider-hiërarchie](media/azure-stack-provider-resource-api/image1.png)

## <a name="api-call-reference"></a>Naslaginformatie over API-aanroep
### <a name="request"></a>Aanvragen
De aanvraag haalt de details van het verbruik voor de aangevraagde abonnementen en voor het aangevraagde tijdsbestek. Er is geen aanvraagtekst.

Dit gebruik API is een serviceprovider-API, zodat de aanroeper een eigenaar, bijdrager of lezer rol bij het abonnement van de provider moet worden toegewezen.

| **Methode** | **Aanvraag-URI** |
| --- | --- |
| GET |https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity} & subscriberId = {sub1.1} & api-version = 2015-06-01-preview & continuationToken = {token-waarde} |

### <a name="arguments"></a>Argumenten
| **Argument** | **Beschrijving** |
| --- | --- |
| *armendpoint* |Azure Resource Manager-eindpunt van uw Azure Stack-omgeving. De Azure Stack-conventie is dat de naam van de Azure Resource Manager-eindpunt in de indeling wordt `https://adminmanagement.{domain-name}`. Bijvoorbeeld: voor de development kit, als de domeinnaam is *local.azurestack.external*, dan is het Resource Manager-eindpunt `https://adminmanagement.local.azurestack.external`. |
| *subId* |Abonnements-ID van de gebruiker die de aanroep doet. |
| *reportedStartTime* |Begintijd van de query. De waarde voor *datum-/* moet in Coordinated Universal Time (UTC) en aan het begin van het uur, bijvoorbeeld: 13:00 uur. Voor dagelijkse aggregatie, moet u deze waarde ingesteld op middernacht UTC. De indeling is *escape* ISO 8601. Bijvoorbeeld, *2015-06-16T18% 3a53% 3a11% 2b00% 3a00Z*, waarbij de dubbele punt is escape naar *% 3a* en het plusteken is escape op *% 2b* zodat deze URI beschrijvende. |
| *reportedEndTime* |Eindtijd van de query. De beperkingen die betrekking hebben op *reportedStartTime* gelden ook voor dit argument. De waarde voor *reportedEndTime* mag niet in de toekomst of de huidige datum. Als dit het geval is, is het resultaat ingesteld op "verwerken is niet voltooid." |
| *aggregationGranularity* |Optionele parameter waarmee twee aparte mogelijke waarden heeft: dag en uur. Als de waarden voorstellen, een geeft als resultaat de gegevens in de dagelijkse granulariteit en de andere is een oplossing die per uur. De dagelijkse is de standaardoptie. |
| *subscriberId* |Abonnements-ID. Als u de gefilterde gegevens, zijn de abonnements-ID van een directe tenant van de provider is vereist. Als er geen abonnement-ID-parameter is opgegeven, retourneert de aanroep van gebruiksgegevens voor directe tenants van de provider. |
| *API-versie* |De versie van het protocol dat wordt gebruikt om deze aanvraag te doen. Deze waarde is ingesteld op *2015-06-01-preview*. |
| *continuationToken* |Token opgehaald van de laatste aanroep aan de gebruiks-API-provider. Dit token is vereist wanneer een antwoord groter dan 1000 regels is en het fungeert als een bladwijzer voor de voortgang. Als het token niet aanwezig is, de gegevens worden opgehaald vanaf het begin van de dag of uur, op basis van de granulatie doorgegeven. |

### <a name="response"></a>Reactie
HAAL /subscriptions/sub1/providers/Microsoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 en reportedEndTime = 2015-06-01T00% 3a00% 3a00% 2b00% 3a00 & aggregationGranularity = dagelijks & subscriberId = sub1.1 & api-versie 1.0 =

```json
{
"value": [
{

"id":
"/subscriptions/sub1.1/providers/Microsoft.Commerce.Admin/UsageAggregate/sub1.1-

meterID1",
"name": "sub1.1-meterID1",
"type": "Microsoft.Commerce.Admin/UsageAggregate",

"properties": {
"subscriptionId":"sub1.1",
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
| *id* |De unieke ID van het gebruik van statistische functie. |
| *De naam* |De naam van het gebruik van statistische functie. |
| *type* |De resourcedefinitie van de. |
| *Abonnements-id* |Abonnements-id van de Azure Stack-gebruiker. |
| *usageStartTime* |UTC-begintijd van de gebruik bucket waartoe dit gebruik statistische functie behoort.|
| *usageEndTime* |UTC-eindtijd van de gebruik bucket waartoe dit gebruik statistische functie behoort. |
| *instanceData* |Sleutel / waarde-paren van exemplaardetails (in een nieuwe indeling):<br> *resourceUri*: volledig gekwalificeerde resource-ID, met inbegrip van de resourcegroepen en de naam van het exemplaar. <br> *locatie*: regio waarin deze service is uitgevoerd. <br> *tags*: resourcetags die zijn opgegeven door de gebruiker. <br> *aanvullende informatie*: meer informatie over de resource die is verbruikt, bijvoorbeeld, het type besturingssysteem versie of de afbeelding. |
| *Hoeveelheid* |Hoeveelheid van het gebruik van resources die in deze periode zijn opgetreden. |
| *meterId* |Unieke ID voor de resource die is verbruikt (ook wel genoemd *ResourceID*). |


## <a name="retrieve-usage-information"></a>Gebruiksgegevens ophalen

### <a name="powershell"></a>PowerShell

Voor het genereren van gegevens over gebruik, moet u de resources die worden uitgevoerd en gebruikt het systeem actief bijvoorbeeld, een actieve virtuele machine of een opslagaccount met enkele gegevens enzovoort hebben. Als u niet zeker weet of u geen bronnen die worden uitgevoerd in Azure Stack Marketplace, een virtuele machine (VM) implementeren en controleren van de virtuele machine wordt monitoring blade om te controleren of deze uitgevoerd. De volgende PowerShell-cmdlets gebruiken om gegevens over gebruik weer te geven:

1. [Installeer PowerShell voor Azure Stack.](azure-stack-powershell-install.md)
2. [Configureren van de Azure Stack-gebruiker](user/azure-stack-powershell-configure-user.md) of de [Azure Stack-operators](azure-stack-powershell-configure-admin.md) PowerShell-omgeving 
3. Als u wilt ophalen van gegevens over gebruik, gebruikt u de [Get-UsageAggregates](/powershell/module/azurerm.usageaggregates/get-usageaggregates) PowerShell-cmdlet:
```powershell
Get-UsageAggregates -ReportedStartTime "<Start time for usage reporting>" -ReportedEndTime "<end time for usage reporting>" -AggregationGranularity <Hourly or Daily>
```
### <a name="rest-api"></a>REST API

U kunt informatie over het gebruik voor abonnementen verwijderd door het aanroepen van de service Microsoft.Commerce.Admin verzamelen. 

**Als u wilt retourneren van alle tenantgebruik voor verwijderd voor actieve gebruikers:**

| **Methode** | **Aanvraag-URI** |
| --- | --- |
| GET | https://{armendpoint}/subscriptions/{subId}/providersMicrosoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime={start-time}&reportedEndTime={end-endtime}&aggregationGranularity=Hourly&api-version= 2015-06-01-preview |

**Om terug te keren het gebruik van verwijderde of actieve tenant:**

| **Methode** | **Aanvraag-URI** |
| --- | --- |
| GET |https://{armendpoint}/subscriptions/{subId}/providersMicrosoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime={start-time}&reportedEndTime={end-endtime}&aggregationGranularity=Hourly&subscriberId={ abonnements-id} & api-version = 2015-06-01-preview |


## <a name="next-steps"></a>Volgende stappen
[Tenant-Resourcegebruik API-verwijzing](azure-stack-tenant-resource-usage-api.md)

[Veelgestelde vragen over het gebruik gerelateerde](azure-stack-usage-related-faq.md)
