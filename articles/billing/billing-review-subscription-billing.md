---
title: Controleer de factureringsgegevens Azure-abonnement met de REST-API | Microsoft Docs
description: Informatie over het gebruik van Azure REST API's voor facturerings-abonnementsgegevens.
services: billing
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: alleonar
ms.openlocfilehash: cc29d1f613af67604d50654be794cc90080098bb
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063853"
---
# <a name="review-subscription-billing-using-rest-apis"></a>Abonnement facturering met REST API's bekijken

Azure rapportage-API's helpen u te controleren en beheren van uw Azure kosten.  

Resultaten om te voldoen aan uw behoeften aanpassen met behulp van filters.

Hier kunt u informatie over een REST-API gebruiken om te retourneren van facturering details van abonnement voor een bepaald datumbereik.

``` http
GET https://management.azure.com/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}/providers/Microsoft.Consumption/usageDetails?$filter=properties/usageEnd ge '${startDate}' AND properties/usageEnd le '${endDate}'
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>De aanvraag voor het samenstellen  

De `{subscriptionID}` parameter is vereist en identificeert het doelabonnement.

De `{billingPeriod}` parameter is vereist en geeft een huidige [factureringsperiode](https://docs.microsoft.com/rest/api/billing/billingperiods/get#billingperiod).

De `${startDate}` en `${endDate}` parameters zijn vereist voor dit voorbeeld, maar is optioneel voor het eindpunt.  Ze het datumbereik opgeven als tekenreeksen in de vorm van jjjj-MM-DD (voorbeelden: `'20180501'` en `'20180615'`). 

De volgende headers zijn vereist: 

|Aanvraag-header|Beschrijving|  
|--------------------|-----------------|  
|*Content-Type:*|Vereist. Ingesteld op `application/json`.|  
|*Autorisatie:*|Vereist. Ingesteld op een geldige `Bearer` [toegangstoken](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |  

## <a name="response"></a>Antwoord  

Statuscode 200 wordt (OK) geretourneerd voor een geslaagde reactie die een lijst met gedetailleerde kosten voor uw account bevat.

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
        "usageQuantity": ${usageQuantity},
        "billableQuantity": ${billableQuantity},
        "pretaxCost": ${cost},
        "meterId": "${meterID}",
        "meterDetails": ${meterDetails}
      }
    }
    ],
    "nextLink": "${nextLinkURL}"
} 
```  

Elk item in **waarde** vertegenwoordigt een details over het gebruik van een service:

|Eigenschap Response|Beschrijving|
|----------------|----------|
|**subscriptionGuid** | Globaal unieke ID voor het abonnement. | 
|**Begindatum** | Datum van het gebruik gestart. |
|**EndDate bevat** | Datum van het gebruik is beëindigd. |
|**useageQuantity** | De hoeveelheid die wordt gebruikt. | 
|**billableQuantity** | De hoeveelheid daadwerkelijk in rekening gebracht. |
|**pretaxCost** | Kosten gefactureerd voordat de van toepassing zijnde belastingen. | 
|**meterDetails** | Gedetailleerde informatie over het gebruik. |
|**nextLink**| Als de waarde, een URL voor de volgende 'pagina"gegevens bevat. Leeg wanneer de pagina het laatste is. |  
||
  
In dit voorbeeld is afgekort; Zie [lijst informatie over het gebruik](https://docs.microsoft.com/rest/api/consumption/usagedetails/listbybillingperiod#usagedetailslistresult) voor een volledige beschrijving van elk antwoordveld. 

Andere statuscodes duiden op fouten. In deze gevallen is het object response wordt uitgelegd waarom de aanvraag is mislukt.

``` json
{  
  "error": [  
    { "code": "Error type." 
      "message": "Error response describing why the operation failed."  
    }  
  ]  
}  
```  

## <a name="next-steps"></a>Volgende stappen 
- Bekijk [Enterprise melden-overzicht](https://docs.microsoft.com/azure/billing/billing-enterprise-api)
- Onderzoek [Enterprise facturering REST-API](https://docs.microsoft.com/rest/api/billing/)   
- [Aan de slag met Azure REST-API](https://docs.microsoft.com/rest/api/azure/)   
