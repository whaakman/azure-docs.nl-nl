---
title: Wat zijn de grondbeginselen van Azure identiteits- en toegangsbeheer management? -Azure Active Directory | Microsoft Docs
description: Meer informatie over de geavanceerde beveiliging-mogelijkheden en aanvullende hulpprogramma's die beschikbaar met Azure Active Directory Premium-edities zijn.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/13/2018
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: f7baa29c77ae4af9813bfc755a39cc07288a3ad2
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45734669"
---
# <a name="what-are-the-fundamentals-of-azure-identity-and-access-management"></a>Wat zijn de grondbeginselen van Azure identiteits- en toegangsbeheer management?
Azure AD Premium is een cloud-gebaseerde identiteits- en toegangsbeheer beheeroplossing, met geavanceerde beveiligingsfuncties. Deze geavanceerde mogelijkheden te bieden een veilige identiteit voor al uw apps, identiteitsbeveiliging (uitgebreid met de [beveiligingsgrafiek van Microsoft intelligence](https://www.microsoft.com/security/intelligence)), en [Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md). Azure AD helpt bij het beveiligen van uw gebruikers-id's in realtime, helpen u bij het maken van de risico's gebaseerde en adaptieve toegangsbeleid om gegevens van uw organisatie.

Bekijk deze korte video voor een snel overzicht van Azure AD-identiteitsbeheer en -beveiliging:
>[!VIDEO https://www.youtube.com/embed/9LGIJ2-FKIM]

Azure AD biedt ook een set hulpprogramma's waarmee u de beveiliging, automatiseren en beheren van uw omgeving, inclusief het opnieuw instellen van wachtwoorden, beheer van gebruikers en groepen en app-aanvragen. Azure AD kan ook helpen u bij het beheren van apparaten van gebruikers en toegang tot en beheer van Software als een Service (SaaS)-apps.

Zie voor meer informatie over de kosten van Azure Active Directory Premium-edities en de bijbehorende hulpprogramma's, [prijzen van Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>On-premises Active Directory verbinden met Azure AD en Office 365
Uw on-premises Active Directory-implementatie naar de cloud uitbreiden door uw on-premises directory's integreren met Azure AD via [hybride-identiteitsbeheer](https://aka.ms/aadframework). [Azure AD Connect](../connect/active-directory-aadconnect.md) biedt deze integratie kunt en uw gebruikers één identiteit en eenmalige aanmelding (SSO) toegang tot uw on-premises resources en cloudservices, zoals Office 365.

Azure AD Connect vervangt oudere versies van hulpprogramma's voor identiteitsintegratie, zoals DirSync en Azure AD Sync, helpt bij de ondersteuning van de behoeften van uw identiteit synchronisatie tussen on-premises en Azure AD. Azure AD Connect-synchronisatie wordt ingeschakeld via:

- **De synchronisatie.** Verantwoordelijk voor het maken van gebruikers, groepen en andere objecten. Het is ook verantwoordelijk voor te zorgen dat de gegevens van de identiteit voor uw on-premises gebruikers komt overeen met wat in Azure AD. Wachtwoord terugschrijven inschakelen helpt ook bij uw on-premises directory's gesynchroniseerd houden wanneer gebruikers bijwerken van wachtwoorden in Azure AD.

- -Verificatie. Het is belangrijk om de juiste verificatiemethode kiezen bij het instellen van uw Azure AD-oplossing voor hybride identiteit. U kunt cloud-verificatie (wachtwoord-Hashsynchronisatie / Pass through-verificatie) of federatieve verificatie (AD FS) voor uw organisatie. Zie voor meer informatie over de beschikbare opties [kiest u de juiste verificatiemethode voor uw Azure Active Directory-oplossing voor hybride identiteit](https://aka.ms/auth-options).

- **Statuscontrole.** Azure AD Connect Health biedt bewaking en een centrale locatie in Azure portal om deze activiteit weer te geven. Zie [Monitor your on-premises identity infrastructure and synchronization services in the cloud](../connect-health/active-directory-aadconnect-health.md) (Uw on-premises infrastructuur voor identiteiten en synchronisatieservices in de cloud controleren) voor meer informatie.

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>De productiviteit verhogen en de helpdesk-kosten beperken met behulp van selfservice en eenmalige aanmelding
Gebruikers bespaart tijd wanneer ze een één gebruikersnaam en wachtwoord, samen met een consistente ervaring op elk apparaat hebben. Gebruikers ook de tijd besparen door u zelf wilt uitvoeren, zoals[opnieuw instellen van een vergeten wachtwoord](../user-help/active-directory-passwords-update-your-own-password.md) of het aanvragen van toegang tot een toepassing zonder te wachten tot de helpdesk om hulp vragen.

Bevordering van de eenmalige aanmelding en een consistente ervaring, Azure AD [uitbreiding van uw on-premises Active Directory](../connect/active-directory-aadconnect.md) naar de cloud, zodat uw gebruikers hun primaire organisatie-account gebruiken voor hun apparaten domein bedrijfsresources, en alle de web- en SaaS-toepassingen die ze nodig hebben om te gebruiken voor een werk gedaan te krijgen. 

Bovendien toegang tot de toepassing kan worden automatisch ingericht (of niet ingericht) op basis van lidmaatschap van groepen en een werknemer de status van gebruiker, zodat u toegang tot het galerie-apps of uw eigen on-premises toepassingen die u hebt ontwikkeld en gepubliceerd via beheren de [Azure AD-toepassingsproxy](../manage-apps/application-proxy.md).

## <a name="manage-and-control-access-to-your-organizational-resources"></a>Beheren en toegang tot uw resources van de organisatie beheren
Microsoft oplossingen voor identiteits- en helpt u bij het beveiligen van toegang tot apps en resources in uw organisatie datacenter en in de cloud. Dit toegangsbeheer biedt extra niveaus van de validatie van zoals [multi-Factor Authentication](../authentication/concept-mfa-howitworks.md) en [beleid voor voorwaardelijke toegang](../conditional-access/overview.md). Bewaking van verdachte activiteiten via geavanceerde rapportage van beveiliging, controle en waarschuwingen kan ook bijdragen aan mogelijke beveiligingsproblemen te beperken.

Met behulp van beleid voor voorwaardelijke toegang in Azure AD Premium kunt u regels voor toegang op basis van beleid voor alle Azure AD verbonden Apps, zoals SaaS-apps, aangepaste apps die worden uitgevoerd in de cloud of on-premises of web-apps maken). Azure AD evalueert de regels in realtime en deze af te dwingen wanneer een gebruiker probeert te krijgen tot een app. Azure-identiteit beveiligingsbeleid kunt u automatisch actie ondernemen (door de toegang blokkeert, meervoudige verificatie afdwingen of opnieuw instellen van wachtwoorden van gebruikers) als verdachte activiteit wordt gedetecteerd.

## <a name="azure-active-directory-privileged-identity-management"></a>Azure Active Directory Privileged Identity Management
[Privileged Identity Management (PIM)](../privileged-identity-management/pim-getting-started.md), opgenomen in de Azure Active Directory Premium 2-editie, helpt u bij het detecteren, beperken en controleren van administratieve accounts en hun toegang tot bronnen in uw Azure Active Directory en andere Microsoft online services. PIM helpt u ook de beheerderstoegang op aanvraag voor de exacte periode die u nodig hebt, wat betekent dat u kunt toestaan dat beheerders om aan te vragen op basis van meerdere factoren is geverifieerd, tijdelijke verhoging van de bevoegdheden voor een vooraf geconfigureerde periode beheren voordat hun accounts terug naar de status van een normale gebruiker.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de Azure AD-architectuur, [wat is de Azure AD-architectuur?](active-directory-architecture.md).
