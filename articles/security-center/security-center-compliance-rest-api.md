---
title: Bewaken van Azure Security Center-naleving met behulp van de REST-API | Microsoft Docs
description: Bekijk de resultaten van geautomatiseerde naleving scant met de REST-API van Azure Security Center.
services: security-center
documentationcenter: na
author: rloutlaw
manager: angerobe
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2018
ms.author: rloutlaw
ms.openlocfilehash: 651d7737258f1b1b17c8392a09882388ddf95635
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42820067"
---
# <a name="review-security-center-compliance-results-using-the-azure-rest-apis"></a>Bekijk de resultaten van Security Center naleving met behulp van de Azure REST API 's

In dit artikel leert u ophalen van gegevens van de naleving voor een lijst met de REST-API van Azure-abonnementen.

## <a name="review-compliance-for-each-subscription"></a>Beoordeling van naleving voor elk abonnement

Het onderstaande voorbeeld haalt u informatie over beveiliging beoordelingshulpprogramma voor een bepaalde nalevings- en -abonnement met de [ophalen om](/rest/api/securitycenter/compliances/get) bewerking.

```http
GET https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Security/compliances/{complianceName}?api-version=2017-08-01-preview
```

De `{complianceName}` -parameter is vereist en moet de naam van de geëvalueerde compatibiliteit voor bevatten `{subscription-id}`. Uitvoer bevat de resultaten van de evaluatie, zoals:

```json
{
...
  "properties": {
    "assessmentResult": [
      {
        "segmentType": "Compliant",
        "percentage": 77.777777777777786
      }
    ],
    "resourceCount": 18,
    "assessmentTimestampUtcDate": "2018-01-01T00:00:00Z"
  }
}
```

## <a name="review-compliance-for-multiple-subscriptions"></a>Naleving voor meerdere abonnementen te controleren

Als u gegevens voor meerdere abonnementen, Controleer de aanroep van hieronder een lijst met abonnementen met behulp van de eerste keer de [lijst met abonnementen](/rest/api/resources/subscriptions/list) bewerking. De bovenstaande invoke [ophalen om](/rest/api/securitycenter/compliances/get) voor elk van de geretourneerde abonnement aanleveren.

De API-aanroep is:

```http
GET https://management.azure.com/subscriptions?api-version=2016-06-01
```

Dit retourneert een reeks met waarden zoals de volgende. Gebruik de abonnements-id-waarden in de bovenstaande aanroep om te controleren van de informatie over de naleving voor alle abonnementen.

```json
"value": [
    {
      "id": "/subscriptions/{subscription-id}",
      "subscriptionId": "{subscription-id}",
      "displayName": "Demo subscription",
      ...
    }
```






