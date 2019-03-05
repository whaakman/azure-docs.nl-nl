---
title: Controleer de factureringsgegevens van Azure-abonnement met REST-API | Microsoft Docs
description: Leer hoe u Azure REST API's gebruiken om te controleren van factureringsgegevens abonnement.
services: billing
documentationcenter: na
author: lleonard-msft
manager: ''
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: erikre
ms.openlocfilehash: 3a487b56c3ce81f3a13add767a9bf7ad59cf79cd
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57315942"
---
# <a name="review-subscription-billing-using-rest-apis"></a>Abonnement facturering met behulp van REST-API's bekijken

Azure API's van Reporting helpt u bij bekijken en beheren van uw Azure-kosten.

Met behulp van filters resultaten om te voldoen aan uw behoeften aanpassen.

Hier kunt u informatie over het gebruik van een REST-API om terug te keren factureringsgegevens abonnement voor een bepaald datumbereik.

``` http
GET https://management.azure.com/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}/providers/Microsoft.Consumption/usageDetails?$filter=properties/usageEnd ge '${startDate}' AND properties/usageEnd le '${endDate}'
Content-Type: application/json
Authorization: Bearer
```

## <a name="build-the-request"></a>De aanvraag voor het samenstellen

De `{subscriptionID}` parameter is vereist en identificeert het doelabonnement.

De `{billingPeriod}` -parameter is vereist en geeft een huidige [factureringsperiode](https://docs.microsoft.com/rest/api/billing/billingperiods/get#billingperiod).

De `${startDate}` en `${endDate}` parameters zijn vereist voor dit voorbeeld, maar optionele voor het eindpunt. Ze het datumbereik opgeven als tekenreeksen in de vorm van DD-MM-jjjj (voorbeelden: `'20180501'` en `'20180615'`).

De volgende headers zijn vereist:

|Aanvraagheader|Description|
|--------------------|-----------------|
|*Content-Type:*|Vereist. Ingesteld op `application/json`.|
|*Autorisatie:*|Vereist. Ingesteld op een geldige `Bearer` [toegangstoken](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

## <a name="response"></a>Antwoord

Statuscode 200 wordt (OK) geretourneerd voor een geslaagde respons, waarin een lijst met gedetailleerde kosten voor uw account.

``` json
{
  "value": [
    {
      "id": "/subscriptions/{$subscriptionID}/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/{$detailsID}",
      "name": "{$detailsID}",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}",
        "invoiceId": "/subscriptions/${subscriptionID}/providers/Microsoft.Billing/invoices/${invoiceID}",
        "usageStart": "${startDate}}",
        "usageEnd": "${endDate}",
        "currency": "USD",
        "usageQuantity": "${usageQuantity}",
        "billableQuantity": "${billableQuantity}",
        "pretaxCost": "${cost}",
        "meterId": "${meterID}",
        "meterDetails": "${meterDetails}"
      }
    }
  ],
  "nextLink": "${nextLinkURL}"
}
```

Elk item in **waarde** vertegenwoordigt een details met betrekking tot het gebruik van een service:

|Antwoord-eigenschap|Description|
|----------------|----------|
|**subscriptionGuid** | Unieke ID voor het abonnement. |
|**startDate** | De datum van het gebruik aan de slag. |
|**endDate** | De datum van het gebruik is beëindigd. |
|**useageQuantity** | De hoeveelheid die wordt gebruikt. |
|**billableQuantity** | De hoeveelheid daadwerkelijk in rekening gebracht. |
|**pretaxCost** | Kosten gefactureerd, voordat u van toepassing zijnde belastingen. |
|**meterDetails** | Gedetailleerde informatie over het gebruik. |
|**nextLink**| Als de waarde, Hiermee geeft u een URL voor de volgende "page" van gegevens. Leeg wanneer de pagina de laatste is. |

In dit voorbeeld is afgekort; Zie [lijst met informatie over het gebruik](https://docs.microsoft.com/rest/api/consumption/usagedetails/listbybillingperiod#usagedetailslistresult) voor een volledige beschrijving van elk antwoordveld.

Andere statuscodes duiden op fouten. In dergelijke gevallen wordt het object response uitgelegd waarom de aanvraag is mislukt.

``` json
{
  "error": [
    {
      "code": "Error type.",
      "message": "Error response describing why the operation failed."
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen
- Beoordeling [Enterprise rapportageoverzicht](https://docs.microsoft.com/azure/billing/billing-enterprise-api)
- Onderzoeken [Enterprise facturering REST-API](https://docs.microsoft.com/rest/api/billing/)
- [Aan de slag met REST API van Azure](https://docs.microsoft.com/rest/api/azure/)
