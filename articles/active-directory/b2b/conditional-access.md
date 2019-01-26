---
title: Voorwaardelijke toegang voor gebruikers van Azure Active Directory B2B-samenwerking | Microsoft Docs
description: Azure Active Directory B2B-samenwerking ondersteunt multi-factor authentication (MFA) voor selectief toegang tot uw zakelijke toepassingen
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.openlocfilehash: d692392ba28899924ca42f7df47311d949633862
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55080006"
---
# <a name="conditional-access-for-b2b-collaboration-users"></a>Voorwaardelijke toegang voor gebruikers van B2B-samenwerking

## <a name="multi-factor-authentication-for-b2b-users"></a>Multi-factor authentication voor B2B-gebruikers
Met Azure AD B2B-samenwerking, kunnen organisaties multi-factor authentication (MFA)-beleid voor B2B-gebruikers afdwingen. Dit beleid kunnen worden afgedwongen op het niveau van de tenant, app, of individuele gebruiker, de dezelfde manier dat ze zijn ingeschakeld voor fulltime werknemers en leden van de organisatie. MFA-beleid worden afgedwongen voor de bronorganisatie.

Voorbeeld:
1. Beheerder of informatie worker in bedrijf A nodigt gebruikers van bedrijf B naar een toepassing *Foo* in bedrijf A.
2. Toepassing *Foo* in bedrijf A is geconfigureerd voor MFA vereisen voor toegang.
3. Wanneer de gebruiker van bedrijf B probeert te krijgen tot app *Foo* in het bedrijf een tenant, dan wordt hen gevraagd een MFA-controle voltooien.
4. De gebruiker hun MFA bij een bedrijf kunt instellen en de MFA-optie kiest.
5. In dit scenario werkt voor een identiteit (Azure AD of MSA, bijvoorbeeld, als gebruikers in bedrijf B verifiëren met behulp van sociale ID)
6. Een bedrijf moet voldoende Azure AD Premium-licenties die ondersteuning bieden voor MFA hebt. De gebruiker van bedrijf B neemt deze licentie van bedrijf A.

De uitnodigende tenancy is altijd verantwoordelijk voor MFA voor gebruikers van de partnerorganisatie, zelfs als de partnerorganisatie MFA mogelijkheden heeft.

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a>MFA in te stellen voor gebruikers van B2B-samenwerking
Om te ontdekken hoe eenvoudig het is het instellen van MFA voor gebruikers van B2B-samenwerking, Zie hoe u in de volgende video:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a>B2B-gebruikers voor MFA-ervaring bieden inwisselen
Bekijk de volgende animatie om te zien van de inschrijving ervaring:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a>MFA opnieuw instellen voor gebruikers van B2B-samenwerking
Op dit moment kan de beheerder vereisen gebruikers van B2B-samenwerking bewijs van opnieuw alleen met behulp van de volgende PowerShell-cmdlets:

1. Verbinding maken met Azure AD

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. Ophalen van alle gebruikers met een bewijs van methoden

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  Hier volgt een voorbeeld:

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. Opnieuw instellen van de MFA-methode voor een specifieke gebruiker moet de gebruiker B2B-samenwerking bewijs van methoden opnieuw instellen. Voorbeeld:

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-the-resource-tenancy"></a>Waarom we MFA uitvoeren op de resource-tenants?

In de huidige versie is de MFA altijd in de resource-tenants, om redenen van voorspelbaarheid. Stel dat bijvoorbeeld een Contoso-gebruiker (Sandra) wordt uitgenodigd voor Fabrikam en Fabrikam MFA is ingeschakeld voor B2B-gebruikers.

Als Contoso ingeschakeld voor App1, maar niet App2 MFA-beleid heeft, klikt u vervolgens als we kijken naar de Contoso-MFA-claim in het token wordt mogelijk zien we het volgende probleem:

* Dag 1: Een gebruiker heeft van MFA in Contoso en toegang heeft tot App1 en geen extra MFA prompt wordt weergegeven in Fabrikam.

* Dag 2: De gebruiker toegang heeft gehad tot App 2 in Contoso, dus nu bij het openen van Fabrikam, moeten ze zich registreren voor MFA er.

Dit proces kan verwarrend zijn en kan leiden tot neerzetten in aanvullen met aanmelden.

Bovendien, zelfs als Contoso MFA mogelijkheid heeft, is het niet altijd dat het geval de Fabrikam zou vertrouwt de Contoso-MFA-beleid.

Tot slot werkt resource tenant MFA ook voor MSA's en sociale-id's en partner organisaties waarvoor geen MFA instellen.

Daarom is de aanbeveling voor MFA voor B2B-gebruikers altijd om MFA te vereisen in de uitnodigende tenant. Deze vereiste kan leiden tot dubbele MFA in sommige gevallen, maar wanneer u toegang wilt tot de uitnodigende tenant, de ervaring voor eindgebruikers voorspelbare is: Sandra moet aan de uitnodigende tenant worden geregistreerd voor MFA.

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a>Op basis van apparaat, op basis van locatie en risico's gebaseerde voorwaardelijke toegang voor B2B-gebruikers

Bij Contoso beleid voor voorwaardelijke toegang op basis van apparaten voor hun zakelijke gegevens kunt, wordt toegang vanaf apparaten die niet beheerd door Contoso en niet compatibel zijn met de Contoso-apparaatbeleid worden voorkomen.

Als het apparaat van de B2B-gebruiker niet wordt beheerd door Contoso, wordt de toegang van B2B-gebruikers van de partnerorganisaties geblokkeerd in welke context deze beleidsregels worden afgedwongen. Contoso kan echter uitsluitingslijsten met specifieke partner gebruikers uitgesloten van het beleid voor voorwaardelijke toegang op basis van het apparaat te maken.

#### <a name="location-based-conditional-access-for-b2b"></a>Voorwaardelijke toegang op basis van locatie voor B2B

Beleid voor voorwaardelijke toegang op basis van locatie kunnen worden afgedwongen voor B2B-gebruikers als de organisatie kan maken van een vertrouwde IP-adresbereik dat hun partnerorganisaties definieert.

#### <a name="risk-based-conditional-access-for-b2b"></a>Risico's gebaseerde voorwaardelijke toegang voor B2B

Op dit moment kunnen niet aanmelden risicobeleid voor B2B-gebruikers worden toegepast omdat de risico-evaluatie wordt uitgevoerd op de startpagina organisatie van de B2B-gebruiker.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen op Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
* [Licenties voor Azure AD B2B-samenwerking](licensing-guidance.md)
* [Veelgestelde vragen over Azure Active Directory B2B-samenwerking](faq.md)
