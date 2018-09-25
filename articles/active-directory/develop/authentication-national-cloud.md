---
title: Verificatie met behulp van Azure AD in nationale clouds
description: Meer informatie over app-registratie en verificatie-eindpunten voor nationale clouds.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: negoe
ms.reviewer: negoe,andret,saeeda
ms.custom: aaddev
ms.openlocfilehash: 866a86178d66b7b4af069d684e4eb56c12db47ca
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46981998"
---
# <a name="national-clouds"></a>Nationale Clouds

Nationale clouds (ook wel soevereine clouds) zijn fysiek geïsoleerd exemplaren van Azure. Deze regio's van Azure zijn ontworpen om ervoor te zorgen dat gegevens gegevenslocatie, soevereiniteit, en vereisten binnen geografische grenzen worden herkend.

Globale cloud, met inbegrip van Azure Active Directory is geïmplementeerd in de volgende nationale clouds:  

- Azure van de Amerikaanse overheid
- Azure Duitsland
- Azure China 21Vianet

## <a name="app-registration-endpoints"></a>Eindpunten voor App-registratie

De volgende tabel bevat de basis-URL's voor de Azure Active Directory (Azure AD)-eindpunten gebruikt voor het registreren van een toepassing voor elke nationale cloud.

| Nationale cloud | Azure AD portal-eindpunt
| --- | --- |
| Azure AD voor de Amerikaanse overheid |https://portal.azure.us
|Azure AD-Duitsland |https://portal.microsoftazure.de
|Azure AD-China, uitgevoerd door 21Vianet |https://portal.azure.cn
|Azure AD (global service)|https://portal.azure.com

## <a name="azure-ad-authentication-endpoints"></a>Azure AD-verificatie-eindpunten

De volgende tabel bevat de basis-URL's voor de Azure Active Directory (Azure AD)-eindpunten voor het verkrijgen van tokens voor het aanroepen van Microsoft Graph voor elke nationale cloud.

| Nationale cloud | Azure AD-verificatie-eindpunt
| --- | --- |
| Azure AD voor de Amerikaanse overheid |`https://login.microsoftonline.us`
|Azure AD-Duitsland| `https://login.microsoftonline.de`
|Azure AD-China, uitgevoerd door 21Vianet | `https://login.chinacloudapi.cn`
|Azure AD (global service)|`https://login.microsoftonline.com`

Aanvragen voor de Azure AD-eindpunten voor de autorisatie of token kunnen worden samengesteld met behulp van de juiste regiospecifieke basis-URL. Bijvoorbeeld, in het geval van Duitsland:

- Algemene autorisatie-eindpunt is `https://login.microsoftonline.de/common/oauth2/authorize`
- Algemene tokeneindpunt is `https://login.microsoftonline.de/common/oauth2/token` 

Voor toepassingen met één tenant, vervangen door de algemene in de bovenstaande URL's met uw tenant-id of naam, bijvoorbeeld `https://login.microsoftonline.de/contoso.com`

>[!NOTE]
> De [Azure AD v2.0-autorisatie]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) en token-eindpunten zijn alleen beschikbaar voor de algemene service. Het is nog niet ondersteund voor nationale cloudimplementaties.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- Meer informatie over [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- Meer informatie over [Azure Duitsland](https://docs.microsoft.com/azure/germany/)
- Meer informatie over de [basisbeginselen van Azure AD-verificatie](authentication-scenarios.md)
- Meer informatie over [Microsoft Graph-implementatie in nationale cloud](https://developer.microsoft.com/graph/docs/concepts/deployments)