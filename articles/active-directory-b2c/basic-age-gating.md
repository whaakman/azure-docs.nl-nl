---
title: Met leeftijd beperken in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het identificeren van minderjarigen met behulp van uw toepassing.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 9a9a86d445deaea4872615f443ad53f76638a758
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47056519"
---
#<a name="using-age-gating-in-azure-ad-b2c"></a>Met behulp van leeftijdsbeperking in Azure AD B2C

>[!IMPORTANT]
>Deze functie is in de beperkte Preview-versie.  Raadpleeg onze [serviceblog](https://blogs.msdn.microsoft.com/azureadb2c/) voor meer informatie over het gebruik van dit wordt beschikbaar, of neem contact op met AADB2CPreview@microsoft.com.  Gebruik dit niet voor productie-mappen, met behulp van deze nieuwe functies kan leiden tot verlies van gegevens en mogelijk onverwachte wijzigingen in werking totdat we gaan naar algemene beschikbaarheid.  
>

##<a name="age-gating"></a>Leeftijdsbeperking
Leeftijd beperken, kunt u Azure AD B2C gebruiken voor het identificeren van minderjarigen in uw toepassing.  U kunt voorkomen dat de gebruiker zich in de toepassing of kunnen ze terugkeren naar de toepassing met extra claims die de gebruikersgroep van de leeftijd en hun status ouderlijke toestemming identificeren.  

>[!NOTE]
>Er is ouderlijke toestemming wordt bijgehouden in een gebruikerskenmerk met de naam `consentProvidedForMinor`.  U kunt deze eigenschap via de Graph API bijwerken en dit veld wordt gebruikt bij het bijwerken van `legalAgeGroupClassification`.
>

##<a name="setting-up-your-directory-for-age-gating"></a>Instellen van uw directory voor leeftijdsbeperking
Als u wilt gebruiken voor leeftijdsbeperking in de gebruikersstroom van een, moet u uw directory om aanvullende eigenschappen in te stellen. Met deze bewerking kan worden gedaan via `Properties` in het menu (dit is alleen beschikbaar als u deel uit van de private preview).  
1. Klik in de Azure AD B2C-extensie op de **eigenschappen** voor uw tenant in het menu aan de linkerkant.
2. Onder de **leeftijdsbeperking** sectie, klikt u op de **configureren** knop.
3. Wacht totdat de bewerking is voltooid en uw directory worden ingesteld voor leeftijdsbeperking.

##<a name="enabling-age-gating-in-your-user-flow"></a>Leeftijdsbeperking in uw beleid inschakelen
Zodra uw directory leeftijdsbeperking gebruiken is ingesteld, kunt u deze functie vervolgens gebruiken in de gebruikersstromen van de preview-versie.  Deze functie moet de wijzigingen die niet compatibel met bestaande typen gebruikersstromen.  U schakelt leeftijdsbeperking met de volgende stappen uit:
1. Een stroom preview-gebruiker maken.
2. Nadat deze is gemaakt, gaat u naar **eigenschappen** in het menu.
3. In de **leeftijdsbeperking** sectie, drukt u op de wisselknop in de functie in te schakelen.
4. Vervolgens kunt u kiezen hoe u gebruikers wilt beheren die als minderjarigen identificeren.

##<a name="what-does-enabling-age-gating-do"></a>Wat doet leeftijdsbeperking inschakelen?
Zodra leeftijdsbeperking in uw beleid is ingeschakeld, wordt de gebruikerservaring wijzigingen.  Op aanmelden, wordt gebruikers nu worden gevraagd om hun geboortedatum en land waar u woont, samen met de kenmerken van de gebruiker geconfigureerd voor het beleid.  Op zich heeft aangemeld, wordt de gebruikers die nog niet eerder hebt opgegeven een geboortedatum en land waar u woont worden gevraagd om deze informatie de volgende keer dat ze zich aanmelden.  Van deze twee waarden, Azure AD B2C wordt bepalen of de gebruiker een minderjarige is en update de `ageGroup` veld de waarde kan zijn `null`, `Undefined`, `Minor`, `Adult`, en `NotAdult`.  De `ageGroup` en `consentProvidedForMinor` velden worden vervolgens gebruikt voor het berekenen van `legalAgeGroupClassification`. 

##<a name="age-gating-options"></a>Opties voor leeftijdsbeperking
U kunt minderjarigen blokkeren voor Azure AD B2C zonder toestemming van ouders of toestaat en de beslissingen over wat u moet doen met deze toepassing.  

###<a name="allowing-minors-without-parental-consent"></a>Minderjarigen zonder toestemming van ouders toestaan
Gebruikersstromen kunt u een aanmelding maximaal, meld u aan of beide, kunt u minderjarigen zonder toestemming in uw toepassing.  Minderjarigen zonder toestemming van ouders ze toegang kunnen krijgen aan te melden of zich aanmelden als normale en Azure AD B2C problemen met een ID-token met de `legalAgeGroupClassification` claim.  Met behulp van deze claim kunt u de ervaring die deze gebruikers hebben, zoals doorlopen een ervaring voor het verzamelen van ouderlijke toestemming (en bij te werken de `consentProvidedForMinor` veld).

###<a name="blocking-minors-without-parental-consent"></a>Minderjarigen zonder toestemming van ouders blokkeren
Gebruikersstromen kunt u een aanmelding maximaal, meld u aan of beide, kunt u minderjarigen zonder toestemming van de toepassing blokkeren.  Er zijn twee opties voor het verwerken van geblokkeerde gebruikers in Azure AD B2C:
* Verzenden van een JSON terug naar de toepassing: deze optie stuurt een antwoord terug naar de toepassing die een minderjarige is geblokkeerd.
* Een foutpagina - weergeven die de gebruiker een pagina die wordt gemeld dat ze geen toegang de toepassing tot worden weergegeven