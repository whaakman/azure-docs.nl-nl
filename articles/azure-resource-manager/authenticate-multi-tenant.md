---
title: Verifiëren via tenants-Azure Resource Manager
description: Hierin wordt beschreven hoe Azure Resource Manager verificatie aanvragen afhandelt tussen tenants.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: tomfitz
ms.openlocfilehash: 625a17156eaf199af0d51151c6fd37769b8f7b4a
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848755"
---
# <a name="authenticate-requests-across-tenants"></a>Aanvragen verifiëren via tenants

Wanneer u een multi tenant toepassing maakt, moet u mogelijk verificatie aanvragen afhandelen voor bronnen die zich in verschillende tenants bevinden. Een veelvoorkomend scenario is wanneer een virtuele machine in een Tenant moet worden toegevoegd aan een virtueel netwerk in een andere Tenant. Azure Resource Manager biedt een header waarde voor het opslaan van de hulp tokens voor het verifiëren van de aanvragen voor verschillende tenants.

## <a name="header-values-for-authentication"></a>Header waarden voor verificatie

De aanvraag heeft de volgende waarden voor de verificatie-header:

| Headernaam | Description | Voorbeeldwaarde |
| ----------- | ----------- | ------------ |
| Authorization | Primair token | &lt;Primaire token van Bearer&gt; |
| x-MS-autorisatie-hulp | Hulp tokens | &gt; &lt;&gt;Bearer-token1, EncryptedBearer hulp token2, Bearer &lt;-token3 &lt;&gt; |

De hulp header kan Maxi maal drie hulp tokens bevatten. 

In de code van uw app met meerdere tenants haalt u het verificatie token voor andere tenants op en slaat u deze op in de hulp headers. Alle tokens moeten afkomstig zijn uit dezelfde gebruiker of toepassing. De gebruiker of toepassing moet als gast aan de andere tenants worden uitgenodigd.

## <a name="processing-the-request"></a>De aanvraag wordt verwerkt

Wanneer uw app een aanvraag naar Resource Manager verzendt, wordt de aanvraag uitgevoerd onder de identiteit van het primaire token. Het primaire token moet geldig zijn en niet verlopen. Dit token moet afkomstig zijn van een Tenant die het abonnement kan beheren.

Wanneer de aanvraag verwijst naar een bron van een andere Tenant, controleert Resource Manager de hulp tokens om te bepalen of de aanvraag kan worden verwerkt. Alle hulp tokens in de header moeten geldig en niet-verlopen zijn. Als een token is verlopen, retourneert Resource Manager een 401-respons code. Het antwoord bevat de client-ID en Tenant-ID van het token dat niet geldig is. Als de hulp header een geldig token voor de Tenant bevat, wordt de aanvraag voor cross tenants verwerkt.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het verzenden van verificatie aanvragen met de Azure Resource Manager Api's de [Resource Manager-verificatie-API gebruiken voor toegang tot abonnementen](resource-manager-api-authentication.md).
* Zie [Azure Active Directory toegangs tokens](/azure/active-directory/develop/access-tokens)voor meer informatie over tokens.
