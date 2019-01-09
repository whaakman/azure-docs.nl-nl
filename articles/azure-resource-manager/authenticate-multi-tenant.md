---
title: Voor tenants - Azure Resource Manager verifiëren
description: Hierin wordt beschreven hoe Azure Resource Manager omgaat met aanvragen voor authenticatie voor tenants.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: tomfitz
ms.openlocfilehash: 6554c05f40f580a6d7ae086e1d09834298f86621
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54108375"
---
# <a name="authenticate-requests-across-tenants"></a>Verifiëren van aanvragen voor tenants

Bij het maken van een toepassing met meerdere tenants, moet u mogelijk de afhandeling van verificatieaanvragen voor bronnen die zich in verschillende tenants. Een veelvoorkomend scenario is wanneer een virtuele machine in één tenant moet deelnemen aan een virtueel netwerk in een andere tenant. Azure Resource Manager biedt een headerwaarde voor het opslaan van hulptokens om te verifiëren van de aanvragen voor andere tenants.

## <a name="header-values-for-authentication"></a>Headerwaarden voor verificatie

De aanvraag heeft de volgende waarden van de verificatie-header:

| Headernaam | Description | Voorbeeldwaarde |
| ----------- | ----------- | ------------ |
| Autorisatie | Primaire token | Bearer &lt;primaire-token&gt; |
| x-ms-autorisatie-assistent | Hulptokens | Bearer &lt;assistent token1&gt;; EncryptedBearer &lt;assistent token2&gt;; Bearer &lt;assistent token3&gt; |

De aanvullende header kan maximaal drie hulptokens bevatten. 

In de code van uw app met meerdere tenants, de verificatie-token ophalen voor andere tenants en op te slaan in de aanvullende headers. Alle tokens moeten op dezelfde gebruiker of toepassing. De gebruiker of toepassing moet zijn uitgenodigd als Gast aan de andere tenants.

## <a name="processing-the-request"></a>Verwerken van de aanvraag

Wanneer uw app naar Resource Manager een aanvraag verzendt, wordt de aanvraag uitgevoerd onder de identiteit van het primaire token. Het primaire token moet geldig zijn en niet-verlopen. Dit token moet afkomstig zijn van een tenant die het abonnement kunt beheren.

Wanneer de aanvraag wordt verwezen naar een resource vanaf een andere tenant, controleert de Resource Manager de hulptokens om te bepalen als de aanvraag kan worden verwerkt. Alle hulptokens in de header moet geldig zijn en niet-verlopen. Als een token is verlopen, retourneert de Resource Manager een 401-respons-code. Het antwoord bevat de client-ID en tenant-ID van het token dat is niet geldig. Als de kop van de aanvullende een geldig token voor de tenant bevat, worden de cross tenantaanvraag wordt verwerkt.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het verzenden van aanvragen voor authenticatie met de Azure Resource Manager API's, [gebruik Resource Manager verificatie-API voor toegang tot abonnementen](resource-manager-api-authentication.md).
* Zie voor meer informatie over tokens [Azure Active Directory-toegangstokens](/azure/active-directory/develop/access-tokens).
