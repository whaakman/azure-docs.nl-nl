---
title: 'Azure AD Connect: Pass through-verificatie - Veelgestelde vragen | Microsoft Docs'
description: Antwoorden op veelgestelde vragen over Azure Active Directory Pass-through-verificatie
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, install Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2018
ms.author: billmath
ms.openlocfilehash: 077a60949b5eed24cb9a1c56008a0073693f121e
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Azure Active Directory Pass-through-verificatie: Veelgestelde vragen

In dit artikel komen Veelgestelde vragen over Azure Active Directory (Azure AD) Pass through-verificatie. Voor bijgewerkte inhoud blijven controleren.

## <a name="which-of-the-methods-to-sign-in-to-azure-ad-pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs-should-i-choose"></a>Welke van de methoden aan te melden bij Azure AD, Pass through-verificatie, wachtwoord-hash-synchronisatie en Active Directory Federation Services (AD FS), moet ik kiezen?

Dit is afhankelijk van de organisatorische vereisten en uw on-premises omgeving. Controleer de [Azure AD Connect gebruiker aanmeldingsopties](active-directory-aadconnect-user-signin.md) artikel voor een vergelijking van de verschillende Azure AD-in methoden.

## <a name="is-pass-through-authentication-a-free-feature"></a>Pass through-verificatie een beschikbare functie is?

Pass through-verificatie is een gratis functie. U hoeft niet elke betaald edities van Azure AD om deze te gebruiken.

## <a name="is-pass-through-authentication-available-in-the-microsoft-azure-germany-cloudhttpwwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Pass through-verificatie beschikbaar is in de [Duitse van Microsoft Azure cloud](http://www.microsoft.de/cloud-deutschland) en de [Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/)?

Nee. Pass through-verificatie is alleen beschikbaar in het wereldwijde exemplaar van Azure AD.

## <a name="does-conditional-accessactive-directory-conditional-access-azure-portalmd-work-with-pass-through-authentication"></a>Biedt [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) werken met Pass through-verificatie?

Ja. Alle mogelijkheden voor voorwaardelijke toegang, waaronder Azure multi-factor Authentication werkt met Pass through-verificatie.

## <a name="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Ondersteunt Pass-through-verificatie 'Alternatieve ID' als de gebruikersnaam, in plaats van 'userPrincipalName'?

Ja. Pass through-verificatie ondersteunt `Alternate ID` als de gebruikersnaam wanneer geconfigureerd in Azure AD Connect. Zie voor meer informatie [aangepaste installatie van Azure AD Connect](active-directory-aadconnect-get-started-custom.md). Niet alle Office 365-toepassingen ondersteunen `Alternate ID`. Raadpleeg de specifieke toepassing documentatie ondersteuningsverklaring.

## <a name="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication"></a>Synchronisatie van wachtwoordhash fungeren als een terugvallen op Pass through-verificatie?

Nee. Pass through-verificatie _heeft geen_ automatisch een failover naar de synchronisatie van wachtwoordhash. Het fungeert alleen als een opvang voor [scenario's voor Pass-through-verificatie biedt geen ondersteuning voor vandaag](active-directory-aadconnect-pass-through-authentication-current-limitations.md#unsupported-scenarios). Gebruiker aanmelden om fouten te voorkomen, moet u configureren Pass through-verificatie voor [hoge beschikbaarheid](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="can-i-install-an-azure-ad-application-proxyactive-directory-application-proxy-get-startedmd-connector-on-the-same-server-as-a-pass-through-authentication-agent"></a>Kan ik installeren een [Azure AD-toepassingsproxy](../active-directory-application-proxy-get-started.md) connector op dezelfde server als een Pass through-verificatie-Agent?

Ja. De rebranded versies van de Pass through-verificatie-Agent, versie 1.5.193.0 of hoger, ondersteunt deze configuratie.

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Welke versies van Azure AD Connect en Pass through-verificatie-Agent moet u gebruiken?

Voor deze functie te gebruiken, moet u versie 1.1.486.0 of hoger voor Azure AD Connect en 1.5.58.0 of hoger voor de Agent van Pass through-verificatie. Alle software installeren op servers met Windows Server 2012 R2 of hoger.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication"></a>Wat gebeurt er als mijn wachtwoord is verlopen en proberen aan te melden met behulp van Pass through-verificatie?

Als u hebt geconfigureerd [wachtwoord terugschrijven](../active-directory-passwords-update-your-own-password.md) voor een specifieke gebruiker, en als de gebruiker zich aanmeldt met behulp van Pass through-verificatie, ze kunnen wijzigen of hun wachtwoord opnieuw instellen. De wachtwoorden worden teruggeschreven naar de lokale Active Directory, zoals verwacht.

Als u terugschrijven van wachtwoord voor een specifieke gebruiker niet hebt geconfigureerd of als de gebruiker beschikt niet over een geldige Azure AD licentie is toegewezen, de gebruiker het wachtwoord in de cloud kan niet worden bijgewerkt. Zelfs als hun wachtwoord is verlopen kunnen ze hun wachtwoord niet bijwerken. De gebruiker in plaats daarvan ziet dit bericht: 'uw organisatie niet kunt u uw wachtwoord op deze site bij te werken. Bijwerken volgens de door uw organisatie aanbevolen wijze of vraag uw beheerder als u hulp nodig hebt." De gebruiker of beheerder moet hun wachtwoord in de lokale Active Directory worden ingesteld.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Hoe biedt Pass through-verificatie u bescherming tegen aanvallen met brute kracht wachtwoord?

Lees [Azure Active Directory Pass-through-verificatie: Smart accountvergrendeling](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) voor meer informatie.

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Wat Pass through-verificatie Agents communiceren via poorten 80 en 443

- De verificatie-Agents maken HTTPS-aanvragen via poort 443 voor alle bewerkingen van de functie.
- De verificatie-Agents maken HTTP-aanvragen via poort 80 voor het downloaden van de certificaatintrekkingslijsten (CRL's) van SSL.

     >[!NOTE]
     >Recente updates beperkt het aantal poorten dat de functie is vereist. Als u oudere versies van Azure AD Connect of de verificatie-Agent hebt, houd deze poorten open ook: 5671, 8080 9090, 9091, 9350, 9352 en 10100 10120.

## <a name="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Kan de Agents van Pass through-verificatie via een webproxyserver voor uitgaande communiceren?

Ja. Als WPAD Web Proxy Auto-Discovery () is ingeschakeld in uw on-premises-omgeving, wordt verificatie Agents automatisch geprobeerd om te zoeken en een webproxyserver gebruikt op het netwerk.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server"></a>Kan ik twee of meer Pass through-verificatie Agents installeren op dezelfde server?

Nee, kunt u alleen een Pass through-verificatie-Agent installeren op één server. Als u wilt een Pass through-verificatie configureren voor hoge beschikbaarheid, volg de instructies in [Azure Active Directory Pass-through-verificatie: snel starten](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="how-do-i-remove-a-pass-through-authentication-agent"></a>Hoe kan ik een Pass through-verificatie-Agent verwijderen?

Als een Pass through-verificatie-Agent wordt uitgevoerd, blijft actief en aanmelden gebruikersaanvragen voortdurend worden verwerkt. Als u een verificatie-Agent verwijderen wilt, gaat u naar **Configuratiescherm -> programma's -> programma's en onderdelen** en verwijderen van zowel de **Microsoft Azure AD Connect-verificatie Agent** en de  **Microsoft Azure AD Connect Agent Updater** programma's.

Als u de blade Pass through-verificatie inschakelt op de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) nadat de vorige stap is voltooid, ziet u de verificatie-Agent weergegeven als **inactief**. Dit is _verwacht_. De verificatie-Agent wordt automatisch verwijderd uit de lijst na een paar dagen.

## <a name="i-already-use-ad-fs-to-sign-in-to-azure-ad-how-do-i-switch-it-to-pass-through-authentication"></a>Ik heb al AD FS aan te melden bij Azure AD gebruiken. Hoe kan ik het schakelen Pass through-verificatie?

Als u AD FS hebt geconfigureerd als uw contactmethode aanmelden via de Azure AD Connect-wizard, wijzigt u de methode die de gebruiker zich aanmeldt bij Pass through-verificatie gebruikt. Deze wijziging kunt Pass through-verificatie op de tenant en converteert _alle_ uw federatieve domeinen in beheerde domeinen. Pass through-verificatie verwerkt alle volgende aanvragen aan te melden bij uw tenant. Er is momenteel geen ondersteunde manier in Azure AD Connect een combinatie van AD FS en Pass through-verificatie gebruiken in verschillende domeinen.

Als AD FS is geconfigureerd als de methode aan te melden _buiten_ de Azure AD Connect-wizard methode wijzigen de gebruiker aanmelden op Pass through-verificatie. Kunt u deze wijziging van de **niet configureert** optie. Deze wijziging kunnen Pass through-verificatie op de tenant, maar blijven alle domeinen in uw federatieve AD FS gebruiken voor aanmelding. Gebruik PowerShell handmatig enkele of alle van deze domeinen federatieve converteren naar beheerde domeinen. Nadat u deze wijziging hebt aangebracht *alleen* Pass through-verificatie alle aanvragen aan te melden bij de beheerde domeinen worden verwerkt.

>[!IMPORTANT]
>Pass through-verificatie kunnen niet worden verwerkt aanmelden voor alleen in de cloud-Azure AD-gebruikers.

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-active-directory-environment"></a>Kan ik Pass through-verificatie gebruiken in een Active Directory-omgeving met meerdere forests?

Ja. Omgevingen met meerdere forests worden ondersteund als er forestvertrouwensrelaties tussen uw Active Directory-forests en als naamachtervoegsels correct is geconfigureerd.

## <a name="how-many-pass-through-authentication-agents-do-i-need-to-install"></a>Hoeveel Pass through-verificatie Agents heb ik nodig om te installeren?

Installatie van meerdere Pass through-verificatie Agents garandeert [hoge beschikbaarheid](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability). Maar biedt geen deterministische taakverdeling tussen de Agents verificatie.

U kunt de piek- en gemiddelde belasting van de aanmeldingspagina aanvragen die u verwacht te zien op uw tenant. Als een benchmark kan één verificatie Agent 300-400-verificaties per seconde op een standard 4-core CPU, 16 GB RAM-geheugen server verwerken.

Gebruik de volgende richtlijn voor een schatting van netwerkverkeer:
- Elke aanvraag heeft een grootte van de nettolading van (0.5K + 1 K * num_of_agents) bytes. dat wil zeggen, gegevens van Azure AD voor de verificatie-Agent. Hier geeft 'num_of_agents' aan dat het aantal Agents verificatie geregistreerd op uw tenant.
- Elk antwoord heeft een grootte van de nettolading van 1K bytes. dat wil zeggen, gegevens uit de verificatie-Agent naar Azure AD.

Twee of drie verificatie Agents in totaal zijn voor de meeste klanten is voldoende voor hoge beschikbaarheid en capaciteit. Dicht bij uw domeincontrollers naar de aanmeldingspagina latentie verbeteren, moet u verificatie Agents installeren.

>[!NOTE]
>Er is een limiet van 12 verificatie Agents per tenant.

## <a name="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect"></a>Kan ik de eerste Pass through-verificatie-Agent installeren op een andere server dan die wordt uitgevoerd van Azure AD Connect

Nee, dit scenario is _niet_ ondersteund.

## <a name="how-can-i-disable-pass-through-authentication"></a>Hoe kan ik Pass through-verificatie uitschakelen?

De Azure AD Connect-wizard opnieuw uitvoeren en de gebruiker aanmelden methode van Pass through-verificatie te wijzigen in een andere methode. Deze wijziging Pass through-verificatie is uitgeschakeld op de tenant en wordt de verificatie-Agent wordt verwijderd van de server. U moet de verificatie-Agents handmatig verwijderen uit de andere servers.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Wat gebeurt er wanneer ik een Pass through-verificatie-Agent verwijderen?

Als u een Pass through-verificatie-Agent van een server verwijderen, worden de server aanmelden aanvragen meer accepteren. Om te voorkomen dat de functionaliteit van de aanmeldingspagina gebruikers op uw tenant, zorg ervoor dat u een andere verificatie-Agent wilt uitvoeren voordat u een Pass through-verificatie-Agent verwijderen.

## <a name="next-steps"></a>Volgende stappen
- [Huidige beperkingen](active-directory-aadconnect-pass-through-authentication-current-limitations.md): meer informatie over welke scenario's worden ondersteund en welke worden niet.
- [Snel starten](active-directory-aadconnect-pass-through-authentication-quick-start.md): actief en werkend krijgen voor Azure AD Pass-through-verificatie.
- [Vergrendelen van smartcard](active-directory-aadconnect-pass-through-authentication-smart-lockout.md): informatie over het configureren van de mogelijkheid slimme vergrendeling op uw tenant gebruikersaccounts te beveiligen.
- [Technische diepgaand](active-directory-aadconnect-pass-through-authentication-how-it-works.md): inzicht in hoe de functie Pass through-verificatie werkt.
- [Problemen met](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): informatie over het oplossen van veelvoorkomende problemen met de functie Pass through-verificatie.
- [Beveiliging diepgaand](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): gedetailleerde technische informatie over de functie Pass through-verificatie.
- [Azure AD naadloze eenmalige aanmelding](active-directory-aadconnect-sso.md): meer informatie over deze aanvullende functie.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): de Azure Active Directory-Forum gebruiken om nieuwe functieaanvragen.

