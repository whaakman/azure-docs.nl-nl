---
title: De werking van de toepassing toestemming | Microsoft Docs
description: Meer informatie over de werking van het framework van Azure AD toestemming om te zien hoe u kunt dit gebruiken wanneer het ontwikkelen van toepassingen in Azure AD
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: d8a475bad5c232ceb7553674d97ad8abdb89ae5b
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52421671"
---
# <a name="how-application-consent-works"></a>Hoe werkt in toepassing toestemming geven

In dit artikel is om te helpen u meer informatie over de werking van het toestemmingsframework Azure AD, zodat u toepassingen effectiever kunt ontwikkelen.

## <a name="recommended-documents"></a>Aanbevolen documenten

- Algemene inzicht te krijgen [hoe toestemming van de eigenaar van een resource om te bepalen van de toegang tot resources van een toepassing kunt](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#consent).
- Een stapsgewijze overzicht van [hoe de Azure AD-toestemmingsframework toestemming implementeert](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework).
- Lees voor meer diepgang [hoe het toestemmingsframework kunt gebruiken in een toepassing met meerdere tenants](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) implementeren 'gebruiker' en 'admin' toestemming, ondersteunt meer geavanceerde toepassingen met meerdere lagen.
- Lees voor meer diepgang [hoe toestemming op het niveau van de OAuth 2.0-protocol wordt ondersteund tijdens de toewijzingsstroom voor autorisatie.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code)

## <a name="next-steps"></a>Volgende stappen
[AzureAD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
