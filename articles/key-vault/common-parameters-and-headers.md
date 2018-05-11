---
title: Algemene parameters en -koppen
description: De parameters en headers die gemeenschappelijk zijn voor alle bewerkingen die u kunt doen met betrekking tot de Sleutelkluis-resources.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: a715d13ca9-d6e8-4e54-ac5e-0ed9400fb15b15d13ca9-d6e8-4e54-ac5e-0ed9400fb15b
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: ead1ac550c9b7c489edefd35d5672a9955e78255
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="common-parameters-and-headers"></a>Algemene parameters en -koppen

De volgende informatie geldt voor alle bewerkingen die u kunt doen over de Sleutelkluis-resources:

- Vervang `{api-version}` met de api-versie in de URI.
- Vervang `{subscription-id}` met uw abonnements-id in de URI
- Vervang `{resource-group-name}` met de resourcegroep. Zie resourcegroepen gebruiken voor het beheren van uw Azure-resources voor meer informatie.
- Vervang `{vault-name}` met de naam van uw sleutelkluis in de URI.
- De header Content-Type ingesteld op application/json.
- Stel de autorisatie-header op een JSON Web Token die u uit Azure Active Directory (AAD verkrijgt). Zie voor meer informatie [Azure Resource Manager verifiëren](authentication-requests-and-responses.md) aanvragen.

## <a name="common-error-response"></a>Algemene fout antwoord
De service maakt gebruik van HTTP-statuscodes om aan te geven slagen of mislukken. Bovendien bevatten fouten antwoord in de volgende indeling:

   {  
     "error": {  
     'code': 'BadRequest'  
     "message": "de sku van de sleutelkluis is ongeldig."  
     }  
   }  

|Elementnaam | Type | Beschrijving |
|---|---|---|
| code | tekenreeks | Het type van de fout is opgetreden.|
| message | tekenreeks | Een beschrijving van wat de fout heeft veroorzaakt. |



## <a name="see-also"></a>Zie ook
 [Naslaginformatie over REST API voor Azure Sleutelkluis](/rest/api/keyvault/)
