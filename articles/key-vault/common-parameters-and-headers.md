---
title: Algemene parameters en headers
description: De parameters en headers die gemeenschappelijk zijn voor alle bewerkingen die u kunt doen met betrekking tot Key Vault-resources.
services: key-vault
author: msmbaldwin
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 01ac02ca0e449dcac8fcd05450566fd8138acba3
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "64696693"
---
# <a name="common-parameters-and-headers"></a>Algemene parameters en headers

De volgende informatie geldt voor alle bewerkingen die u kunt doen met betrekking tot Key Vault-resources:

- Vervang `{api-version}` met de api-versie in de URI.
- Vervang `{subscription-id}` met uw abonnements-id in de URI
- Vervang `{resource-group-name}` met de resourcegroep. Zie voor meer informatie, met behulp van resourcegroepen voor het beheren van uw Azure-resources.
- Vervang `{vault-name}` met de naam van uw key vault in de URI.
- De header Content-Type ingesteld op application/json.
- De autorisatie-header ingesteld op een JSON Web Token die u van Azure Active Directory (AAD aanschaft). Zie voor meer informatie, [verificatie van Azure Resource Manager](authentication-requests-and-responses.md) aanvragen.

## <a name="common-error-response"></a>Algemene fout antwoord
De service maakt gebruik van HTTP-statuscodes om aan te geven van slagen of mislukken. Fouten bevatten bovendien een antwoord in de volgende indeling:

```
   {  
     "error": {  
     "code": "BadRequest",  
     "message": "The key vault sku is invalid."  
     }  
   }  
```

|De naam van element | Type | Description |
|---|---|---|
| code | string | Het soort fout dat is opgetreden.|
| message | string | Een beschrijving van wat de fout heeft veroorzaakt. |



## <a name="see-also"></a>Zie ook
 [Azure Key Vault REST API-verwijzing](/rest/api/keyvault/)
