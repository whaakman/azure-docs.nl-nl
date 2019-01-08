---
title: Algemene parameters en headers
description: De parameters en headers die gemeenschappelijk zijn voor alle bewerkingen die u kunt doen met betrekking tot Key Vault-resources.
services: key-vault
documentationcenter: ''
author: bryanla
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: a715d13ca9-d6e8-4e54-ac5e-0ed9400fb15b15d13ca9-d6e8-4e54-ac5e-0ed9400fb15b
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: bryanla
ms.openlocfilehash: de9243a0a95c14a048be124976b07853a48a9a08
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54075223"
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

   {  
     "error": {  
     "code": "BadRequest"  
     "message": "De sku van sleutelkluis is ongeldig."  
     }  
   }  

|De naam van element | Type | Description |
|---|---|---|
| Code | string | Het soort fout dat is opgetreden.|
| message | string | Een beschrijving van wat de fout heeft veroorzaakt. |



## <a name="see-also"></a>Zie ook
 [Azure Key Vault REST API-verwijzing](/rest/api/keyvault/)
