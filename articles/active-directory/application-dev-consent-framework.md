---
title: De werking van de toepassing toestemming | Microsoft Docs
description: Meer informatie over de werking van het framework van Azure AD toestemming om te zien hoe u kunt dit gebruiken wanneer het ontwikkelen van toepassingen in Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 91378f4816db773aebd038bd9f176596a4f2c316
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366924"
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
