---
title: Facturerings gegevens voor Azure Enter prise-inschrijving door nemen met REST API | Microsoft Docs
description: Meer informatie over het gebruik van Azure REST Api's voor het controleren van de facturerings gegevens van de Enter prise-inschrijving.
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
ms.openlocfilehash: 25d9b48696dc2a83ea0ba77c1be2c7aad7627fff
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443152"
---
# <a name="review-enterprise-enrollment-billing-using-rest-apis"></a>De facturering van ENTER prise Enrollment controleren met behulp van REST Api's

Met Azure Reporting Api's kunt u uw Azure-kosten controleren en beheren.

In dit artikel vindt u informatie over het ophalen van de facturerings gegevens die zijn gekoppeld aan accounts voor facturerings accounts, afdelingen of ENTER prise Agreement (EA) met de Azure REST Api's. 

## <a name="individual-account-billing"></a>Facturering van afzonderlijke accounts

Details van het gebruik van accounts in een afdeling ophalen:

```http
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

De `{billingAccountId}` para meter is vereist en moet de id voor het account bevatten.

De volgende headers zijn vereist: 

|Aanvraag header|Description|  
|--------------------|-----------------|  
|*Inhouds type:*|Vereist. Ingesteld op `application/json`.|  
|*Authorization:*|Vereist. Stel in op een `Bearer` geldige [API-sleutel](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based). |  

In dit voor beeld ziet u een synchrone aanroep waarmee Details voor de huidige facturerings cyclus worden geretourneerd. Uit prestatie overwegingen retour neren synchrone informatie over de afgelopen maand.  U kunt de API ook [asynchroon](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) aanroepen om gegevens te retour neren voor 36 maanden.


## <a name="response"></a>Antwoord  

Status code 200 (OK) wordt geretourneerd voor een geslaagde reactie, die een lijst met gedetailleerde kosten voor het account bevat.

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

Dit voor beeld is afgekort; Zie [gebruiks gegevens voor een facturerings account ophalen](/rest/api/consumption/usagedetails/list#billingaccountusagedetailslist) voor een volledige beschrijving van elk antwoord veld en fout afhandeling.

## <a name="department-billing"></a>Facturering afdeling 

Statistische gegevens over gebruik ophalen voor alle accounts in een afdeling. 

```http
GET https://management.azure.com/providers/Microsoft.Billing/departments/{departmentId}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

De `{departmentId}` para meter is vereist en moet de id voor de afdeling in het inschrijvings account bevatten.

De volgende headers zijn vereist: 

|Aanvraag header|Description|  
|--------------------|-----------------|  
|*Inhouds type:*|Vereist. Ingesteld op `application/json`.|  
|*Authorization:*|Vereist. Stel in op een `Bearer` geldige [API-sleutel](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based). |  

In dit voor beeld ziet u een synchrone aanroep waarmee Details voor de huidige facturerings cyclus worden geretourneerd. Uit prestatie overwegingen retour neren synchrone informatie over de afgelopen maand.  U kunt de API ook [asynchroon](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) aanroepen om gegevens te retour neren voor 36 maanden.

### <a name="response"></a>Antwoord  

Status code 200 (OK) wordt geretourneerd voor een geslaagde reactie, die een lijst met gedetailleerde gebruiks gegevens en-kosten bevat voor een bepaalde facturerings periode en factuur-ID voor de afdeling.


In het volgende voor beeld ziet u de uitvoer van de `1234`rest API voor afdeling.

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

Dit voor beeld is afgekort; Zie [gebruiks gegevens voor een afdeling ophalen](/rest/api/consumption/usagedetails/list#departmentusagedetailslist) voor een volledige beschrijving van elk antwoord veld en fout afhandeling.

## <a name="enrollment-account-billing"></a>Facturering van inschrijvings account

Statistische gegevens over gebruik ophalen voor het inschrijvings account.

```http
GET GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/{enrollmentAccountId}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

De `{enrollmentAccountId}` para meter is vereist en moet de id voor het inschrijvings account bevatten.

De volgende headers zijn vereist: 

|Aanvraag header|Description|  
|--------------------|-----------------|  
|*Inhouds type:*|Vereist. Ingesteld op `application/json`.|  
|*Authorization:*|Vereist. Stel in op een `Bearer` geldige [API-sleutel](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based). |  

In dit voor beeld ziet u een synchrone aanroep waarmee Details voor de huidige facturerings cyclus worden geretourneerd. Uit prestatie overwegingen retour neren synchrone informatie over de afgelopen maand.  U kunt de API ook [asynchroon](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) aanroepen om gegevens te retour neren voor 36 maanden.

### <a name="response"></a>Antwoord  

Status code 200 (OK) wordt geretourneerd voor een geslaagde reactie, die een lijst met gedetailleerde gebruiks gegevens en-kosten bevat voor een bepaalde facturerings periode en factuur-ID voor de afdeling.

In het volgende voor beeld ziet u de uitvoer van de REST API `1234`voor Enter prise-inschrijving.

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

Dit voor beeld is afgekort; Zie [gebruiks gegevens ophalen voor een inschrijvings account](/rest/api/consumption/usagedetails/list#enrollmentaccountusagedetailslist) voor een volledige beschrijving van elk antwoord veld en fout afhandeling.

## <a name="next-steps"></a>Volgende stappen 
- [Overzicht van ENTER prise Reporting](https://docs.microsoft.com/azure/billing/billing-enterprise-api) bekijken
- [Facturerings rest API voor ondernemingen](https://docs.microsoft.com/rest/api/billing/) onderzoeken   
- [Aan de slag met Azure REST API](https://docs.microsoft.com/rest/api/azure/)   
