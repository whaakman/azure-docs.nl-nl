---
title: Voorwaardelijke toegang voor gebruikers van Azure Active Directory B2B-samenwerking | Microsoft Docs
description: Azure Active Directory B2B-samenwerking ondersteunt multi-factor authentication (MFA) voor selectief toegang tot uw zakelijke toepassingen
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 09/11/2017
ms.author: sasubram
ms.openlocfilehash: 2f2cfc351d372d665aac054d52d6e1520e1ffe48
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="conditional-access-for-b2b-collaboration-users"></a>Voorwaardelijke toegang voor gebruikers voor B2B-samenwerking

## <a name="multi-factor-authentication-for-b2b-users"></a>Multi-factor authentication voor B2B-gebruikers
Met de Azure AD B2B-samenwerking organisaties kunnen multi-factor authentication (MFA) beleid afdwingen voor B2B-gebruikers. Dit beleid kunnen worden afgedwongen op de tenant, app niveau of een afzonderlijke gebruiker, dezelfde manier als ze zijn ingeschakeld voor fulltime werknemers en leden van de organisatie. MFA-beleid worden afgedwongen voor de organisatie van de bronpartner.

Voorbeeld:
1. Beheerder of de werknemer in bedrijf A nodigt gebruiker uit bedrijf B naar een toepassing *Foo* in bedrijf A.
2. Toepassing *Foo* in bedrijf A is geconfigureerd voor MFA vereisen voor toegang.
3. Wanneer de gebruiker van bedrijf B probeert toegang tot app *Foo* in het bedrijf een tenant wordt gevraagd een challenge MFA voltooien.
4. De gebruiker hun MFA met bedrijf A kunt instellen en de MFA-optie kiest.
5. Dit scenario is geschikt voor een identiteit (Azure AD of MSA, bijvoorbeeld, als gebruikers van bedrijf B zich verifiëren met behulp van sociale ID)
6. Bedrijf A moet voldoende Azure AD Premium-licenties die ondersteuning bieden voor MFA hebt. De gebruiker uit bedrijf B verbruikt deze licentie van bedrijf A.

De uitnodiging tenancymodus is altijd verantwoordelijk zijn voor MFA voor gebruikers van de partnerorganisatie, zelfs als de andere organisatie MFA mogelijkheden heeft.

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a>MFA in te stellen voor gebruikers voor B2B-samenwerking
Als u wilt ontdekken hoe eenvoudig het is voor het instellen van MFA voor B2B-samenwerking gebruikers, Zie hoe in de volgende video:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a>B2B-gebruikers MFA voor ervaren bieden inwisseling
Bekijk de volgende animatie om te zien van de inwisselcode ervaring:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a>MFA voor B2B-samenwerking gebruikers opnieuw instellen
Op dit moment wordt kan de beheerder vereisen B2B-samenwerking gebruikers bewijs van opnieuw alleen met behulp van de volgende PowerShell-cmdlets:

1. Verbinding maken met Azure AD

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. Alle gebruikers krijgen met bewijs van methoden

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  Hier volgt een voorbeeld:

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. De MFA-methode voor een specifieke gebruiker moet de gebruiker B2B-samenwerking opnieuw in te stellen bewijs van methoden Reset. Voorbeeld:

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-the-resource-tenancy"></a>Waarom we MFA op de resource-tenancymodus uitvoeren?

In de huidige versie is MFA altijd in de tenancymodus bron omwille van de hoge voorspelbaarheid is vereist. Stel dat bijvoorbeeld een gebruiker Contoso (Sandra) wordt verzocht Fabrikam en Fabrikam MFA voor B2B-gebruikers heeft ingeschakeld.

Als Contoso ingeschakeld voor App1, maar niet App2 MFA-beleid heeft, klikt u vervolgens als we de claim Contoso MFA in het token kijken mogelijk zien we het volgende probleem:

* Dag 1: Een gebruiker heeft MFA in Contoso en toegang heeft tot App1 en er zijn geen extra MFA prompt wordt weergegeven in Fabrikam.

* Dag 2: De gebruiker toegang heeft gehad tot 2 van de App in Contoso, dus bij het openen van Fabrikam, moeten ze zich registreren voor MFA er.

Dit proces kan verwarrend zijn en kan leiden tot neerzetten in aanmelden voltooiingen.

Bovendien, zelfs als Contoso MFA-mogelijkheid heeft, is het niet altijd dat het geval de Fabrikam zou vertrouwt de Contoso-MFA-beleid.

Tot slot werkt resource tenant MFA ook voor MSA's en sociale-id's en voor de partner-organisaties waarvoor geen MFA instellen.

Daarom is de aanbeveling voor MFA voor B2B gebruikers altijd om MFA te vereisen in de uitnodiging tenant. Deze vereiste kan ertoe leiden dat dubbele MFA in sommige gevallen, maar wanneer toegang tot de uitnodiging tenant, de ervaring voor eindgebruikers is voorspelbaar: Sandra moet registreren voor MFA bij de uitnodiging tenant.

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a>Op basis van apparaten, op basis van locatie en risico gebaseerde voorwaardelijke toegang voor B2B-gebruikers

Wanneer Contoso beleidsregels voor voorwaardelijke toegang op basis van apparaten voor hun zakelijke gegevens kunt, wordt toegang niet van apparaten die niet beheerd door Contoso en niet compatibel zijn met het apparaatbeleid Contoso worden.

Als het apparaat van de B2B-gebruiker niet wordt beheerd door Contoso, wordt toegang van gebruikers van de partnerorganisaties B2B geblokkeerd in welke context deze beleidsregels worden afgedwongen. Contoso kan echter uitsluitingslijsten met specifieke partner-gebruikers uitgesloten van het beleid voor voorwaardelijke toegang op basis van apparaten te maken.

#### <a name="location-based-conditional-access-for-b2b"></a>Voorwaardelijke toegang voor B2B op basis van locatie

Beleid voor voorwaardelijke toegang op basis van locatie kunnen worden afgedwongen voor B2B-gebruikers als de organisatie uitnodigen kunnen maken van een vertrouwde IP-adresbereik dat hun partnerorganisaties definieert.

#### <a name="risk-based-conditional-access-for-b2b"></a>Voorwaardelijke toegang op basis van een risico voor B2B

Worden op dit moment aanmelden beleid op basis van risico's kunnen niet toegepast voor B2B-gebruikers de risico-evaluatie wordt uitgevoerd op de B2B-gebruiker thuisorganisatie.

## <a name="next-steps"></a>Volgende stappen

Lees ook onze andere artikelen over Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hoe voeg beheerders van Azure Active Directory B2B-samenwerking gebruikers?](active-directory-b2b-admin-add-users.md)
* [Hoe kunnen IT-medewerkers B2B-samenwerking gebruikers toevoegen](active-directory-b2b-iw-add-users.md)
* [De elementen van de uitnodigingsmail voor B2B-samenwerking](active-directory-b2b-invitation-email.md)
* [B2B-samenwerking uitnodiging inwisseling](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B-samenwerking licentieverlening](active-directory-b2b-licensing.md)
* [Het oplossen van Azure Active Directory B2B-samenwerking](active-directory-b2b-troubleshooting.md)
* [Azure Active Directory B2B-samenwerking Veelgestelde vragen (FAQ)](active-directory-b2b-faq.md)
* [Azure Active Directory B2B-samenwerking API en de aanpassing](active-directory-b2b-api.md)
* [B2B-samenwerking gebruikers zonder een uitnodiging toevoegen](active-directory-b2b-add-user-without-invite.md)
* [Artikel index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
