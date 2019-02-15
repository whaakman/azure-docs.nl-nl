---
title: Overzicht van Azure Active Directory (Azure AD) Identity Protection-beveiliging | Microsoft Docs
description: Meer informatie over hoe de beveiligingsoverzicht biedt u een beter inzicht in de beveiligingsstatus van uw organisatie.
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: markvi
ms.reviewer: raluthra
ms.collection: M365-identity-device-management
ms.openlocfilehash: dca184e5b94d69845bfa6e2d83753f53554cadeb
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56266744"
---
# <a name="azure-active-directory-identity-protection---security-overview"></a>Azure Active Directory Identity Protection - beveiligingsoverzicht

De [beveiligingsoverzicht](https://aka.ms/IdentityProtectionRefresh) biedt u een beter inzicht in de beveiligingsstatus van uw organisatie. Het helpt potentiële aanvallen identificeren en te begrijpen van de effectiviteit van uw beleid.

De beveiligingsoverzicht is breed onderverdeeld in twee secties:

- Trends, bieden een tijdlijn van de risico's in uw organisatie aan de linkerkant.

- Tegels aan de rechterkant, markeert u de belangrijkste lopende problemen in uw organisatie en voorstellen hoe snel actie te ondernemen.


![Beveiligingsoverzicht](./media/security-overview/01.png)
  
## <a name="trends"></a>Trends

### <a name="new-risky-users-detected"></a>Er zijn nieuwe riskante gebruikers gedetecteerd

In deze grafiek toont het aantal nieuwe riskante gebruikers die zijn gedetecteerd via de geselecteerde tijdsperiode. U kunt de weergave van deze grafiek door gebruiker risiconiveau (laag, Gemiddeld, hoog) filteren. Beweeg de muisaanwijzer over de stappen van de UTC-datum om het aantal riskante gebruikers gevonden voor die dag te zien. Klik in deze grafiek gaat u naar het rapport 'Riskante gebruikers'. Als u wilt herstellen van gebruikers die risico lopen, kunt u overwegen hun wachtwoord wijzigen.

### <a name="new-risky-sign-ins-detected"></a>Nieuwe riskante aanmeldingen gedetecteerd

In deze grafiek toont het aantal riskante aanmeldingen gedetecteerd via de geselecteerde tijdsperiode. U kunt de weergave van deze grafiek van het type van het aanmeldingsrisico (realtime of cumulatieve) en het niveau van aanmeldingsrisico (laag, Gemiddeld, hoog) filteren. Niet-beveiligde aanmeldingen zijn geslaagd realtime risico-aanmeldingen die eerder niet MFA gebruiken waren. (Opmerking: Sign-ins die riskant zijn vanwege offline detecties kan niet worden beveiligd realtime door beleid voor aanmeldingsrisico). Beweeg de muisaanwijzer over de stappen van de UTC-datum om het aantal aanmeldingen die zijn gedetecteerd op risico's voor die dag te zien. Klik in deze grafiek gaat u naar het rapport 'Riskante aanmeldingen'.

## <a name="tiles"></a>Tegels
 
### <a name="high-risk-users"></a>Gebruikers met een hoog risico

De tegel 'gebruikers met een hoog risico' ziet u de meest recente telling van gebruikers met hoge kans van inbreuk op de identiteit. Dit moeten een topprioriteit voor onderzoek. Klik op de tegel 'gebruikers met een hoog risico' wordt u doorgestuurd naar een gefilterde weergave van het rapport 'Riskante gebruikers' is alleen gebruikers met een hoog risico weergegeven. Met dit rapport kunt u meer informatie en herstellen van deze gebruikers met een wachtwoord opnieuw instellen.

![Beveiligingsoverzicht](./media/security-overview/02.png)


### <a name="medium-risk-users"></a>Gebruikers met gemiddeld risico
De tegel 'gemiddeld risico gebruikers' ziet de meest recente telling van gebruikers met gemiddeld kans van inbreuk op de identiteit. Klik op 'gemiddeld risico gebruikers' tegel wordt u doorgestuurd naar een gefilterde weergave van het rapport 'Riskante gebruikers' is alleen gebruikers met een risiconiveau van gemiddeld weergegeven. Met dit rapport kunt u verder onderzoeken en herstellen van deze gebruikers.

### <a name="unprotected-risky-sign-ins"></a>Niet-beveiligde riskante aanmeldingen

De tegel 'Niet-beveiligde riskante aanmeldingen' zien van de afgelopen week aantal geslaagde, realtime riskante aanmeldingen die niet zijn geblokkeerd en ook geen sprake van een beleid voor voorwaardelijke toegang, Identity Protection-beleid voor gebruikersrisico's of MFA per gebruiker MFA. Dit zijn mogelijk verdachte aanmeldingen die geslaagd zijn en geen MFA gebruiken. Als u wilt beveiligen die aanmeldingen in de toekomst, een beleid voor aanmeldingsrisico van toepassing. Klik op de tegel 'Niet-beveiligde riskante aanmeldingen' wordt omgeleid naar de blade aanmeldingsrisico beleid configuratie waarin u het beleid voor aanmeldingsrisico om MFA te vereisen op een aanmelding met een opgegeven risiconiveau kunt configureren.


### <a name="legacy-authentication"></a>Verouderde verificatie

De tegel 'Legacy authentication' ziet u de afgelopen week telling van verouderde verificaties in uw organisatie. Verouderde verificatieprotocollen bieden geen ondersteuning voor moderne beveiligingsmethoden, zoals een MFA. Om te voorkomen dat een legacy-verificatie, kunt u een beleid voor voorwaardelijke toegang toepassen. Klik op de tegel 'Legacy verificatie' wordt u doorgestuurd naar de 'identiteit Secure Score'.


### <a name="identity-secure-score"></a>Identiteit beveiligde Score

De identiteit Secure Score meet en vergelijking van uw beveiligingspostuur branche patronen. Als u klikt op de tegel 'Identiteit Secure Score (Preview)', wordt deze omgeleid naar de blade 'Identiteit Secure Score (Preview)' is waar u kunt meer informatie over uw beveiligingspostuur verbeteren.


## <a name="next-steps"></a>Volgende stappen

- [Channel 9: Azure AD en Identity weergeven: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

- [Azure Active Directory Identity Protection inschakelen](enable.md)

