---
title: Controleer Azure enterprise-inschrijving facturering van gegevens met REST-API | Microsoft Docs
description: Leer hoe u Azure REST API's gebruiken om te controleren van enterprise-inschrijving factureringsgegevens.
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
ms.author: erikre
ms.openlocfilehash: f4b5fccefb08597a13fcc91eda11bbf3eb122914
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394843"
---
# <a name="review-enterprise-enrollment-billing-using-rest-apis"></a>Enterprise-inschrijving facturering met behulp van REST-API's bekijken

Azure API's van Reporting helpt u bij bekijken en beheren van uw Azure-kosten.

In dit artikel leert u om op te halen van de informatie die is gekoppeld aan facturering-accounts, afdeling of enterprtise agreement (EA)-inschrijvingsaccounts met behulp van de Azure REST API's. 

## <a name="individual-account-billing"></a>Facturering voor afzonderlijke account

Informatie over het gebruik voor accounts in een afdeling ophalen:

```http
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

De `{billingAccountId}` -parameter is vereist en de ID voor de account moet bevatten.

De volgende headers zijn vereist: 

|Aanvraagheader|Beschrijving|  
|--------------------|-----------------|  
|*Content-Type:*|Vereist. Ingesteld op `application/json`.|  
|*Autorisatie:*|Vereist. Ingesteld op een geldige `Bearer` [API-sleutel](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based). |  

Dit voorbeeld toont een synchrone aanroep waarmee gegevens worden geretourneerd voor de huidige factureringscyclus. Uit prestatieoverwegingen synchrone aanroepen informatie geretourneerd voor de afgelopen maand.  U kunt ook aanroepen de [API asynchroon](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) om terug te keren gegevens gedurende 36 maanden.


## <a name="response"></a>Antwoord  

Statuscode 200 wordt (OK) geretourneerd voor een geslaagde respons, waarin een lijst met gedetailleerde kosten voor het account.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/BillingAccounts/1234/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        ...
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        "instanceName": "shared1",
        "instanceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-Web-eastasia/providers/Microsoft.Web/sites/shared1",
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        "isEstimated": false,
        ...
      }
    }
  ]
}
```  

In dit voorbeeld is afgekort; Zie [gebruiksdetails ophalen voor een factureringsaccount](/rest/api/consumption/usagedetails/listbybillingaccount) voor een volledige beschrijving van elk veld met de reactie en foutafhandeling.

## <a name="department-billing"></a>Afdeling facturering 

Informatie over het gebruik samengevoegd voor alle accounts in een afdeling ophalen. 

```http
GET https://management.azure.com/providers/Microsoft.Billing/departments/{departmentId}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

De `{departmentId}` -parameter is vereist en moet de ID voor de afdeling in het inschrijvingsaccount bevatten.

De volgende headers zijn vereist: 

|Aanvraagheader|Beschrijving|  
|--------------------|-----------------|  
|*Content-Type:*|Vereist. Ingesteld op `application/json`.|  
|*Autorisatie:*|Vereist. Ingesteld op een geldige `Bearer` [API-sleutel](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based). |  

Dit voorbeeld toont een synchrone aanroep waarmee gegevens worden geretourneerd voor de huidige factureringscyclus. Uit prestatieoverwegingen synchrone aanroepen informatie geretourneerd voor de afgelopen maand.  U kunt ook aanroepen de [API asynchroon](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) om terug te keren gegevens gedurende 36 maanden.

### <a name="response"></a>Antwoord  

Statuscode 200 wordt (OK) geretourneerd voor een geslaagde respons, waarin een lijst met informatie over het gedetailleerde gebruik en kosten voor een bepaalde facturering periode en de factuur-ID voor de afdeling.


Het volgende voorbeeld ziet de uitvoer van de REST-API voor afdeling `1234`.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/Departments/1234/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/providers/Microsoft.Billing/Departments/1234/providers/Microsoft.Billing/billingPeriods/201702",
        "invoiceId": "/providers/Microsoft.Billing/Departments/1234/providers/Microsoft.Billing/invoices/201703-123456789",
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        "instanceName": "shared1",
        "instanceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-Web-eastasia/providers/Microsoft.Web/sites/shared1",
        "instanceLocation": "eastasia",
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        ...
      }
    }
  ]
}
```  

In dit voorbeeld is afgekort; Zie [gebruiksdetails ophalen voor een afdeling](/rest/api/consumption/usagedetails/listbydepartment) voor een volledige beschrijving van elk veld met de reactie en foutafhandeling.

## <a name="enrollment-account-billing"></a>Facturering voor inschrijving

Ophalen van informatie over het gebruik voor de apparaatregistratie-account samengevoegd.

```http
GET GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/{enrollmentAccountId}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

De `{enrollmentAccountId}` -parameter is vereist en de ID voor de inschrijvingsaccount moet bevatten.

De volgende headers zijn vereist: 

|Aanvraagheader|Beschrijving|  
|--------------------|-----------------|  
|*Content-Type:*|Vereist. Ingesteld op `application/json`.|  
|*Autorisatie:*|Vereist. Ingesteld op een geldige `Bearer` [API-sleutel](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based). |  

Dit voorbeeld toont een synchrone aanroep waarmee gegevens worden geretourneerd voor de huidige factureringscyclus. Uit prestatieoverwegingen synchrone aanroepen informatie geretourneerd voor de afgelopen maand.  U kunt ook aanroepen de [API asynchroon](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) om terug te keren gegevens gedurende 36 maanden.

### <a name="response"></a>Antwoord  

Statuscode 200 wordt (OK) geretourneerd voor een geslaagde respons, waarin een lijst met informatie over het gedetailleerde gebruik en kosten voor een bepaalde facturering periode en de factuur-ID voor de afdeling.

Het volgende voorbeeld ziet de uitvoer van de REST-API voor enterprise-inschrijving `1234`.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/EnrollmentAccounts/1234/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/providers/Microsoft.Billing/EnrollmentAccounts/1234/providers/Microsoft.Billing/billingPeriods/201702",
        "invoiceId": "/providers/Microsoft.Billing/EnrollmentAccounts/1234/providers/Microsoft.Billing/invoices/201703-123456789",
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        ....
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        ...
      }
    }
  ]
}
``` 

In dit voorbeeld is afgekort; Zie [gebruiksdetails ophalen voor een inschrijvingsaccount](/rest/api/consumption/usagedetails/listbyenrollmentaccount) voor een volledige beschrijving van elk veld met de reactie en foutafhandeling.

## <a name="next-steps"></a>Volgende stappen 
- Beoordeling [Enterprise rapportageoverzicht](https://docs.microsoft.com/azure/billing/billing-enterprise-api)
- Onderzoeken [Enterprise facturering REST-API](https://docs.microsoft.com/rest/api/billing/)   
- [Aan de slag met REST API van Azure](https://docs.microsoft.com/rest/api/azure/)   
