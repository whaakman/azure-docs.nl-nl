---
title: Delen van accounts met behulp van Azure AD | Microsoft Docs
description: Hierin wordt beschreven hoe Azure Active Directory kunnen organisaties veilig delen van accounts voor on-premises toepassingen en consumentencloudservices.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 10/29/2018
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 58b91388ec81726bd42e57fbf2b83d3d9f368833
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244022"
---
# <a name="sharing-accounts-with-azure-ad"></a>Accounts delen met Azure AD
## <a name="overview"></a>Overzicht
Soms moeten organisaties een één gebruikersnaam en wachtwoord gebruiken voor meerdere gebruikers, wat doorgaans in twee gevallen gebeurt:

* Bij het openen van toepassingen waarvoor een unieke aanmeldingsnaam en wachtwoord voor elke gebruiker, of on-premises toepassingen of de consument cloud services (bijvoorbeeld, zakelijke sociale media-accounts).
* Bij het maken van omgevingen met meerdere gebruikers. Mogelijk hebt u een enkele, lokale account met verhoogde bevoegdheden en wordt gebruikt om u te core-installatie, beheer en herstel activiteiten. Bijvoorbeeld, account de lokale 'globale beheerder' voor Office 365 of het root-account in Salesforce.

Traditioneel worden deze accounts gedeeld door de referenties (gebruikersnaam en wachtwoord) distribueren naar de juiste personen of opslaat in een gedeelde locatie is waar meerdere vertrouwde agents toegankelijk.

Het traditionele model voor delen heeft enkele nadelen:

* Toegang tot nieuwe toepassingen inschakelen, moet u referenties voor iedereen die toegang nodig heeft wilt distribueren.
* Elke gedeelde toepassing mogelijk een eigen unieke set van gedeelde referenties, dat gebruikers hoeven te onthouden meerdere sets met referenties. Wanneer gebruikers hoeft te veel referenties onthouden, verhoogt het risico dat ze gebruik te maken van riskante procedures. (bijvoorbeeld, schrijven wachtwoorden).
* U kunt geen zien wie toegang heeft tot een toepassing.
* U kunt geen zien wie heeft *geopend* een toepassing.
* Wanneer u toegang tot een toepassing verwijderen wilt, hebt u de referenties bijwerken en deze opnieuw distribueren naar iedereen die toegang tot deze toepassing nodig heeft.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory-account delen
Azure AD biedt een nieuwe benadering voor het gebruik van gedeelde accounts waarmee deze nadelen worden opgelost.

De Azure AD-beheerder configureert welke toepassingen die een gebruiker toegang heeft tot met behulp van het toegangsvenster en kiezen van het type van eenmalige aanmelding beste geschikt is voor de toepassing. Een van deze typen *wachtwoord gebaseerde eenmalige aanmelding*, kunt u Azure AD die fungeren als een soort 'broker' tijdens de aanmelding voor die app.

Gebruikers melden zich in één keer met hun organisatie-account. Dit account is hetzelfde account dat ze regelmatig gebruikt voor toegang tot hun bureaublad of e-mailbericht. Ze kunnen detecteren en alleen de toepassingen die ze zijn toegewezen. Deze lijst met toepassingen kan gedeelde accounts een willekeurig aantal gedeelde referenties bevatten. De eindgebruiker hoeft niet te onthouden of schrijf deze op de diverse accounts dat kunnen worden gebruikt.

Gedeelde accounts niet alleen toezicht verhogen en de bruikbaarheid verbeteren, worden ze ook de beveiliging verbeteren. Gebruikers met machtigingen om de referenties te gebruiken het gedeelde wachtwoord niet bekijken, maar in plaats van machtigingen voor het gebruik van het wachtwoord als onderdeel van een stroom gecoördineerd verificatie. Bovendien sommige toepassingen van de SSO-wachtwoord u de mogelijkheid bieden van het gebruik van Azure AD om periodiek rollover (update) wachtwoorden. Het systeem maakt gebruik van grote, complexe wachtwoorden, die verhoogt de beveiliging van accounts. De beheerder eenvoudig kunt verlenen of intrekken van toegang tot een toepassing weet wie toegang heeft tot het account en wie toegang heeft gekregen tot het in het verleden.

Azure AD biedt ondersteuning voor gedeelde accounts voor Enterprise Mobility Suite (EMS), Premium of Basic-gebruikers, met een licentie voor alle typen één wachtwoord aanmeldings-toepassingen. U kunt accounts voor het gebruik van duizenden vooraf geïntegreerde toepassingen in de toepassingsgalerie delen en uw eigen toepassing verifiëren met een wachtwoord met toevoegen [aangepaste apps voor eenmalige aanmelding](../manage-apps/configure-single-sign-on-portal.md).

Azure AD-functies waarmee het delen van account zijn onder andere:

* [Wachtwoord eenmalige aanmelding](../manage-apps/what-is-single-sign-on.md#password-based-single-sign-on)
* Afzonderlijke wachtwoord aanmeldings-agent
* [Toewijzing van updategroep](groups-self-service-management.md)
* Aangepaste wachtwoord apps
* [App-dashboard/gebruiksrapporten](../active-directory-passwords-get-insights.md)
* Eindgebruikers-portals voor toegang
* [App-proxy](../manage-apps/application-proxy.md)
* [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Een account delen
Voor het gebruik van Azure AD voor het delen van een account, moet u naar:

* Een toepassing toevoegen [app-galerie](https://azure.microsoft.com/marketplace/active-directory/) of [aangepaste toepassing](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-now-in-preview/)
* De toepassing voor wachtwoord eenmalige aanmelding (SSO) configureren
* Gebruik [toewijzing op basis van een groep](groups-saasapps.md) en selecteer de optie voor het invoeren van een gedeelde referenties
* Optioneel: in sommige toepassingen, zoals Facebook, Twitter en LinkedIn, kunt u de optie inschakelen voor [Azure AD wachtwoord meegenomen geautomatiseerd](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/)

U kunt ook uw gedeelde account beter beveiligen met multi-factor Authentication (MFA) (meer informatie over [met Azure AD-toepassingen beveiligen](../authentication/concept-mfa-whichversion.md)) en kunt u de mogelijkheid om te beheren wie toegang heeft tot de toepassing met behulp van delegeren[ Azure AD-selfservice](groups-self-service-management.md) groepsbeheer.

## <a name="related-articles"></a>Verwante artikelen:
* [Application Management in Azure Active Directory](../manage-apps/what-is-application-management.md) (Toepassingsbeheer in Azure Active Directory)
* [Beveiligen van apps met voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md)
* [Groepsbeheer met Self-Service management/SSAA](groups-self-service-management.md)
