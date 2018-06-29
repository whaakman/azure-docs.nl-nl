---
title: Bekijk Azure enterprise-inschrijving facturering gegevens met REST-API | Microsoft Docs
description: Informatie over het gebruik van Azure REST API's voor enterprise-inschrijving facturering informatie.
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
ms.openlocfilehash: 046b2e31aaefa5916a42b3652f9e6a8fdceff367
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063885"
---
# <a name="review-enterprise-enrollment-billing-using-rest-apis"></a>Enterprise-inschrijving facturering met REST API's bekijken

Azure rapportage-API's helpen u te controleren en beheren van uw Azure kosten.

U leert hier, voor het ophalen van de huidige factuur die is gekoppeld aan een enterprise-inschrijving-account.

Voor het ophalen van de huidige factuur:
``` http
GET https://consumption.azure.com/v2/enrollments/{enrollmentID}/usagedetails
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>De aanvraag voor het samenstellen  

De `{enrollmentID}` parameter is vereist en de registratie-ID voor de Enterprise-Account (EA) moet bevatten.

De volgende headers zijn vereist: 

|Aanvraag-header|Beschrijving|  
|--------------------|-----------------|  
|*Content-Type:*|Vereist. Ingesteld op `application/json`.|  
|*Autorisatie:*|Vereist. Ingesteld op een geldige `Bearer` [API-sleutel](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based). |  

In dit voorbeeld toont het synchrone aanroepen die als resultaat details voor de huidige factureringscyclus geeft. Uit prestatieoverwegingen synchrone aanroepen informatie geretourneerd voor de afgelopen maand.  U kunt ook aanroepen de [API asynchroon](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) gegevens te retourneren voor 36 maanden.


## <a name="response"></a>Antwoord  

Statuscode 200 wordt (OK) geretourneerd voor een geslaagde reactie die een lijst met gedetailleerde kosten voor uw account bevat.

``` json
{
    "id": "${id}",
    "data": [
        {
            "cost": ${cost}, 
            "departmentId": ${departmentID},
            "subscriptionGuid" : ${subscriptionGuid} 
            "date": "${date}",
            "tags": "${tags}",
            "resourceGroup": "${resourceGroup}"
        } // ...
    ],
    "nextLink": "${nextLinkURL}"
}
```  

Elk item in **gegevens** vertegenwoordigt kosten met zich mee:

|Eigenschap Response|Beschrijving|
|----------------|----------|
|**Kosten** | Het bedrag in rekening gebracht, in een geschikt is voor de locatie van het datacenter valuta. |
|**subscriptionGuid** | Globaal unieke ID voor het abonnement. | 
|**DepartmentID gemeenschappelijk hebben** | ID voor de afdeling, indien van toepassing. |
|**Datum** | Datum waarop die de kosten is gefactureerd. |
|**tags** | JSON-tekenreeks met labels van het abonnement. |
|**resourceGroup**|Naam van de resourcegroep met het object dat de kosten ontstaan. |
|**nextLink**| Als de waarde, een URL voor de volgende 'pagina"gegevens bevat. Leeg wanneer de pagina het laatste is. |  
||
  
Afdeling-id's, resourcegroepen, labels en verwante velden worden gedefinieerd door de beheerder EA.  

In dit voorbeeld is afgekort; Zie [gebruik Details ophalen](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) voor een volledige beschrijving van elk antwoordveld. 

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
