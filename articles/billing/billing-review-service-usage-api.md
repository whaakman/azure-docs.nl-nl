---
title: Bekijk het gebruik van Azure-service-resource met REST-API | Microsoft Docs
description: Leer hoe u Azure REST API's gebruiken om te controleren van Resourcegebruik van de Azure-service.
services: billing
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2018
ms.author: alleonar
ms.openlocfilehash: 1b7b1455413fb4886b317d468e6d278111c094b1
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "40225925"
---
# <a name="review-azure-resource-usage-using-the-rest-api"></a>Bekijk het gebruik van Azure-resource met behulp van de REST-API


Azure [verbruik API's](https://docs.microsoft.com/rest/api/consumption/) help de kosten en gebruik de gegevens voor uw Azure-resources te bekijken.

In dit artikel leert u hoe u ophalen en Verzamel informatie over het gebruik van resource voor resources in een Azure-resourcegroep, en hoe deze resultaten filteren op basis van [Azure resource manager-tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags).

## <a name="get-usage-for-a-resource-group"></a>Gebruik voor een resourcegroep ophalen

Als u gebruik van resources voor Computing, database en andere resources in een resourcegroep, gebruikt de `usageDetails` REST-bewerking en filter de resultaten per groep.

```http
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30&filter=properties/resourceGroup eq '{resource-group}]
Content-Type: application/json   
Authorization: Bearer
```

De `{subscription-id}` -parameter is vereist en moet een abonnements-ID die toegang hebben tot de {resource-group}-resourcegroep met een rol van lezer bevatten. 

De volgende headers zijn vereist: 

|Aanvraagheader|Beschrijving|  
|--------------------|-----------------|  
|*Content-Type:*| Vereist. Ingesteld op `application/json`. |  
|*Autorisatie:*| Vereist. Ingesteld op een geldige `Bearer` token. |

### <a name="response"></a>Antwoord  

Statuscode 200 (OK) wordt geretourneerd voor een geslaagde respons met een lijst van gebruiksstatistieken voor elke Azure-resource in de resourcegroep met de ID van de subscriptipon `00000000-0000-0000-0000-000000000000`.

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702",
        "invoiceId": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/invoices/201703-123456789",
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        "instanceName": "shared1",
        "instanceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group/providers/Microsoft.Web/sites/shared1",
        "instanceLocation": "eastasia",
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        "isEstimated": false,
        "meterId": "00000000-0000-0000-0000-000000000000",
        "partNumber": "Part Number 1",
        "resourceGuid": "00000000-0000-0000-0000-000000000000",
        "offerId": "Offer Id 1",
        "chargesBilledSeparately": true,
        "location": "EU West"
      }
    } ] }
```

## <a name="get-usage-for-tagged-resources"></a>Gebruik voor resources met tag ophalen

Ophalen van Resourcegebruik voor resources in geordend op label, gebruiken de `usageDetails` REST-bewerking en filter de resultaten door de tag de naam van de `$filter` queryparameter.

```http
https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Consumption/usageDetails?$filter=tags eq 'tag1'&api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

De `{subscription-id}` -parameter is vereist en moet een abonnements-ID dat toegang heeft tot de resources met tag bevatten.


### <a name="response"></a>Antwoord  

Statuscode 200 (OK) wordt geretourneerd voor een geslaagde respons met een lijst van gebruiksstatistieken voor elke Azure-resource in de resourcegroep met de ID van de subscriptipon `00000000-0000-0000-0000-000000000000` en tag de naam van sleutelkluis paar is `dev` en `tools`. 

Het voorbeeldantwoord:

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "tags": {
        "dev": "tools"
      },
      "properties": {
        "billingPeriodId": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702",
        "invoiceId": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/invoices/201703-123456789",
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        "instanceName": "shared1",
        "instanceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-Web-eastasia/providers/Microsoft.Web/sites/shared1",
        "instanceLocation": "eastasia",
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        "isEstimated": false,
        "meterId": "00000000-0000-0000-0000-000000000000",
        "partNumber": "Part Number 1",
        "resourceGuid": "00000000-0000-0000-0000-000000000000",
        "offerId": "Offer Id 1",
        "chargesBilledSeparately": true,
        "location": "EU West"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen
- [Aan de slag met REST API van Azure](https://docs.microsoft.com/rest/api/azure/)   
