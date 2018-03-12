---
title: Provider Resourcegebruik API | Microsoft Docs
description: Verwijzing voor het gebruik van bronnen API, die informatie over het gebruik van Azure-Stack opgehaald
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: 763b0af9c258a70392e8c7ebbb4c107e94fce5b2
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="provider-resource-usage-api"></a>Resourcegebruik-API voor providers
De term *provider* geldt voor de servicebeheerder en voor alle providers gedelegeerd. Azure Stack-operators en gedelegeerd providers kunnen u de provider gebruiks-API gebruiken om het gebruik van hun directe tenants weer te geven. Bijvoorbeeld, zoals in het diagram, P0 de serviceprovider-API voor van gebruiksinformatie over de P1 kunt aanroepen en rechtstreeks gebruik van P2 en P1 kunt bellen voor informatie over het gebruik van P3 en P4.

![Conceptuele model van de provider-hiërarchie](media/azure-stack-provider-resource-api/image1.png)

## <a name="api-call-reference"></a>Naslaginformatie over de API-aanroep
### <a name="request"></a>Aanvraag
De aanvraag opgehaald verbruik details voor de aangevraagde abonnementen en voor de aangevraagde periode. Er is geen aanvraaginhoud.

Dit gebruik API is een provider API, zodat de aanroeper moet een eigenaar, bijdrager of lezer rol in het abonnement van de provider worden toegewezen.

| **Methode** | **Aanvraag-URI** |
| --- | --- |
| TOEVOEGEN |https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&subscriberId={sub1.1}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumenten
| **Argument** | **Beschrijving** |
| --- | --- |
| *armendpoint* |Azure Resource Manager-eindpunt van uw Azure-Stack-omgeving. Een conventie voor de Azure-Stack is dat de naam van de Azure Resource Manager-eindpunt in de indeling `https://adminmanagement.{domain-name}`. Bijvoorbeeld: voor de development kit als de domeinnaam is *local.azurestack.external*, dan is het Resource Manager-eindpunt `https://adminmanagement.local.azurestack.external`. |
| *subId* |Abonnement-ID van de gebruiker die de oproep doet. |
| *reportedStartTime* |Begintijd van de query. De waarde voor *DateTime* moet in Coordinated Universal Time (UTC) en aan het begin van het uur, bijvoorbeeld: 13:00. Voor dagelijkse aggregatie, moet u deze waarde ingesteld op middernacht UTC. De indeling is *escape-teken* ISO 8601. Bijvoorbeeld, *2015-06-16T18% 3a53% 3a11% 2b00% 3a00Z*, waarbij de dubbele punt is ontsnapt naar *% 3a* en het plusteken is ontsnapt naar *% 2b* zodat deze beschrijvende URI. |
| *reportedEndTime* |Eindtijd van de query. De beperkingen die betrekking hebben op *reportedStartTime* ook van toepassing op dit argument. De waarde voor *reportedEndTime* kan niet in de toekomst of de huidige datum. Als het, is het resultaat ingesteld op "verwerking niet voltooid." |
| *aggregationGranularity* |Optionele parameter waarmee twee aparte mogelijke waarden heeft: dag en uur. Als de waarden worden voorgesteld, een retourneert de gegevens in dagelijkse samenvattingen en de andere is een oplossing die per uur. De dagelijkse is de standaardoptie. |
| *subscriberId* |Abonnements-ID. Als u de gefilterde gegevens, is de abonnement-ID van een directe tenant van de provider vereist. Als er geen abonnement-ID-parameter is opgegeven, retourneert de aanroep van gebruiksgegevens voor directe tenants van de provider. |
| *API-versie* |De versie van het protocol dat wordt gebruikt voor het maken van deze aanvraag. Deze waarde is ingesteld op *2015-06-01-preview*. |
| *continuationToken* |Token opgehaald uit de laatste aanroep aan de provider gebruiks-API. Dit token is vereist wanneer een antwoord groter dan 1000 regels is en het fungeert als een bladwijzer voor de voortgang. Als het token niet aanwezig is is, de gegevens worden opgehaald van het begin van de dag of uur, op basis van de granulatie doorgegeven. |

### <a name="response"></a>Antwoord
GET /subscriptions/sub1/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00&reportedEndTime=2015-06-01T00%3a00%3a00%2b00%3a00&aggregationGranularity=Daily&subscriberId=sub1.1&api-version=1.0

```json
{
"value": [
{

"id":
"/subscriptions/sub1.1/providers/Microsoft.Commerce/UsageAggregate/sub1.1-

meterID1",
"name": "sub1.1-meterID1",
"type": "Microsoft.Commerce/UsageAggregate",

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
| *id* |De unieke ID van de statistische informatie over het gebruik. |
| *Naam* |Naam van de statistische informatie over het gebruik. |
| *type* |Resourcedefinitie. |
| *abonnements-id* |Abonnement-id van de Azure-Stack-gebruiker. |
| *usageStartTime* |UTC begintijd van de informatie over het gebruik bucket waartoe deze aggregatie gebruik behoort.|
| *usageEndTime* |UTC-eindtijd van de informatie over het gebruik bucket waartoe deze aggregatie gebruik behoort. |
| *instanceData* |Sleutel-waardeparen van het exemplaardetails (in een nieuwe indeling):<br> *resourceUri*: volledig gekwalificeerde resource-ID, waaronder de resourcegroepen en de exemplaarnaam. <br> *locatie*: regio waarin deze service is uitgevoerd. <br> *labels*: resourcetags die zijn opgegeven door de gebruiker. <br> *aanvullende informatie*: meer informatie over de bron die is verbruikt, bijvoorbeeld, het type besturingssysteem versie of image. |
| *hoeveelheid* |Hoeveelheid resourceverbruik dat is opgetreden in deze periode. |
| *meterId* |Unieke ID voor de resource die is verbruikt (ook wel *ResourceID*). |


## <a name="retrieve-usage-information"></a>Ophalen van gebruiksgegevens

Als de gebruiksgegevens worden gegenereerd, moet u de resources die zijn uitgevoerd en dat actief met behulp van het systeem, bijvoorbeeld, een actieve virtuele machine of een opslagaccount met bepaalde gegevens enzovoort hebben. Als u niet zeker weet of u bronnen die worden uitgevoerd in Azure Stack Marketplace, een virtuele machine (VM) te implementeren en controleren van de virtuele machine wordt bewaking blade om te controleren of deze uitgevoerd. De volgende PowerShell-cmdlets gebruiken om de gebruiksgegevens weer te geven:

1. [Installeer PowerShell voor Azure-Stack.](azure-stack-powershell-install.md)
2. [Configureren van de gebruiker van de Azure-Stack](user/azure-stack-powershell-configure-user.md) of de [Azure Stack-operator](azure-stack-powershell-configure-admin.md) PowerShell-omgeving 
3. Voor het ophalen van de gebruiksgegevens, gebruiken de [Get-UsageAggregates](/powershell/module/azurerm.usageaggregates/get-usageaggregates) PowerShell-cmdlet:
```powershell
Get-UsageAggregates -ReportedStartTime "<Start time for usage reporting>" -ReportedEndTime "<end time for usage reporting>" -AggregationGranularity <Hourly or Daily>
```

## <a name="next-steps"></a>Volgende stappen
[Tenant-Resourcegebruik API-referentiemateriaal](azure-stack-tenant-resource-usage-api.md)

[Veelgestelde vragen over het gebruik van gerelateerde](azure-stack-usage-related-faq.md)
