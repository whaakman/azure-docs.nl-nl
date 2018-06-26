---
title: Dreiging management in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over detectie en risicobeperking technieken voor denial of service-aanvallen en wachtwoord aanvallen in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 03/27/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 0a439a2bb485829b863d6aa666938ab58c90c61f
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750774"
---
# <a name="azure-active-directory-b2c-threat-management"></a>Azure Active Directory B2C: Threat management

Threat management omvat het plannen voor bescherming tegen aanvallen op uw systeem en netwerken. Denial of service-aanvallen kunnen maken resources niet beschikbaar voor beoogde gebruikers. Wachtwoord aanvallen leiden tot onbevoegde toegang tot bronnen. Azure Active Directory B2C (Azure AD B2C) beschikt over ingebouwde functies waarmee u uw gegevens beschermen tegen deze bedreigingen op verschillende manieren kunnen.

## <a name="denial-of-service-attacks"></a>Denial of service-aanvallen

Azure AD B2C maakt gebruik van detectie en risicobeperking technieken zoals SYN cookies en snelheid en verbinding beperkingen voor de onderliggende resources tegen denial of service-aanvallen te beveiligen.

## <a name="password-attacks"></a>Wachtwoord aanvallen

Azure AD B2C heeft ook risicobeperking technieken voor wachtwoord aanvallen. Risicobeperking omvat aanvallen met brute kracht wachtwoord en woordenboekaanvallen wachtwoord. Wachtwoorden die zijn ingesteld door gebruikers zijn te redelijkerwijs ingewikkeld vereist. Azure AD B2C analyseert met behulp van verschillende signalen, de integriteit van aanvragen. Azure AD B2C is ontworpen om te onderscheiden op intelligente wijze beoogde gebruikers tegen hackers en botnets. Azure AD B2C biedt een strategie voor een geavanceerde aan accounts van vergrendeling op basis van de wachtwoorden die zijn ingevoerd, in de kans op een aanval.

Voor meer informatie gaat u naar de [Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/default.aspx).
