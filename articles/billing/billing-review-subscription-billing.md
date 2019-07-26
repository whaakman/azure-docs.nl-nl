---
title: Facturerings gegevens van het Azure-abonnement bekijken met REST API | Microsoft Docs
description: Meer informatie over het gebruik van Azure REST Api's om de facturerings gegevens van het abonnement te bekijken.
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
ms.author: banders
ms.openlocfilehash: 8cfa429b18fb282f5c1f85d2fd1637704653b855
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443053"
---
# <a name="review-subscription-billing-using-rest-apis"></a>Facturering van abonnementen controleren met REST-Api's

Met Azure Reporting Api's kunt u uw Azure-kosten controleren en beheren.

Filters helpen u bij het aanpassen van de resultaten om te voldoen aan uw behoeften.

Hier vindt u informatie over het gebruik van een REST API voor het retour neren van de facturerings gegevens van het abonnement voor een bepaald datum bereik.

``` http
GET https://management.azure.com/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}/providers/Microsoft.Consumption/usageDetails?$filter=properties/usageEnd ge '${startDate}' AND properties/usageEnd le '${endDate}'
Content-Type: application/json
Authorization: Bearer
```

## <a name="build-the-request"></a>De aanvraag voor het samenstellen

De `{subscriptionID}` para meter is vereist en identificeert het doel abonnement.

De `{billingPeriod}` para meter is vereist en geeft een huidige [facturerings periode](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods).

De `${startDate}` para `${endDate}` meters en zijn vereist voor dit voor beeld, maar is optioneel voor het eind punt. Hiermee geeft u het datum bereik op als teken reeksen in de vorm van jjjj-mm-dd `'20180501'` ( `'20180615'`voor beelden: en).

De volgende headers zijn vereist:

|Aanvraag header|Description|
|--------------------|-----------------|
|*Inhouds type:*|Vereist. Ingesteld op `application/json`.|
|*Authorization:*|Vereist. Ingesteld op een geldig `Bearer` [toegangs token](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

## <a name="response"></a>Antwoord

Status code 200 (OK) wordt geretourneerd voor een geslaagde reactie, die een lijst met gedetailleerde kosten voor uw account bevat.

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

Elk item in **waarde** vertegenwoordigt een Details over het gebruik van een service:

|Eigenschap Response|Description|
|----------------|----------|
|**subscriptionGuid** | Wereld wijd unieke ID voor het abonnement. |
|**Begin** | De datum waarop het gebruik is gestart. |
|**endDate** | De datum waarop het gebruik is beëindigd. |
|**useageQuantity** | Gebruikte hoeveelheid. |
|**billableQuantity** | Werkelijk Gefactureerd aantal. |
|**pretaxCost** | Gefactureerde kosten, vóór toepas bare belastingen. |
|**meterDetails** | Gedetailleerde informatie over het gebruik. |
|**nextLink**| Als deze instelling is ingesteld, geeft u een URL op voor de volgende ' pagina ' van Details. Leeg wanneer de pagina de laatste is. |

Dit voor beeld is afgekort; Zie [Details](https://docs.microsoft.com/rest/api/consumption/usagedetails/list#usagedetailslistforbillingperiod) van het gebruik van een lijst voor een volledige beschrijving van elk antwoord veld.

Andere status codes wijzen op fout condities. In deze gevallen wordt in het reactie object uitgelegd waarom de aanvraag is mislukt.

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
- [Overzicht van ENTER prise Reporting](https://docs.microsoft.com/azure/billing/billing-enterprise-api) bekijken
- [Facturerings rest API voor ondernemingen](https://docs.microsoft.com/rest/api/billing/) onderzoeken
- [Aan de slag met Azure REST API](https://docs.microsoft.com/rest/api/azure/)
