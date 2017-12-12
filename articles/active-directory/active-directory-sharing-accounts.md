---
title: Delen van accounts met behulp van Azure AD | Microsoft Docs
description: Hierin wordt beschreven hoe Azure Active Directory kunnen organisaties veilig delen accounts voor consumer-cloudservices en lokale apps.
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: e2d77104-d978-46a3-bfea-03ffdf3b61e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 3b6a83d91ec5d8466669655d6c3bd7ae7b42dd2f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="sharing-accounts-with-azure-ad"></a>Accounts delen met Azure AD
## <a name="overview"></a>Overzicht
Soms moeten organisaties een één gebruikersnaam en wachtwoord gebruiken voor meerdere mensen die doorgaans in beide gevallen wordt uitgevoerd:

* Bij het openen van toepassingen waarvoor een unieke aanmeldingsnaam en wachtwoord voor elke gebruiker of lokale apps of de consument cloudservices (bijvoorbeeld zakelijke sociale media-accounts).
* Bij het maken van omgeving met meerdere gebruikers. U wellicht een lokale account die verhoogde bevoegdheden en wordt gebruikt om u te core-installatie, beheer- en hersteltaken activiteiten. Bijvoorbeeld: de lokale 'globale beheerder' account voor Office 365 of het root-account in Salesforce.

Deze accounts worden traditioneel gedeeld door de referenties (gebruikersnaam en wachtwoord) distribueren naar de juiste personen of ze opslaan in een gedeelde locatie waar meerdere vertrouwde agents er toegang toe.

Het traditionele model delen, is enkele nadelen:

* Toegang tot de nieuwe toepassingen inschakelen, moet u referenties voor iedereen die toegang nodig heeft distribueren.
* Elke gedeelde toepassing moet mogelijk een eigen unieke set van gedeelde referenties, dat gebruikers hoeven te onthouden meerdere sets met referenties. Wanneer gebruikers hebben te veel referenties onthouden, verhoogt het risico dat ze gebruik te maken van riskante procedures. (bijvoorbeeld, schrijven wachtwoorden).
* U kunt niet controleren wie toegang heeft tot een toepassing.
* U kunt niet controleren wie heeft *toegankelijk* een toepassing.
* Wanneer u verwijderen van de toegang tot een toepassing wilt, hebt u de referenties bijwerken en deze opnieuw distribueren naar iedereen die toegang tot die toepassing nodig.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory-account delen
Azure AD levert een nieuwe benadering voor het gebruik van gedeelde accounts waarmee deze nadelen.

De Azure AD-beheerder configureert welke toepassingen die een gebruiker toegang heeft met het toegangsvenster en kiezen van het type van eenmalige aanmelding beste geschikt is voor die toepassing. Een van deze typen *op basis van wachtwoorden eenmalige aanmelding*, kunt u Azure AD fungeren als een soort 'broker' tijdens de aanmelding voor die app.

Gebruikers melden zich in één keer aan hun organisatieaccount is gekoppeld. Dit account is hetzelfde account dat ze regelmatig gebruiken voor toegang tot het bureaublad of in e-mailbericht. Ze kunnen detecteren en toegang tot alleen de toepassingen die ze worden toegewezen aan. Deze lijst van toepassingen kan een onbeperkt aantal gedeelde referenties bevatten met gedeelde accounts. De eindgebruiker hoeft niet te onthouden of schrijf deze op de diverse accounts die kunnen uitvoeren.

Gedeelde accounts niet alleen toezicht verhogen en de bruikbaarheid verbeteren, ze ook de beveiliging verbeteren. Gebruikers met machtigingen voor de referenties gebruiken het gedeelde wachtwoord niet ziet, maar in plaats daarvan machtigingen het wachtwoord gebruiken als onderdeel van een geregiseerde authenticatiestroom ophalen. Bovendien bieden sommige toepassingen van de SSO-wachtwoord u Azure AD om periodiek rollover (update) wachtwoorden te gebruiken. Het systeem gebruikt grote, complexe wachtwoorden, die accountbeveiliging wordt verbeterd. De beheerder kan eenvoudig toegang verlenen of intrekken voor een toepassing weet wie toegang heeft tot het account en wie het heeft in het verleden heeft geopend.

Azure AD ondersteunt gedeelde accounts voor Enterprise Mobility Suite (EMS), Premium of Basic gelicentieerde gebruikers, alle typen van één wachtwoord aanmelding toepassingen. U kunt accounts voor een van de duizenden vooraf geïntegreerde toepassingen in de galerie met toepassingen delen en uw eigen toepassing wachtwoord verifiëren met toevoegen [aangepaste SSO apps](active-directory-enterprise-apps-manage-sso.md).

Azure AD-functies die delen van het account inschakelen:

* [Eenmalige aanmelding wachtwoord](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Één wachtwoord aanmelding agent
* [Toewijzing van updategroep](active-directory-accessmanagement-self-service-group-management.md)
* Aangepast wachtwoord apps
* [App-dashboard/gebruiksrapporten](active-directory-passwords-get-insights.md)
* Eindgebruikers-portals voor toegang
* [App-proxy](active-directory-application-proxy-get-started.md)
* [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Delen van een account
Als u Azure AD wilt delen van een account, moet u:

* Een toepassing toevoegen [app-galerie](https://azure.microsoft.com/marketplace/active-directory/) of [aangepaste toepassing](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
* De toepassing voor eenmalige aanmelding (SSO) wachtwoord configureren
* Gebruik [toewijzing op basis van een groep](active-directory-accessmanagement-group-saasapps.md) en selecteer de optie voor het invoeren van een gedeelde referentie
* Optioneel: in sommige toepassingen, zoals Facebook, Twitter en LinkedIn, kunt u de optie inschakelen voor [Azure AD geautomatiseerde wachtwoord roll-over](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

U kunt ook uw gedeelde account beter beveiligen met multi-factor Authentication (MFA) (meer informatie over [beveiliging van toepassingen met Azure AD](../multi-factor-authentication/multi-factor-authentication-get-started.md)) en kunt u de mogelijkheid om te beheren wie toegang heeft tot de toepassing die gebruikmaakt van delegeren[ Azure AD-Self-service](active-directory-accessmanagement-self-service-group-management.md) groepsbeheer.

## <a name="related-articles"></a>Verwante artikelen:
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Apps met voorwaardelijke toegang beveiligen](active-directory-conditional-access-azure-portal.md)
* [Groepsbeheer met Self-Service management/SSAA](active-directory-accessmanagement-self-service-group-management.md)

