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
ms.openlocfilehash: 5d8d526e67a69959687fb79ac6406d8313a417a8
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44724013"
---
# <a name="how-application-consent-works"></a>Hoe werkt in toepassing toestemming geven

In dit artikel is om te helpen u meer informatie over de werking van het toestemmingsframework Azure AD, zodat u toepassingen effectiever kunt ontwikkelen.

## <a name="recommended-documents"></a>Aanbevolen documenten

- Algemene inzicht te krijgen [hoe toestemming van de eigenaar van een resource om te bepalen van de toegang tot resources van een toepassing kunt](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#consent).
- Een stapsgewijze overzicht van [hoe de Azure AD-toestemmingsframework toestemming implementeert](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework).
- Lees voor meer diepgang [hoe het toestemmingsframework kunt gebruiken in een toepassing met meerdere tenants](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) implementeren 'gebruiker' en 'admin' toestemming, ondersteunt meer geavanceerde toepassingen met meerdere lagen.
- Lees voor meer diepgang [hoe toestemming op het niveau van de OAuth 2.0-protocol wordt ondersteund tijdens de toewijzingsstroom voor autorisatie.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code)

## <a name="next-steps"></a>Volgende stappen
[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
