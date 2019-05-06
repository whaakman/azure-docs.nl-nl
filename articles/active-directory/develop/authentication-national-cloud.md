---
title: Verificatie met behulp van Azure Active Directory in nationale clouds
description: Meer informatie over app-registratie en verificatie-eindpunten voor nationale clouds.
services: active-directory
documentationcenter: ''
author: negoe
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: negoe
ms.reviewer: negoe,CelesteDG
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0d4586df23548854f4acbfefd32081a36906097
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067892"
---
# <a name="national-clouds"></a>Nationale clouds

Nationale clouds zijn fysiek geïsoleerd exemplaren van Azure. Deze regio's van Azure zijn ontworpen om ervoor te zorgen dat gegevens gegevenslocatie, soevereiniteit, en vereisten binnen geografische grenzen worden herkend.

Globale cloud, met inbegrip van Azure Active Directory is geïmplementeerd in de volgende nationale clouds:  

- Azure van de Amerikaanse overheid
- Azure Duitsland
- Azure China 21Vianet

Nationale clouds zijn uniek en andere omgeving dan Azure wereldwijd. Daarom is het belangrijk dat u rekening houden met enkele belangrijke verschillen tijdens het ontwikkelen van uw toepassing voor deze omgevingen, zoals het registreren van toepassingen, het verkrijgen van tokens en het configureren van eindpunten.

## <a name="app-registration-endpoints"></a>Eindpunten voor App-registratie

Er is een afzonderlijke Azure-portal voor elk van de nationale clouds. U moet voor de integratie van toepassingen met de Microsoft Identity-Platform in een nationale cloud, afzonderlijk uw toepassing registreren in elk van de Azure portal die specifiek zijn voor de omgeving.

De volgende tabel bevat de basis-URL's voor de Azure Active Directory (Azure AD)-eindpunten gebruikt voor het registreren van een toepassing voor elke nationale cloud.

| Nationale cloud | Azure AD portal-eindpunt |
|----------------|--------------------------|
| Azure AD voor de Amerikaanse overheid | `https://portal.azure.us` |
| Azure AD Germany | `https://portal.microsoftazure.de` |
| Azure AD-China, uitgevoerd door 21Vianet | `https://portal.azure.cn` |
| Azure AD (global service) |`https://portal.azure.com` |

## <a name="azure-ad-authentication-endpoints"></a>Azure AD-verificatie-eindpunten

De nationale clouds verificatie van gebruikers in elke omgeving afzonderlijk en afzonderlijke verificatie-eindpunten hebben.

De volgende tabel bevat de basis-URL's voor de Azure Active Directory (Azure AD)-eindpunten voor het verkrijgen van tokens voor elke nationale cloud.

| Nationale cloud | Azure AD-verificatie-eindpunt |
|----------------|-------------------------|
| Azure AD voor de Amerikaanse overheid | `https://login.microsoftonline.us` |
| Azure AD Germany| `https://login.microsoftonline.de` |
| Azure AD-China, uitgevoerd door 21Vianet | `https://login.chinacloudapi.cn` |
| Azure AD (global service)| `https://login.microsoftonline.com` |

- Aanvragen voor de Azure AD-eindpunten voor de autorisatie of token kunnen worden samengesteld met behulp van de juiste regiospecifieke basis-URL. Bijvoorbeeld: voor Azure Duitsland:

  - Algemene autorisatie-eindpunt is `https://login.microsoftonline.de/common/oauth2/authorize`.
  - Algemene tokeneindpunt is `https://login.microsoftonline.de/common/oauth2/token`.

- Voor toepassingen met één tenant, vervangen door de algemene in de vorige URL's met uw tenant-ID of naam, bijvoorbeeld `https://login.microsoftonline.de/contoso.com`.

> [!NOTE]
> De [Azure AD v2.0-autorisatie]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) en token-eindpunten zijn alleen beschikbaar voor de algemene service. Het is nog niet ondersteund voor nationale cloudimplementaties.

## <a name="microsoft-graph-api"></a>Microsoft Graph API

Voor meer informatie over het aanroepen van de Microsoft Graph-API's in nationale cloudomgeving gaat u naar [Microsoft Graph in nationale cloud](https://developer.microsoft.com/graph/docs/concepts/deployments).

> [!IMPORTANT]
> Bepaalde services en functies die zich in specifieke gebieden van de algemene service mogelijk niet beschikbaar in alle van de nationale clouds. Om erachter te komen welke services beschikbaar zijn. Ga naar [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast).

Volg deze [Microsoft Authentication Library (MSAL) zelfstudie](msal-national-cloud.md) voor meer informatie over het bouwen van een toepassing met behulp van Microsoft identity-platform. Deze app wordt Meld u aan een gebruiker ophalen specifiek, een toegangstoken voor het aanroepen van de Microsoft Graph API.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over:

- [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Duitsland](https://docs.microsoft.com/azure/germany/)
- [Basisbeginselen van Azure AD-verificatie](authentication-scenarios.md)
