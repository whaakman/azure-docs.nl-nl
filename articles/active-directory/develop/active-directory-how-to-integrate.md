---
title: Het integreren met Azure Active Directory | Microsoft Docs
description: Een handleiding voor de voordelen van en resources voor de integratie met Azure Active Directory.
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mtillman
editor: 
ms.assetid: d13bba54-96bd-4b81-bee9-c8025ffa1648
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: bd9f984a625ae951a16eb729c0a4565eea05e46b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="integrating-with-azure-active-directory"></a>Integreren met Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory biedt organisaties met bedrijfsniveau Identiteitsbeheer voor cloud-toepassingen.  Azure AD-integratie geeft uw gebruikers een gestroomlijnde ervaring aanmelden en helpt u uw toepassing in overeenstemming zijn met de IT-beleid.

## <a name="how-to-integrate"></a>Het integreren van
Er zijn verschillende manieren om uw toepassing uit te integreren met Azure AD.  Profiteren van veel of enkele van deze scenario's zoals geschikt is voor uw toepassing.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Ondersteuning voor Azure AD als een manier om aanmelden bij uw toepassing
**Aanmelden wrijving verminderen en de ondersteuningskosten reduceren.** Uw gebruikers geen met Azure AD aan te melden bij uw toepassing, één meer naam en wachtwoord te onthouden.  Als een ontwikkelaar hebt u één minder wachtwoord op te slaan en te beveiligen.  Omdat niet hoeft te handelen vergeten wachtwoorden, kan een aanzienlijke besparing alleen zijn.  Azure AD drijft aanmelden voor een aantal van de hele wereld meest populaire cloudtoepassingen, met inbegrip van Office 365 en Microsoft Azure.  Met honderden miljoenen gebruikers van miljoenen organisaties kans op zijn voor uw gebruiker is al aangemeld bij Azure AD.  Meer informatie over [ondersteuning voor Azure AD aanmelding toe te voegen](active-directory-authentication-scenarios.md).

**Aanmelding van vereenvoudigen voor uw toepassing.**  Tijdens de aanmelding voor uw toepassing, kan Azure AD essentiële informatie over een gebruiker verzenden zodat u kunt vooraf vul je aanmelding formulier of volledig elimineren.  Gebruikers kunnen zich registreren voor uw toepassing met behulp van hun Azure AD-account via een bekend toestemming ervaring vergelijkbaar met die gevonden in sociale media en mobiele toepassingen.  Elke gebruiker kan registreren en aanmelden bij een toepassing die is geïntegreerd met Azure AD zonder tussenkomst van IT.  Meer informatie over [ondertekening van uw toepassing voor Azure AD-Account aanmelding](../../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Zoeken naar gebruikers, beheren gebruikers inrichten en beheren van toegang tot uw toepassing
**Blader naar gebruikers in de directory.**  De Graph API gebruiken om gebruikers te zoeken en bladeren naar andere personen in hun organisatie wanneer andere gebruikers uitnodigen of verlenen van toegang, in plaats van vereisen dat ze naar het type e-adressen.  Gebruikers kunnen bladeren met behulp van een bekende adres adresboek stijlinterface, inclusief de details van de organisatie-hiërarchie weergeven.  Meer informatie over de [Graph API](active-directory-graph-api.md).

**Active Directory-groepen en distributielijsten die al wordt beheerd door de klant opnieuw gebruiken.**  Azure AD bevat de groepen die uw klant al wordt gebruikt voor e-maildistributie en toegang beheren.  Deze groepen in plaats van dat uw klant maken en beheren van een afzonderlijke set met groepen in uw toepassing met behulp van de Graph API, opnieuw worden gebruikt.  Informatie over de kan ook worden verzonden naar uw toepassing in aanmelden tokens.  Meer informatie over de [Graph API](active-directory-graph-api.md).

**Azure AD om te bepalen wie toegang tot uw toepassing heeft gebruikt.**  Beheerders en eigenaren van de toepassing in Azure AD kunnen access toewijzen aan specifieke gebruikers en groepen van toepassingen.  De Graph API gebruikt, kunt u deze lijst lezen en gebruiken om te bepalen de inrichting en de inrichting van resources en toegang in uw toepassing.

**Gebruik Azure AD voor rollen gebaseerd toegangsbeheer.**  Beheerders en eigenaren van de toepassing kunnen gebruikers en groepen toewijzen aan rollen die u definiëren wanneer u uw toepassing in Azure AD registreren.  Rol informatie wordt verzonden naar uw toepassing in tokens aanmelden en kan ook worden gelezen met de Graph API.  Meer informatie over [gebruikmaken van Azure AD voor autorisatie](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Toegang krijgen tot het gebruikersprofiel, agenda, E-mail, contactpersonen, bestanden en meer
**Azure AD is de server van de autorisatie voor Office 365 en andere zakelijke services van Microsoft.**  Als u Azure AD voor aanmelden bij uw toepassing of ondersteuning van uw huidige gebruikersaccounts te koppelen aan Azure AD-gebruikersaccounts met behulp van OAuth 2.0 ondersteunt, kunt u lees- en schrijftoegang tot het profiel van een gebruiker, agenda, e-mail, contactpersonen, bestanden en andere informatie vragen.  U kunt naadloos gebeurtenissen schrijven naar de agenda van gebruiker, en lezen of schrijven van bestanden naar hun OneDrive.  Meer informatie over [toegang tot de Office 365-API](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Uw toepassing in Azure en Office 365 marktplaatsen promoveren
**Uw toepassing om de miljoenen organisaties die al van Azure AD gebruikmaken te promoveren.**  Gebruikers die zoeken en deze marktplaatsen al gebruikmaakt van een of meer cloud-services, zodat ze cloud-serviceklanten gekwalificeerd.  Meer informatie over het wijzigen van uw toepassing in [Azure Marketplace](https://azure.microsoft.com/marketplace/partner-program/).

**Wanneer gebruikers zich registreren voor uw toepassing, wordt deze weergegeven in hun Azure AD-Toegangsvenster en Office 365-app starten.**  Gebruikers zich snel en eenvoudig terugkeren naar uw toepassing later gebruiker engagement verbeteren.  Meer informatie over de [Azure AD-Toegangsvenster](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Veilige communicatie voor apparaat-naar-Service en Service-naar-Service
**Met Azure AD voor identiteitsbeheer van services en apparaten de code hoeft vermindert te schrijven en kan IT om toegang te beheren.**  Services en apparaten kunnen tokens verkrijgen van Azure AD met OAuth en gebruiken van deze tokens voor toegang tot web-API's.  Met behulp van Azure AD kunt u voorkomen schrijven complexe verificatiecode op te geven.  Omdat de identiteit van de services en de apparaten worden opgeslagen in Azure AD, IT-sleutels en intrekken op één plek in plaats van hiervoor afzonderlijk in uw toepassing kunt beheren.

## <a name="benefits-of-integration"></a>Voordelen van de integratie
Integratie met Azure AD wordt geleverd met voordelen, hoeft u aanvullende code schrijven.

### <a name="integration-with-enterprise-identity-management"></a>Integratie met Enterprise-identiteitsbeheer
**Uw toepassing voldoen aan de IT-beleid helpen.**  Organisaties hun onderneming systemen voor identiteitsbeheer integreren met Azure AD, zodat wanneer een persoon een organisatie verlaat, zij automatisch toegang tot uw toepassing zonder verliezen zullen IT hoeven extra stappen uitvoeren.  IT kunt beheren die toegang tot uw toepassing en bepalen welke toegangsbeleid zijn vereist - voorbeeld multi-factor Authentication - hoeft te code schrijven om te voldoen aan het bedrijfsbeleid complexe verminderen.  Azure AD biedt beheerders een gedetailleerde controlelogboek van die aangemeld bij uw toepassing zodat IT bijhouden van gebruik.

**Azure AD wordt Active Directory uitgebreid naar de cloud, zodat uw toepassing kan worden geïntegreerd met AD.**  Veel organisaties overal ter wereld Active Directory gebruiken als hun principal aanmelden en identiteitsbeheersysteem en hun toepassingen werken met AD vereisen.  Uw app integreren met Azure AD worden geïntegreerd met Active Directory.

### <a name="advanced-security-features"></a>Geavanceerde beveiligingsfuncties
**Multi-factor authentication-server.**  Azure AD levert systeemeigen multi-factor authentication-server.  IT-beheerders kunnen multi-factor authentication toegang krijgen tot uw toepassing vereisen, zodat er geen code van deze ondersteuning zelf.  Meer informatie over [multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Afwijkende aanmelden detectie.**  Azure AD processen meer dan een miljard aanmeldingen per dag, tijdens het gebruik van machine learning-algoritmen voor het detecteren van verdachte activiteit en IT-beheerders van mogelijke problemen melden.  Uw toepassing opgehaald door de ondersteuning van Azure AD aanmelden, het voordeel van deze beveiligingsgroep. Meer informatie over [Azure Active Directory-rapport weer te geven](../active-directory-view-access-usage-reports.md).

**Voorwaardelijke toegang.**  Naast de multi-factor authentication-beheerders kunnen vereisen bepaalde voorwaarden worden voldaan voordat gebruikers kunnen aanmelden bij uw toepassing.  Voorwaarden die kunnen worden ingesteld omvatten het IP-adresbereik van clientapparaten, lidmaatschap van de opgegeven groepen en de status van het apparaat wordt gebruikt om toegang te krijgen.  Meer informatie over [voorwaardelijke toegang van Azure Active Directory](../active-directory-conditional-access-azure-portal.md).

### <a name="easy-development"></a>Eenvoudige manier kunt ontwikkelen
**Protocollen volgens de industrienorm.**  Microsoft hecht naar ondersteunende industrienormen.  Azure AD biedt ondersteuning voor de verificatieprotocollen SAML 2.0, OpenID Connect 1.0, OAuth 2.0 en WS-Federation 1.2.  De Graph API is OData 4.0-compatibel.  Als de toepassing al het SAML 2.0 of OpenID Connect 1.0-protocol voor federatieve aanmelding ondersteunt, zijn het toevoegen van ondersteuning voor Azure AD eenvoudige.  Meer informatie over [Azure AD-verificatieprotocollen ondersteund](active-directory-authentication-protocols.md).

**Open-source bibliotheken.**  Microsoft biedt een volledig ondersteunde open-source bibliotheken voor populaire talen en platformen ontwikkelen versnellen.  De broncode wordt in licentie gegeven onder Apache 2.0 en u kunt vertakken en bijdragen terug naar de projecten zijn.  Meer informatie over [Azure AD-verificatiebibliotheken](active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Wereldwijde aanwezigheid en hoge beschikbaarheid
**Azure AD wordt in datacenters over de hele wereld wordt geïmplementeerd en beheerd en bewaakt het servicepakket.**  Azure AD het beheersysteem identiteit voor Microsoft Azure en Office 365 en wordt geïmplementeerd in 28 datacenters over de hele wereld.  Active Directory-gegevens is worden gerepliceerd naar ten minste drie datacenters gegarandeerd.  Globale netwerktaakverdelers Zorg ervoor dat gebruikers toegang tot de dichtstbijzijnde kopie van Azure AD met hun gegevens en aanvragen voor andere datacenters automatisch opnieuw verzenden als er een probleem wordt gedetecteerd.

## <a name="next-steps"></a>Volgende stappen
[Schrijven van code aan de slag](active-directory-developers-guide.md#get-started).

[Gebruikers aanmelden met behulp van Azure AD](active-directory-authentication-scenarios.md)

