---
title: 'Azure AD Connect: Pass through-verificatie - Veelgestelde vragen | Microsoft Docs'
description: Antwoorden op veelgestelde vragen over Azure Active Directory Pass-through-verificatie.
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, install Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: 821d70ec7236e165d4f60ed6217c49d10de1cfc3
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Azure Active Directory Pass-through-verificatie: Veelgestelde vragen

In dit artikel adres we Veelgestelde vragen over Azure Active Directory (Azure AD) Pass through-verificatie. Terug naar de nieuwe inhoud blijven controleren.

## <a name="which-of-the-azure-ad-sign-in-methods---pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs---should-i-choose"></a>Welke van de Azure AD aanmeldingsmethoden - Pass through-verificatie, synchronisatie van wachtwoordhash en Active Directory Federation Services (AD FS) - moet ik kiezen?

Dit is afhankelijk van de organisatorische vereisten en uw on-premises omgeving. Raadpleeg dit artikel voor een [vergelijking van de verschillende Azure AD-in methoden](active-directory-aadconnect-user-signin.md).

## <a name="is-pass-through-authentication-a-free-feature"></a>Pass through-verificatie een beschikbare functie is?

Pass through-verificatie is een gratis functie en u hoeft betaald edities van Azure AD om deze te gebruiken.

## <a name="is-pass-through-authentication-available-in-microsoft-cloud-germanyhttpwwwmicrosoftdecloud-deutschland-and-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Pass through-verificatie beschikbaar is in [Microsoft Cloud Duitsland](http://www.microsoft.de/cloud-deutschland) en [Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/)?

Nee,-Pass through-verificatie is alleen beschikbaar in het wereldwijde exemplaar van Azure AD.

## <a name="does-conditional-accessactive-directory-conditional-access-azure-portalmd-work-with-pass-through-authentication"></a>Biedt [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) werken met Pass through-verificatie?

Ja, worden alle mogelijkheden voor voorwaardelijke toegang, waaronder Azure multi-factor Authentication werkt met Pass through-verificatie.

## <a name="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Ondersteunt Pass-through-verificatie 'Alternatieve ID' als de gebruikersnaam, in plaats van 'userPrincipalName'?

Ja. Pass through-verificatie ondersteunt `Alternate ID` als de gebruikersnaam wanneer geconfigureerd in Azure AD Connect, zoals [hier](active-directory-aadconnect-get-started-custom.md). Niet alle Office 365-toepassingen ondersteunen `Alternate ID`. Raadpleeg de documentatie voor de instructie ondersteuning van de specifieke toepassing.

## <a name="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication"></a>Synchronisatie van wachtwoordhash fungeren als een terugvallen op Pass through-verificatie?

Nee, Pass through-verificatie _heeft geen_ automatisch een failover naar de synchronisatie van wachtwoordhash. Het fungeert alleen als een opvang voor [scenario's voor Pass-through-verificatie biedt geen ondersteuning voor vandaag](active-directory-aadconnect-pass-through-authentication-current-limitations.md#unsupported-scenarios). Gebruiker aanmelden om fouten te voorkomen, moet u configureren Pass through-verificatie voor [hoge beschikbaarheid](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="can-i-install-an-azure-ad-application-proxyactive-directory-application-proxy-get-startedmd-connector-on-the-same-server-as-a-pass-through-authentication-agent"></a>Kan ik installeren een [Azure AD-toepassingsproxy](../active-directory-application-proxy-get-started.md) connector op dezelfde server als een Pass through-verificatie-Agent?

Ja, deze configuratie wordt ondersteund door de rebranded versies van de Agent van Pass through-verificatie (versies 1.5.193.0 of hoger).

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Welke versies van Azure AD Connect en Pass through-verificatie-Agent moet u gebruiken?

U moet versie 1.1.486.0 of hoger voor Azure AD Connect en 1.5.58.0 of hoger voor de Agent Pass through-verificatie voor deze functie werkt. Alle software moet worden geïnstalleerd op servers met Windows Server 2012 R2 of hoger.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-using-pass-through-authentication"></a>Wat gebeurt er als mijn wachtwoord is verlopen en probeert u aan te melden bij het gebruik van Pass through-verificatie?

Als u hebt geconfigureerd [wachtwoord terugschrijven](../active-directory-passwords-update-your-own-password.md) voor een specifieke gebruiker, en als de gebruiker zich aanmeldt met Pass through-verificatie, ze kunnen wijzigen of hun wachtwoord opnieuw instellen. De wachtwoorden worden teruggeschreven naar de lokale Active Directory, zoals verwacht.

Echter als wachtwoord terugschrijven is niet geconfigureerd voor een specifieke gebruiker of als de gebruiker een geldige Azure AD heeft geen licentie is toegewezen, de gebruiker het wachtwoord in de cloud kan niet worden bijgewerkt. Zelfs als hun wachtwoord is verlopen kunnen ze hun wachtwoord niet bijwerken. De gebruiker in plaats daarvan ziet dit bericht: 'uw organisatie niet kunt u uw wachtwoord op deze site bij te werken. Neem bijwerken volgens de door uw organisatie aanbevolen wijze of vraag uw beheerder als u hulp nodig hebt." De gebruiker of door de beheerder heeft om in te stellen hun wachtwoord in uw lokale Active Directory.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Hoe biedt Pass through-verificatie u bescherming tegen beveiligingsaanvallen wachtwoord?

Lees [in dit artikel](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) voor meer informatie.

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Wat Pass through-verificatie Agents communiceren via poorten 80 en 443

- De verificatie-Agents maken HTTPS-aanvragen via poort 443 voor alle bewerkingen van de functie.
- De verificatie-Agents maken HTTP-aanvragen via poort 80 voor het downloaden van certificaatintrekkingslijsten SSL.

     >[!NOTE]
     >In recente updates verminderd we het aantal poorten die zijn vereist voor de functie. Als u oudere versies van Azure AD Connect of de verificatie-Agent hebt, houd deze poorten ook geopend: 5671, 8080, 9090, 9091, 9350, 9352 en 10100 10120.

## <a name="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Kan de Agents van Pass through-verificatie via een webproxyserver voor uitgaande communiceren?

Ja. Als WPAD (Web Proxy Auto-Discovery) is ingeschakeld in uw on-premises-omgeving, wordt verificatie Agents automatisch geprobeerd om te zoeken en een webproxyserver gebruikt op het netwerk.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server"></a>Kan ik twee of meer Pass through-verificatie Agents installeren op dezelfde server?

Nee, kunt u alleen een Pass through-verificatie-Agent installeren op één server. Als u wilt een Pass through-verificatie configureren voor hoge beschikbaarheid, volg de instructies in dit [artikel](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability) in plaats daarvan.

## <a name="i-already-use-active-directory-federation-services-ad-fs-for-azure-ad-sign-in-how-do-i-switch-it-to-pass-through-authentication"></a>Ik gebruik al Active Directory Federation Services (AD FS) voor aanmelding bij Azure AD. Hoe kan ik het schakelen Pass through-verificatie?

Als u AD FS hebt geconfigureerd als aanmelden methode met de Azure AD Connect-wizard, wijzigt u de gebruiker aanmelden methode in Pass through-verificatie. Deze wijziging kunt Pass through-verificatie op de tenant en converteert _alle_ uw federatieve domeinen naar beheerde domeinen. Alle volgende aanmelden aanvragen in uw tenant worden afgehandeld door Pass through-verificatie. Er is momenteel geen ondersteunde manier in Azure AD Connect een combinatie van AD FS en Pass through-verificatie gebruiken in verschillende domeinen.

Als AD FS is geconfigureerd als de methode aanmelden _buiten_ van de Azure AD Connect-wizard, wijzigt u de gebruiker aanmelden methode in Pass through-verificatie (van de optie 'Niet configureren'). Deze wijziging kunnen Pass through-verificatie op de tenant. Maar alle domeinen in uw federatieve AD FS gebruiken voor aanmelding blijven. Gebruik PowerShell handmatig enkele of alle deze federatieve domeinen te converteren naar beheerde domeinen. Na die aanvragen voor alle aanmelden op de beheerde domeinen (_alleen_) worden verwerkt door Pass through-verificatie.

>[!IMPORTANT]
>Pass through-verificatie kunnen niet worden verwerkt op aanmeldingspagina voor alleen in de cloud-Azure AD-gebruikers.

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-ad-environment"></a>Kan ik Pass through-verificatie gebruiken in een omgeving met meerdere forests AD?

Ja. Omgevingen met meerdere forests worden ondersteund als er forestvertrouwensrelaties tussen uw AD-forests en als naamachtervoegsels correct is geconfigureerd.

## <a name="how-many-pass-through-authentication-agents-do-i-need-to-install"></a>Hoeveel Pass through-verificatie Agents heb ik nodig om te installeren?

Installatie van meerdere Pass through-verificatie Agents garandeert [hoge beschikbaarheid](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability). Maar biedt geen deterministische taakverdeling tussen de Agents verificatie.

U kunt de piek- en gemiddelde belasting van de aanmeldingspagina aanvragen die u verwacht te zien op uw tenant. Als een benchmark kan één verificatie Agent 300.000-400.000 verificaties per seconde op een standard 4-core CPU, server met 16 GB RAM-geheugen verwerken. Twee of drie verificatie Agents in totaal zijn voor de meeste klanten is voldoende voor hoge beschikbaarheid en capaciteit.

Het is raadzaam dat u verificatie Agents dicht bij uw domeincontrollers installeert naar de aanmeldingspagina latentie te verbeteren.

## <a name="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect"></a>Kan ik de eerste Pass through-verificatie-Agent installeren op een andere server dan die wordt uitgevoerd van Azure AD Connect

Nee, dit scenario is _niet_ ondersteund.

## <a name="how-many-pass-through-authentication-agents-should-i-install"></a>Hoeveel Pass through-verificatie-Agents moet ik installeren?

We raden aan dat:

- U kunt twee of drie verificatie-Agents installeren in totaal. Deze configuratie is voldoende voor de meeste klanten.
- U verificatie Agents installeren op uw domeincontrollers (of als in de buurt van deze mogelijk) voor het verbeteren van de aanmeldingspagina latentie.
- U ervoor zorgen dat servers (waarbij verificatie Agents zijn geïnstalleerd) worden toegevoegd aan dezelfde AD-forest als de gebruikers waarvan de wachtwoorden moeten worden gevalideerd.

>[!NOTE]
>Er is een limiet van 12 verificatie Agents per tenant.

## <a name="how-can-i-disable-pass-through-authentication"></a>Hoe kan ik Pass through-verificatie uitschakelen?

De Azure AD Connect-wizard opnieuw uitvoeren en de gebruiker aanmelden methode van Pass through-verificatie te wijzigen in een andere methode. Deze wijziging Pass through-verificatie is uitgeschakeld op de tenant en wordt de verificatie-Agent wordt verwijderd van de server. U hebt de verificatie-Agents handmatig verwijderen van andere servers.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Wat gebeurt er wanneer ik een Pass through-verificatie-Agent verwijderen?

Verwijdert een Pass through-verificatie-Agent van een server zorgt ervoor dat het accepteren van aanmeldingsaanvragen. Zorg ervoor dat u een andere verificatie-Agent uitgevoerd voordat u deze bewerking uitvoert om te voorkomen dat gebruikers zich aanmelden op uw tenant op te splitsen.

## <a name="next-steps"></a>Volgende stappen
- [**Huidige beperkingen** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -welke scenario's worden ondersteund en welke worden niet meer.
- [**Snel starten** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - laten en Azure AD Pass-through-verificatie wordt uitgevoerd.
- [**Vergrendelen van smartcard** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -mogelijkheid slimme accountvergrendeling configureren op uw tenant gebruikersaccounts te beveiligen.
- [**Technische diepgaand** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -begrijpen hoe deze functie werkt.
- [**Problemen met** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**Beveiliging diepgaand** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -extra uitgebreide technische informatie over de functie.
- [**Azure AD naadloze eenmalige aanmelding** ](active-directory-aadconnect-sso.md) -meer informatie over deze aanvullende functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen indienen.
