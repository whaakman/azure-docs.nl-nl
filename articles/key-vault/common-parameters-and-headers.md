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
ms.openlocfilehash: d635c7bdc6602c662ea6b91aad7e3f7a5e726547
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
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
