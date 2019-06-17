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
ms.openlocfilehash: fd37366697a9c1f5019d2864e6d81a4dcd02e3a2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66235494"
---
# <a name="national-clouds"></a>Nationale clouds

Nationale clouds zijn fysiek geïsoleerd exemplaren van Azure. Deze regio's van Azure zijn ontworpen om ervoor te zorgen dat gegevens gegevenslocatie, soevereiniteit, en vereisten binnen geografische grenzen worden herkend.

Azure Active Directory (Azure AD) is geïmplementeerd met inbegrip van de globale cloud in de volgende nationale clouds:  

- Azure Government
- Azure Duitsland
- Azure China 21Vianet

Nationale clouds uniek zijn en een afzonderlijke omgeving van Azure wereldwijd. Het is belangrijk om te worden op de hoogte van belangrijke verschillen tijdens het ontwikkelen van uw toepassing voor deze omgevingen. Verschillen zijn toepassingen registreren, het verkrijgen van tokens en het configureren van eindpunten.

## <a name="app-registration-endpoints"></a>Eindpunten voor App-registratie

Er is een afzonderlijke Azure-portal voor elk van de nationale clouds. Voor het integreren van toepassingen met de Microsoft identity-platform in een nationale cloud, bent u verplicht afzonderlijk uw toepassing registreren in elke Azure-portal die specifiek is voor de omgeving.

De volgende tabel bevat de basis-URL's voor de Azure AD-eindpunten gebruikt voor het registreren van een toepassing voor elke nationale cloud.

| Nationale cloud | Azure AD portal-eindpunt |
|----------------|--------------------------|
| Azure AD voor de Amerikaanse overheid | `https://portal.azure.us` |
| Azure AD Germany | `https://portal.microsoftazure.de` |
| Azure AD-China, uitgevoerd door 21Vianet | `https://portal.azure.cn` |
| Azure AD (global service) |`https://portal.azure.com` |

## <a name="azure-ad-authentication-endpoints"></a>Azure AD-verificatie-eindpunten

De nationale clouds verificatie van gebruikers in elke omgeving afzonderlijk en afzonderlijke verificatie-eindpunten hebben.

De volgende tabel bevat de basis-URL's voor de Azure AD-eindpunten voor het verkrijgen van tokens voor elke nationale cloud.

| Nationale cloud | Azure AD-verificatie-eindpunt |
|----------------|-------------------------|
| Azure AD voor de Amerikaanse overheid | `https://login.microsoftonline.us` |
| Azure AD Germany| `https://login.microsoftonline.de` |
| Azure AD-China, uitgevoerd door 21Vianet | `https://login.chinacloudapi.cn` |
| Azure AD (global service)| `https://login.microsoftonline.com` |

Aanvragen voor de autorisatie van Azure AD of token-eindpunten kunt u vormen met behulp van de juiste regiospecifieke basis-URL. Bijvoorbeeld: voor Azure Duitsland:

  - Algemene autorisatie-eindpunt is `https://login.microsoftonline.de/common/oauth2/authorize`.
  - Algemene tokeneindpunt is `https://login.microsoftonline.de/common/oauth2/token`.

Vervang 'algemene' in de vorige URL's met de naam van uw tenant-ID of voor één tenant-toepassingen. Een voorbeeld is `https://login.microsoftonline.de/contoso.com`.

> [!NOTE]
> De [Azure AD v2.0-autorisatie]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) en token-eindpunten zijn alleen beschikbaar voor de globale-service. Ze worden niet ondersteund voor nationale cloudimplementaties.

## <a name="microsoft-graph-api"></a>Microsoft Graph API

Voor meer informatie over het aanroepen van de Microsoft Graph-API's in een nationale cloudomgeving, gaat u naar [Microsoft Graph in nationale cloudimplementaties](https://developer.microsoft.com/graph/docs/concepts/deployments).

> [!IMPORTANT]
> Bepaalde services en functies die zich in specifieke gebieden van de algemene service mogelijk niet beschikbaar in alle van de nationale clouds. Als u wilt weten welke services beschikbaar zijn, gaat u naar [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast).

Voor meer informatie over het bouwen van een toepassing met behulp van de Microsoft identity-platform, volgt u de [Microsoft Authentication Library (MSAL) zelfstudie](msal-national-cloud.md). Deze app wordt specifiek, meld u aan een gebruiker en een toegangstoken voor het aanroepen van de Microsoft Graph API.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over:

- [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Duitsland](https://docs.microsoft.com/azure/germany/)
- [Basisbeginselen van Azure AD-verificatie](authentication-scenarios.md)
