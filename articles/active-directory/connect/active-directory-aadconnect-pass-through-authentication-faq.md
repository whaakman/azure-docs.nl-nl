---
title: 'Azure AD Connect: Pass through-verificatie - Veelgestelde vragen | Microsoft Docs'
description: Antwoorden op veelgestelde vragen over Azure Active Directory Pass through-verificatie
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, installatie van Active Directory, vereiste onderdelen voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 6d5cd79a6336b2e5c4b3c5c6f5765d92cd602552
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048965"
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Azure Active Directory Pass through-verificatie: Veelgestelde vragen

In dit artikel komen de antwoorden op veelgestelde vragen over Azure Active Directory (Azure AD) Pass through-verificatie. Voor bijgewerkte inhoud blijven controleren.

## <a name="which-of-the-methods-to-sign-in-to-azure-ad-pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs-should-i-choose"></a>Welke methoden voor het aanmelden bij Azure AD, Pass-through-verificatie, wachtwoord-hash-synchronisatie en Active Directory Federation Services (AD FS), moet ik kiezen?

Dat hangt ervan af op uw on-premises omgeving en vereisten van de organisatie. Controleer de [Azure AD Connect-gebruiker aanmelden opties](active-directory-aadconnect-user-signin.md) artikel voor een vergelijking van de verschillende Azure AD aanmelden methoden.

## <a name="is-pass-through-authentication-a-free-feature"></a>Een gratis functie voor Pass through-verificatie is?

Pass through-verificatie is een gratis functie. U hoeft niet elke betaalde versies van Azure AD om deze te gebruiken.

## <a name="is-pass-through-authentication-available-in-the-microsoft-azure-germany-cloudhttpwwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Pass through-verificatie beschikbaar is in de [cloud van Microsoft Azure Duitsland](http://www.microsoft.de/cloud-deutschland) en de [Microsoft Azure Government-cloud](https://azure.microsoft.com/features/gov/)?

Nee. Pass through-verificatie is alleen beschikbaar in het wereldwijde exemplaar van Azure AD.

## <a name="does-conditional-accessactive-directory-conditional-access-azure-portalmd-work-with-pass-through-authentication"></a>Biedt [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) werken met Pass through-verificatie?

Ja. Alle mogelijkheden voor voorwaardelijke toegang, met inbegrip van Azure multi-factor Authentication, werken met Pass through-verificatie.

## <a name="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Ondersteunt Pass-through-verificatie 'Alternatieve ID' als de gebruikersnaam, in plaats van 'userPrincipalName'?

Ja. Pass through-verificatie ondersteunt `Alternate ID` als de gebruikersnaam wanneer in Azure AD Connect hebt geconfigureerd. Zie voor meer informatie, [aangepaste installatie van Azure AD Connect](active-directory-aadconnect-get-started-custom.md). Niet alle Office 365-toepassingen ondersteuning bieden voor `Alternate ID`. Raadpleeg de documentatie-ondersteuningsverklaring van de specifieke toepassing.

## <a name="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication"></a>Wachtwoord-hashsynchronisatie fungeren als een terugval naar Pass-through-verificatie?

Nee. Pass through-verificatie _niet_ automatisch een failover naar wachtwoord-hashsynchronisatie. Het fungeert alleen als alternatieve methode voor [scenario's voor Pass through-verificatie biedt geen ondersteuning voor vandaag](active-directory-aadconnect-pass-through-authentication-current-limitations.md#unsupported-scenarios). Om te voorkomen dat een gebruiker aanmeldingen dat is toegestaan, moet u Pass-through-verificatie voor configureren [hoge beschikbaarheid](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="can-i-install-an-azure-ad-application-proxymanage-appsapplication-proxymd-connector-on-the-same-server-as-a-pass-through-authentication-agent"></a>Kan ik Installeer een [Azure AD-toepassingsproxy](../manage-apps/application-proxy.md) connector op dezelfde server als een Pass through-verificatie-Agent?

Ja. De rebranded versies van de Pass through-verificatie-Agent, versie 1.5.193.0 of hoger, ondersteuning voor deze configuratie.

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Welke versies van Azure AD Connect en Pass through-verificatie-Agent hebt u nodig?

Voor deze functie te gebruiken, moet u versie 1.1.486.0 of hoger voor Azure AD Connect en 1.5.58.0 of hoger voor de verificatie-Agent voor Pass Through-query. Alle software installeren op servers met Windows Server 2012 R2 of hoger.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication"></a>Wat gebeurt er als mijn gebruikerswachtwoord is verlopen en probeert u aan te melden met behulp van Pass through-verificatie?

Als u hebt geconfigureerd [wachtwoord terugschrijven](../user-help/active-directory-passwords-update-your-own-password.md) voor een specifieke gebruiker, en als de gebruiker zich aanmeldt met behulp van Pass through-verificatie, kunnen ze wijzigen of hun wachtwoord opnieuw instellen. De wachtwoorden worden teruggeschreven naar on-premises Active Directory, zoals verwacht.

Als u het terugschrijven van wachtwoorden voor een specifieke gebruiker niet hebt geconfigureerd of als de gebruiker beschikt niet over een geldige Azure AD-licentie is toegewezen, de gebruiker het wachtwoord in de cloud kan niet bijwerken. Het wachtwoord bijwerken niet, zelfs als het wachtwoord is verlopen. De gebruiker in plaats daarvan ziet dit bericht: 'uw organisatie niet kunt u uw wachtwoord op deze site bij te werken. Bijgewerkt op basis van de door uw organisatie aanbevolen wijze of vraag uw beheerder als u hulp nodig hebt." De gebruiker of beheerder moet de hun on-premises Active Directory-wachtwoord opnieuw instellen.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Hoe biedt Pass through-verificatie u bescherming tegen aanvallen met brute kracht wachtwoord?

Lezen [Azure Active Directory Pass through-verificatie: Smart Lockout](../authentication/howto-password-smart-lockout.md) voor meer informatie.

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Wat Pass through-verificatie-Agents communiceren via de poorten 80 en 443

- De verificatie-Agents moet HTTPS-aanvragen via poort 443 voor alle bewerkingen van de functie.
- De verificatie-Agents moet HTTP-aanvragen via poort 80 voor het downloaden van de SSL-certificaatintrekkingslijsten (CRL's).

     >[!NOTE]
     >Recente updates beperkt het aantal poorten die door de functie is vereist. Als u oudere versies van Azure AD Connect of de verificatie-Agent hebt, houd deze poorten open ook: 5671, 8080 9090, 9091, 9350, 9352 en 10100 10120.

## <a name="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Kan de Pass through-verificatie-Agents communiceren via een uitgaande webproxyserver?

Ja. Als Web Proxy Auto-Discovery (WPAD) is ingeschakeld in uw on-premises-omgeving, wordt de verificatie-Agents automatisch probeert om te zoeken en een webproxyserver gebruikt op het netwerk.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server"></a>Kan ik twee of meer Pass through-verificatie-Agents installeren op dezelfde server?

Nee, kunt u alleen een Pass through-verificatie-Agent installeren op één server. Als u configureren van Pass-through-verificatie voor maximale beschikbaarheid wilt, volg de instructies in [Azure Active Directory Pass through-verificatie: snel starten](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="how-do-i-remove-a-pass-through-authentication-agent"></a>Hoe kan ik een Pass through-verificatie-Agent verwijderen?

Als een Pass through-verificatie-Agent wordt uitgevoerd, blijft actief en gebruiker aanmeldingsaanvragen voortdurend worden verwerkt. Als u een verificatie-Agent verwijderen wilt, gaat u naar **Configuratiescherm -> programma's -> programma's en onderdelen** en verwijderen van zowel de **Microsoft Azure AD Connect Authentication-Agent** en de  **Microsoft Azure AD Connect-Agent Updater** programma's.

Als u de blade Pass through-verificatie inschakelt op de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) nadat de vorige stap is voltooid, ziet u de verificatie-Agent weergegeven als **inactief**. Dit is _verwacht_. De verificatie-Agent wordt automatisch verwijderd uit de lijst met na een paar dagen.

## <a name="i-already-use-ad-fs-to-sign-in-to-azure-ad-how-do-i-switch-it-to-pass-through-authentication"></a>Ik AD FS gebruiken voor het aanmelden bij Azure AD. Hoe kan ik het schakelen naar Pass-through-verificatie?

Als u AD FS hebt geconfigureerd als de methode aan te melden bij de Azure AD Connect-wizard, wijzigt u de methode die de gebruiker zich aanmeldt bij Pass through-verificatie gebruikt. Deze wijziging, schakelt u Pass-through-verificatie op de tenant en converteert _alle_ uw federatieve domeinen in beheerde domeinen. Pass through-verificatie verwerkt alle volgende aanvragen te melden bij uw tenant. Er is op dit moment geen ondersteunde manier in Azure AD Connect een combinatie van AD FS en Pass through-verificatie gebruiken in verschillende domeinen.

Als AD FS is geconfigureerd als de methode aan te melden bij _buiten_ de Azure AD Connect-wizard methode wijzigen het aanmelden van gebruikers naar Pass-through-verificatie. Kunt u deze wijziging van de **niet configureert** optie. Door deze wijziging kunt Pass through-verificatie voor de tenant, maar uw federatieve domeinen worden echter ook doorgaan met AD FS voor aanmelding bij. PowerShell gebruiken voor het handmatig enkele of al deze federatieve domeinen converteren naar beheerde domeinen. Nadat u deze wijziging hebt aangebracht *alleen* alle aanvragen te melden bij de beheerde domeinen voor Pass through-verificatie afhandelt.

>[!IMPORTANT]
>Pass through-verificatie niet afhandelen van aanmelding voor alleen-cloud Azure AD-gebruikers.

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-active-directory-environment"></a>Kan ik Pass through-verificatie gebruiken in een omgeving met meerdere forests Active Directory?

Ja. Omgevingen met meerdere forests worden ondersteund als er forestvertrouwensrelaties tussen uw Active Directory-forests en als naamachtervoegsels correct is geconfigureerd.

## <a name="how-many-pass-through-authentication-agents-do-i-need-to-install"></a>Hoeveel Pass through-verificatie-Agents heb ik nodig om te installeren?

Meerdere Pass through-verificatie-Agents installeren garandeert [hoge beschikbaarheid](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability). Maar biedt niet deterministisch taakverdeling tussen de verificatie-Agents.

Houd rekening met de piek- en gemiddelde belasting van aanmeldingsaanvragen die u verwacht te zien op uw tenant. Als een benchmark kunnen 300-400 verificaties per seconde op een standaard CPU met 4 kernen, 16 GB RAM-server worden verwerkt door een afzonderlijke verificatie-Agent.

Voor een schatting van netwerkverkeer, gebruik de volgende richtlijn voor formaatbepaling:
- Elke aanvraag heeft een grootte van de nettolading van (0.5K + 1 K * num_of_agents) bytes. dat wil zeggen gegevens uit Azure AD de verificatie-Agent. Hier 'num_of_agents' geeft aan dat het aantal verificatie-Agents op uw tenant is geregistreerd.
- Elk antwoord heeft een grootte van de nettolading van 1K bytes. dat wil zeggen gegevens uit de verificatie-Agent naar Azure AD.

Twee of drie verificatie-Agents in totaal zijn voor de meeste klanten voldoende voor hoge beschikbaarheid en capaciteit. U moet verificatie-Agents installeren dicht bij uw domeincontrollers voor het aanmelden latentie verbeteren.

>[!NOTE]
>Er is een limiet van 12 verificatie-Agents per tenant.

## <a name="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect"></a>Kan ik de eerste Pass through-verificatie-Agent installeren op een andere server dan die wordt uitgevoerd van Azure AD Connect?

Nee, dit scenario is _niet_ ondersteund.

## <a name="how-can-i-disable-pass-through-authentication"></a>Hoe kan ik Pass through-verificatie uitschakelen?

De Azure AD Connect-wizard opnieuw uitvoeren en wijzig de aanmeldingsmethode gebruiker van Pass through-verificatie in een andere methode. Deze wijziging wordt uitgeschakeld Pass through-verificatie voor de tenant en wordt de verificatie-Agent verwijderd van de server. U moet de verificatie-Agents handmatig verwijderen van de andere servers.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Wat gebeurt er wanneer ik een Pass through-verificatie-Agent verwijderen?

Als u een Pass through-verificatie-Agent van een server verwijderen, worden de server om te stoppen met het accepteren van aanmeldingsaanvragen. Om te voorkomen dat de gebruiker aanmelden mogelijkheid belangrijke op uw tenant, controleert u of een andere verificatie-Agent wordt uitgevoerd voordat u een Pass through-verificatie-Agent verwijderen.

## <a name="next-steps"></a>Volgende stappen
- [Huidige beperkingen](active-directory-aadconnect-pass-through-authentication-current-limitations.md): informatie over welke scenario's worden ondersteund en welke niet.
- [Quick start-](active-directory-aadconnect-pass-through-authentication-quick-start.md): aan de slag op Azure AD Pass-through-verificatie.
- [Vergrendeling van het smart](../authentication/howto-password-smart-lockout.md): informatie over het configureren van de functie Smart Lockout op uw tenant om te beveiligen van gebruikersaccounts.
- [Technische details](active-directory-aadconnect-pass-through-authentication-how-it-works.md): informatie over de werking van de functie voor Pass through-verificatie.
- [Problemen oplossen](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): informatie over het oplossen van veelvoorkomende problemen met de functie voor Pass through-verificatie.
- [Grondig onderzoek van beveiliging](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): uitgebreide technische informatie over de functie voor Pass through-verificatie.
- [Azure AD naadloze eenmalige aanmelding](active-directory-aadconnect-sso.md): meer informatie over deze aanvullende functie.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): de Azure Active Directory-Forum gebruiken om nieuwe functieaanvragen.

