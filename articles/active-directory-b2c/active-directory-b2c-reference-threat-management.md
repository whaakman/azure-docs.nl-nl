---
title: Bedreigingen van beheer in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over detectie en risicobeperking technieken voor denial-of-service-aanvallen en aanvallen wachtwoord in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/27/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 1801fe9695aa15850d600300b957df2c7d7cd9ef
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42058126"
---
# <a name="azure-active-directory-b2c-threat-management"></a>Azure Active Directory B2C: Beveiligingsbeheer

Beveiligingsbeheer omvat het plannen voor bescherming tegen aanvallen op uw systeem en netwerken. Denial-of-service-aanvallen kunnen resources niet beschikbaar maken voor beoogde gebruikers. Wachtwoord aanvallen leiden tot niet-geautoriseerde toegang tot bronnen. Azure Active Directory B2C (Azure AD B2C) beschikt over ingebouwde functies waarmee u uw gegevens beschermen tegen deze bedreigingen op verschillende manieren kunnen.

## <a name="denial-of-service-attacks"></a>Denial-of-service-aanvallen

Azure AD B2C maakt gebruik van detectie en risicobeperking technieken zoals SYN cookies en limieten voor snelheid en verbinding om onderliggende resources tegen denial-of-service-aanvallen te beschermen.

## <a name="password-attacks"></a>Wachtwoord aanvallen

Azure AD B2C heeft ook risicobeperking technieken in plaats van wachtwoord-aanvallen. Risicobeperking omvat wachtwoord brute-force-aanvallen en woordenboekaanvallen wachtwoord. Wachtwoorden die zijn ingesteld door gebruikers moeten redelijk complex zijn. Azure AD B2C analyseert met behulp van verschillende signalen, de integriteit van aanvragen. Azure AD B2C is ontworpen om u te onderscheiden op intelligente wijze beoogde gebruikers tegen hackers en botnets. Azure AD B2C biedt een geavanceerde strategie om account te vergrendelen op basis van de wachtwoorden die zijn ingevoerd, in de kans op een aanval.

Voor meer informatie gaat u naar de [Microsoft Trust Center](https://www.microsoft.com/trustcenter/default.aspx).
