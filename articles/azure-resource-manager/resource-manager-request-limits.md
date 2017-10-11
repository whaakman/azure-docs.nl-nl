---
title: Azure Resource Manager-aanvraaglimieten | Microsoft Docs
description: Beschrijft hoe u met Azure Resource Manager-aanvragen beperking als abonnementen hebt bereikt.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: tomfitz
ms.openlocfilehash: 6d7eeaf460674c3ab98425a5412ffa465b9ffd1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="throttling-resource-manager-requests"></a>Beperking van Resource Manager-aanvragen
Voor elk abonnement en de tenant Resource Manager-limieten aanvragen voor 15.000 per uur lezen en schrijven van aanvragen naar 1200 per uur. Deze limieten gelden voor elk exemplaar van Azure Resource Manager; Er zijn meerdere exemplaren in elke Azure-regio en Azure Resource Manager wordt geïmplementeerd op alle Azure-regio's.  Dus in de praktijk limieten effectief veel hoger dan de zijn hierboven vermelde, als gebruiker worden aanvragen doorgaans afgehandeld door veel verschillende exemplaren.

Als uw toepassing of het script is deze limiet bereikt, moet u uw verzoeken om te beperken. In dit onderwerp leest u het bepalen van de resterende aanvragen voordat de limiet bereikt, en hoe moet worden gereageerd wanneer u hebt de limiet bereikt.

Wanneer u de limiet bereikt, wordt de HTTP-statuscode **429 te veel aanvragen**.

Het aantal aanvragen is afgestemd op uw abonnement of uw tenant. Als er meerdere, gelijktijdige toepassingen voor aanvragen die in uw abonnement, het aanvragen van deze toepassingen worden toegevoegd samen om te bepalen het aantal resterende aanvragen.

Verzoeken om abonnementen binnen het bereik zijn die de betrokken bij het doorgeven van uw abonnement-id, zoals bij het ophalen van de resource-in uw abonnement groepen. Aanvragen van de tenant die binnen het bereik van opnemen uw abonnements-id, zoals bij het ophalen van geldige Azure locaties niet.

## <a name="remaining-requests"></a>Overige aanvragen
U kunt het aantal resterende aanvragen door te onderzoeken antwoordheaders bepalen. Elke aanvraag bevat de waarden voor het aantal resterende lees- en schrijfaanvragen. De volgende tabel beschrijft de antwoordheaders die u voor deze waarden controleren kunt:

| Reactieheader | Beschrijving |
| --- | --- |
| x-MS-ratelimit-Remaining-Subscription-reads |Abonnement bereik leest resterende |
| x-MS-ratelimit-Remaining-Subscription-Writes |Abonnement bereik schrijft resterende |
| x-MS-ratelimit-Remaining-tenant-reads |Tenant bereik leest resterende |
| x-MS-ratelimit-Remaining-tenant-Writes |Tenant bereik schrijft resterende |
| x-MS-ratelimit-Remaining-Subscription-resource-Requests |Abonnement binnen het bereik van aanvragen van het type resource resterende.<br /><br />Deze headerwaarde wordt alleen geretourneerd als een service de standaardlimiet is genegeerd. Resource Manager wordt toegevoegd voor deze waarde in plaats van het abonnement lees- of schrijfbewerkingen. |
| x-MS-ratelimit-Remaining-Subscription-resource-ENTITIES-Read |Abonnement resource type verzameling aanvragen resterende binnen het bereik.<br /><br />Deze headerwaarde wordt alleen geretourneerd als een service de standaardlimiet is genegeerd. Deze waarde geeft het aantal resterende verzameling aanvragen (lijst met resources). |
| x-MS-ratelimit-Remaining-tenant-resource-Requests |Tenant aanvragen van het type resource resterende binnen het bereik.<br /><br />Deze header alleen voor aanvragen op niveau van de tenant is toegevoegd en alleen als een service heeft de standaardlimiet overschreven. Resource Manager wordt toegevoegd voor deze waarde in plaats van de tenant lees- of schrijfbewerkingen. |
| x-MS-ratelimit-Remaining-tenant-resource-ENTITIES-Read |Tenant resource type verzameling aanvragen resterende binnen het bereik.<br /><br />Deze header alleen voor aanvragen op niveau van de tenant is toegevoegd en alleen als een service heeft de standaardlimiet overschreven. |

## <a name="retrieving-the-header-values"></a>De headerwaarden worden opgehaald
Bij het ophalen van deze headerwaarden in uw code of het script is niet anders dan bij het ophalen van een headerwaarde. 

Bijvoorbeeld in **C#**, ophalen van de waarde van de header van een **HttpWebResponse** object met de naam **antwoord** met de volgende code:

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

In **PowerShell**, voor het ophalen van de waarde van de header van een bewerking Invoke-WebRequest.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Of wilt zien van de resterende aanvragen voor foutopsporing, kunt u opgeven als de **-Debug** parameter op uw **PowerShell** cmdlet.

```powershell
Get-AzureRmResourceGroup -Debug
```

Die veel waarden, inclusief de waarde van de volgende antwoord geretourneerd:

```powershell
...
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 14999
...
```

In **Azure CLI**, voor het ophalen van de waarde van de header met de uitgebreidere optie.

```azurecli
azure group list -vv --json
```

Die veel waarden, met inbegrip van de volgende object geretourneerd:

```azurecli
...
silly: returnObject
{
  "statusCode": 200,
  "header": {
    "cache-control": "no-cache",
    "pragma": "no-cache",
    "content-type": "application/json; charset=utf-8",
    "expires": "-1",
    "x-ms-ratelimit-remaining-subscription-reads": "14998",
    ...
```

## <a name="waiting-before-sending-next-request"></a>Wacht voordat het volgende aanvraag verzenden
Wanneer u de aanvraaglimiet heeft bereikt, Resource Manager retourneert de **429** HTTP-statuscode en een **opnieuw proberen na** waarde in de header. De **opnieuw proberen na** waarde geeft het aantal seconden dat uw toepassing moet wachten (of slaapstand) voordat u de volgende aanvraag verzendt. Als u een aanvraag verzenden voordat de waarde voor opnieuw proberen is verstreken, wordt uw aanvraag is niet verwerkt en wordt een nieuwe retry-waarde geretourneerd.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over limieten en quota's [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md).
* Zie voor meer informatie over het verwerken van asynchrone REST-aanvragen, [bijhouden asynchrone Azure bewerkingen](resource-manager-async-operations.md).
