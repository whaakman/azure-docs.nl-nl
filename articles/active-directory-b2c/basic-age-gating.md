---
title: Met leeftijd beperken in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het identificeren van minderjarigen met behulp van uw toepassing.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: bbf885fbcca22e42b2ec0ad7ff4e7a70aa5f5828
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711967"
---
#<a name="using-age-gating-in-azure-ad-b2c"></a>Met behulp van leeftijd in Azure AD B2C beperken

>[!IMPORTANT]
>Deze functie is afgeschermd voorbeeld.  Zie onze [service blog](https://blogs.msdn.microsoft.com/azureadb2c/) wordt voor meer informatie als dit beschikbaar is of neem contact op met AADB2CFeedback@microsoft.com.  Gebruik dit niet voor productie-mappen, met behulp van deze nieuwe functies kan leiden tot verlies van gegevens en wellicht onverwachte wijzigingen in gedrag totdat we gaat u naar de algemene beschikbaarheid.  
>

##<a name="age-gating"></a>Leeftijd beperken
Leeftijd beperken, kunt u Azure AD B2C gebruiken om u te identificeren minderjarigen in uw toepassing.  U kunt voorkomen dat de gebruiker aanmeldt bij de toepassing of kunnen ze naar de toepassing met extra claims die groep van de leeftijd van de gebruiker en hun status ouderlijke toestemming te identificeren.  

>[!NOTE]
>Ouderlijke toestemming wordt bijgehouden in een gebruikerskenmerk aangeroepen `consentProvidedForMinor`.  U kunt deze eigenschap via de Graph API bijwerken en dit veld worden gebruikt bij het bijwerken van `legalAgeGroupClassification`.
>

##<a name="setting-up-your-directory-for-age-gating"></a>Instellen van uw directory voor het beperken van de leeftijd
Om te kunnen gebruiken leeftijd beperken in een gebruikersstroom, moet u uw directory om aanvullende eigenschappen configureren. Deze bewerking kan worden uitgevoerd via `Properties` in het menu (dit is alleen beschikbaar als u deel uitmaakt van de private preview bent).  
1. Klik in de extensie Azure AD B2C, op de **eigenschappen** voor uw tenant in het menu aan de linkerkant.
2. Onder de **leeftijd beperken** sectie, klikt u op de **configureren** knop.
3. Wacht tot de bewerking is voltooid en uw directory worden ingesteld voor het beperken van de leeftijd.

##<a name="enabling-age-gating-in-your-user-flow"></a>Beperken van de leeftijd in de gebruikersstroom inschakelen
Zodra uw directory is ingesteld om te beperken van de leeftijd gebruiken, klikt u vervolgens kunt u deze functie in de preview-versie gebruiker stromen.  Deze functie moet de wijzigingen die niet compatibel met bestaande soorten gebruiker stromen.  U inschakelen leeftijd beperken met de volgende stappen uit:
1. Maak een gebruikersstroom preview.
2. Zodra deze is gemaakt, gaat u naar **eigenschappen** in het menu.
3. In de **leeftijd beperken** sectie, drukt u op de wisselknop inschakelen.
4. Vervolgens kunt u kiezen hoe u wilt beheren van gebruikers die als minderjarigen identificeren.

##<a name="what-does-enabling-age-gating-do"></a>Wat is het beperken van de leeftijd inschakelen?
Zodra het beperken van de leeftijd in de gebruikersstroom is ingeschakeld, wordt door de gebruiker wijzigingen optreden.  Op de registratiepagina, worden gebruikers nu gevraagd voor hun geboortedatum en land waar u woont samen met de kenmerken van de gebruiker geconfigureerd voor de gebruikersstroom.  Op aanmelden, gebruikers die nog niet eerder zijn ingevoerd een geboortedatum en land waar u woont gevraagd om deze informatie de volgende keer dat ze zich aanmelden.  Van deze twee waarden, Azure AD B2C wordt aangeven of de gebruiker een secundaire is en werk de `ageGroup` veld de waarde kan zijn `null`, `Undefined`, `Minor`, `Adult`, en `NotAdult`.  De `ageGroup` en `consentProvidedForMinor` velden worden vervolgens gebruikt voor het berekenen van `legalAgeGroupClassification`. 

##<a name="age-gating-options"></a>Na verloop van tijd beperkende opties
U kunt Azure AD B2C blok minderjarigen zonder ouderlijke toestemming hebben of kunnen ze en de beslissingen over wat te doen met deze toepassing.  

###<a name="allowing-minors-without-parental-consent"></a>Minderjarigen zonder ouderlijke toestemming toestaan
Voor de gebruiker te definiëren die een teken dat maximaal, meld u aan of beide, kunt u toestaan minderjarigen zonder toestemming in uw toepassing.  Minderjarigen zonder ouderlijke toestemming ze mag aanmelden of registreren als normaal en Azure AD B2C een token ID met geeft de `legalAgeGroupClassification` claim.  Met behulp van deze claim kunt u de ervaring die deze gebruikers hebben, zoals doorlopen een ervaring voor het verzamelen van ouderlijke toestemming (en werk de `consentProvidedForMinor` veld).

###<a name="blocking-minors-without-parental-consent"></a>Minderjarigen zonder ouderlijke toestemming blokkeren
Voor de gebruiker te definiëren die een teken dat maximaal, meld u aan of beide, kunt u blokkeren minderjarigen zonder toestemming van toepassing.  Er zijn twee opties voor het verwerken van geblokkeerde gebruikers in Azure AD B2C:
* Verzenden van een JSON terug naar de toepassing: deze optie stuurt een reactie terug naar de toepassing of een secundaire is geblokkeerd.
* Een foutpagina - weergeven als dat een pagina geïnformeerd dat ze geen toegang de toepassing tot wordt weergegeven door de gebruiker

##<a name="known-issues"></a>Bekende problemen
###<a name="format-for-the-response-when-a-minor-is-blocked"></a>Indeling voor het antwoord wanneer een secundaire wordt geblokkeerd.
Het antwoord op dit moment is niet goed gevormd, deze fout wordt in een komende update worden verholpen.

###<a name="deleting-specific-attributes-that-were-added-during-setup-can-make-your-directory-unable-to-use-age-gating"></a>Verwijderen van specifieke kenmerken die zijn toegevoegd tijdens de installatie kan zorgen dat uw directory kan niet worden gebruikt met leeftijd beperken.
In de instellingen voor het beperken van de leeftijd die u hebt geconfigureerd uw directory via een optie in uw `Properties`.  Als u een verwijdert `legalCountry` of `dateOfBirth` via grafiek, uw directory niet meer kan gebruiken leeftijd beperken en deze eigenschappen kunnen niet opnieuw worden gemaakt.

###<a name="list-of-countries-is-incomplete"></a>Lijst met landen is onvolledig
Momenteel de lijst met landen voor legalCountry onvolledig is, voegen we toe de rest van de landen komende bij te werken.