---
title: Aanvraaglimieten en beperkingen - Azure Resource Manager
description: Beschrijft hoe u gebruik van beperking met Azure Resource Manager-aanvragen wanneer de limieten voor een abonnement is bereikt.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2018
ms.author: tomfitz
ms.custom: seodec18
ms.openlocfilehash: 0a4be349bfd8ce546ee2a27c206a7bd86306c27a
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55493555"
---
# <a name="throttling-resource-manager-requests"></a>Beperking van Resource Manager-aanvragen

Voor elke Azure-abonnement en de tenant staat Resource Manager maximaal 12.000 lezen aanvragen per uur en 1200 schrijven aanvragen per uur. Deze limieten zijn gericht op de principal-ID de aanvragen en de abonnements-ID of tenant-ID. Als uw aanvragen afkomstig is van meer dan één principal-ID, is de limiet voor het abonnement of de tenant is groter dan 12.000 en 1200 per uur.

Aanvragen worden toegepast op uw abonnement of uw tenant. Verzoeken om abonnementen zijn services die de betrokken bij het doorgeven van uw abonnement-ID, zoals het ophalen van de resourcegroepen in uw abonnement. Tenant-aanvragen zijn niet opgenomen uw abonnements-ID, zoals het ophalen van geldige Azure-locaties.

Deze limieten gelden voor elk exemplaar van Azure Resource Manager. Er zijn meerdere exemplaren in elke Azure-regio en Azure Resource Manager wordt geïmplementeerd voor alle Azure-regio's.  Dus in de praktijk limieten effectief veel hoger zijn dan deze limieten zijn, als gebruiker worden aanvragen doorgaans afgehandeld door veel verschillende exemplaren.

Als uw toepassing of script deze limiet is bereikt, moet u uw aanvragen beperken. In dit artikel leest u hoe u bepaalt de resterende aanvragen die u hebt voordat de limiet wordt bereikt, en om te reageren wanneer u de limiet hebt bereikt.

Wanneer u de limiet is bereikt, ontvangt u de HTTP-statuscode **429 te veel aanvragen**.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="remaining-requests"></a>Overige aanvragen
U kunt het aantal resterende aanvragen bepalen door onderzoeken antwoordheaders. Elke aanvraag bevat de waarden voor het aantal resterende lees- en schrijfaanvragen. De volgende tabel beschrijft de reactieheaders die u voor deze waarden kunt bekijken:

| Reactieheader | Description |
| --- | --- |
| x-MS-ratelimit-Remaining-Subscription-reads |Abonnement binnen het bereik van leest de resterende. Deze waarde wordt geretourneerd voor leesbewerkingen. |
| x-MS-ratelimit-Remaining-Subscription-Writes |Abonnement binnen het bereik van schrijft resterende. Deze waarde wordt geretourneerd op schrijfbewerkingen. |
| x-MS-ratelimit-Remaining-tenant-reads |Tenant binnen het bereik van leest resterend |
| x-MS-ratelimit-Remaining-tenant-Writes |Tenant binnen het bereik van schrijft resterend |
| x-MS-ratelimit-Remaining-Subscription-resource-Requests |Abonnement binnen het bereik van aanvragen van het type resource resterende.<br /><br />Deze headerwaarde wordt alleen geretourneerd als een service de standaardlimiet is opgeheven. Resource Manager wordt deze waarde in plaats van het abonnement lees- of schrijfbewerkingen toegevoegd. |
| x-MS-ratelimit-Remaining-Subscription-resource-ENTITIES-Read |Abonnement binnen het bereik van type verzameling resourceaanvragen resterende.<br /><br />Deze headerwaarde wordt alleen geretourneerd als een service de standaardlimiet is opgeheven. Deze waarde geeft het aantal resterende verzameling aanvragen (lijst met resources). |
| x-MS-ratelimit-Remaining-tenant-resource-Requests |Tenant binnen het bereik van aanvragen van het type resource resterende.<br /><br />Deze header is alleen voor aanvragen op tenantniveau toegevoegd en alleen als een service heeft de standaardlimiet overschreven. Resource Manager wordt deze waarde in plaats van de tenant lees- of schrijfbewerkingen toegevoegd. |
| x-MS-ratelimit-Remaining-tenant-resource-ENTITIES-Read |Type verzameling resourceaanvragen resterende binnen het bereik van tenant.<br /><br />Deze header is alleen voor aanvragen op tenantniveau toegevoegd en alleen als een service heeft de standaardlimiet overschreven. |

## <a name="retrieving-the-header-values"></a>Bij het ophalen van de waarden van de koptekst
Bij het ophalen van deze headerwaarden in uw code of het script is niet anders dan bij het ophalen van een headerwaarde. 

Bijvoorbeeld, in **C#**, haalt u de headerwaarde van een **HttpWebResponse** object met de naam **antwoord** door de volgende code:

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

In **PowerShell**, haalt u de headerwaarde uit een Invoke-WebRequest-bewerking.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Zie voor een compleet voorbeeld van PowerShell, [Resource Manager-limieten voor een abonnement controleren](https://github.com/Microsoft/csa-misc-utils/tree/master/psh-GetArmLimitsViaAPI).

Als u zien van de resterende aanvragen wilt voor foutopsporing, kunt u opgeven de **-Debug** parameter op uw **PowerShell** cmdlet.

```powershell
Get-AzResourceGroup -Debug
```

Die veel waarden, met inbegrip van de waarde van het volgende antwoord geretourneerd:

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 14999
```

Als u limieten voor schrijven, gebruikt u een schrijfbewerking: 

```powershell
New-AzResourceGroup -Name myresourcegroup -Location westus -Debug
```

Die veel waarden, met inbegrip van de volgende waarden geretourneerd:

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
Created

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-writes: 1199
```

In **Azure CLI**, haalt u de headerwaarde met de uitgebreidere optie.

```azurecli
az group list --verbose --debug
```

Die veel waarden, met inbegrip van de volgende waarden geretourneerd:

```azurecli
msrest.http_logger : Response status: 200
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Content-Encoding': 'gzip'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'Vary': 'Accept-Encoding'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-reads': '14998'
```

Als u limieten voor schrijven, gebruikt u een schrijfbewerking: 

```azurecli
az group create -n myresourcegroup --location westus --verbose --debug
```

Die veel waarden, met inbegrip van de volgende waarden geretourneerd:

```azurecli
msrest.http_logger : Response status: 201
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Length': '163'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-writes': '1199'
```

## <a name="waiting-before-sending-next-request"></a>Wacht voordat de volgende aanvraag verzenden
De aanvraaglimiet is bereikt, Resource Manager is, retourneert de **429** HTTP-statuscode en een **Retry-After** waarde in de header. De **Retry-After** waarde geeft het aantal seconden dat uw toepassing moet wachten (of slaapstand) voordat de volgende aanvraag wordt verzonden. Als u een aanvraag verzendt voordat de waarde voor opnieuw proberen is verstreken, wordt uw aanvraag is niet verwerkt en wordt een nieuwe waarde voor opnieuw proberen wordt geretourneerd.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een compleet voorbeeld van PowerShell, [Resource Manager-limieten voor een abonnement controleren](https://github.com/Microsoft/csa-misc-utils/tree/master/psh-GetArmLimitsViaAPI).
* Zie voor meer informatie over limieten en quota [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
* Zie voor meer informatie over het verwerken van asynchrone REST-aanvragen, [asynchrone bewerkingen van Azure bijhouden](resource-manager-async-operations.md).
