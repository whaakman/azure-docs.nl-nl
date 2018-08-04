---
title: Uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie | Microsoft Docs
description: Meer informatie over het weergeven van een toepassing die ondersteuning biedt voor eenmalige aanmelding in de app-galerie van Azure Active Directory
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/14/2018
ms.author: celested
ms.reviewer: elisol, bryanla
ms.custom: aaddev
ms.openlocfilehash: c84be8985b50be4a9339af6aa9e308341662b291
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504563"
---
# <a name="list-your-application-in-the-azure-active-directory-application-gallery"></a>Uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie


##  <a name="what-is-the-azure-ad-application-gallery"></a>Wat is de Azure AD-toepassingsgalerie?

Azure Active Directory (Azure AD) is een identiteitsservice in de cloud gebaseerde. De [Azure AD-toepassingsgalerie](https://azure.microsoft.com/marketplace/active-directory/all/) is in de Azure Marketplace appstore, waar alle toepassingsconnectoren voor eenmalige aanmelding en inrichten van gebruikers worden gepubliceerd. Klanten die gebruikmaken van Azure AD als id-provider vinden de verschillende SaaS-toepassing connectors gepubliceerd hier. IT-beheerders connectors toevoegen vanuit de app-galerie, en vervolgens configureren en gebruiken van de connectors voor eenmalige aanmelding en de inrichting. Azure AD ondersteunt alle belangrijke federation-protocollen voor eenmalige aanmelding, met inbegrip van SAML 2.0, OpenID Connect, OAuth en WS-Federation.

## <a name="what-are-the-benefits-of-listing-an-application-in-the-gallery"></a>Wat zijn de voordelen van de aanbieding van een toepassing in de galerie?

*  Klanten vinden de best mogelijke eenmalige aanmelding ervaring.

*  Configuratie van de toepassing is eenvoudig en minimale.

*  Uw toepassing zoekt een snelle zoekactie in de galerie.

*  Gratis, Basic en Premium, Azure AD-klanten kunnen worden gebruikt door deze integratie.

*  Wederzijdse klanten krijgen een stapsgewijze configuratie-zelfstudie.

*  Klanten die SCIM gebruiken kunt voor dezelfde app wordt ingericht.

##  <a name="prerequisites-implement-federation-protocol"></a>Vereisten: Implementeer federation-protocol

Als u een toepassing in de galerie met Azure AD-app, moet u eerst een van de volgende federation protocollen die worden ondersteund door Azure AD te implementeren en akkoord gaat met Azure AD-toepassing galerie voorwaarden en bepalingen. Lees de voorwaarden en bepalingen van de Azure AD-toepassingsgalerie van [hier](https://azure.microsoft.com/en-us/support/legal/active-directory-app-gallery-terms/).

*   **OpenID Connect**: de multitenant-toepassing in Azure AD maken en implementeren van de [Azure AD-toestemmingsframework](quickstart-v1-integrate-apps-with-azure-ad.md#overview-of-the-consent-framework) voor uw toepassing. De aanmeldingsaanvraag naar een gemeenschappelijk eindpunt verzenden, zodat elke klant u toestemming voor de toepassing krijgt. U kunt toegang van gebruikers op basis van de tenant-ID en de UPN van de gebruiker in het token ontvangen. Als u wilt uw toepassing integreren met Azure AD, gaat u als volgt de [ontwikkelaars instructies](authentication-scenarios.md).

    ![Tijdlijn van de OpenID Connect-toepassing in de galerie vermelden](./media/active-directory-app-gallery-listing/openid.png)

    * Als u wilt uw toepassing toevoegen aan lijst in de galerie met OpenID Connect, selecteer **OpenID Connect en OAuth 2.0** zoals hierboven.

    * Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

*   **SAML 2.0** of **WS-Federation**: uw toepassing moet beschikken over de mogelijkheid de SAML/WS-Federation SSO integreren in de modus SP of id-provider. Als uw app SAML 2.0 ondersteunt, u kunt deze rechtstreeks integreren met een Azure AD-tenant met behulp van de [instructies voor het toevoegen van een aangepaste toepassing](../active-directory-saas-custom-apps.md).

    ![Tijdlijn van SAML 2.0 of WS-Federation-toepassing in de galerie vermelden](./media/active-directory-app-gallery-listing/saml.png)

    * Als u wilt dat uw toepassing toevoegen aan lijst in de galerie met **SAML 2.0** of **WS-Federation**, selecteer **SAMl 2.0/WS-Federation** zoals hierboven.

    * Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

*   **SSO-wachtwoord**: Maak een webtoepassing met een HTML-aanmeldingspagina configureren [wachtwoord gebaseerde eenmalige aanmelding](../manage-apps/what-is-single-sign-on.md). Wachtwoord gebaseerde SSO, ook wel aangeduid als wachtwoord vaulting, kunt u voor het beheren van toegang voor gebruikers en wachtwoorden voor webtoepassingen die bieden geen ondersteuning voor identiteitsfederatie. Het is ook handig voor scenario's waarin meerdere gebruikers één account moeten, zoals om accounts voor sociale media-app van uw organisatie te delen.

    ![Tijdlijn van de aanbieding wachtwoord SSO-toepassing in de galerie](./media/active-directory-app-gallery-listing/passwordsso.png)

    * Als u wilt uw toepassing toevoegen aan lijst in de galerie met behulp van wachtwoord SSO, selecteer **wachtwoord SSO** zoals hierboven.

    * Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

##  <a name="updateremove-existing-listing"></a>Bestaande aanbieding bijwerken/verwijderen

Als u wilt bijwerken of verwijderen van een bestaande toepassing in de galerie van Azure AD-app, moet u eerst dien de aanvraag in de [Portal voor App-netwerk](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Als u een Office 365-account hebt, kunt u die zich aanmeldt bij deze portal gebruiken. Als dat niet het geval is, moet u uw Microsoft-account (zoals Outlook of Hotmail) gebruikt om te melden.

* Selecteer relevante optie uit de onderstaande afbeelding

    ![Tijdlijn van saml-toepassing in de galerie vermelden](./media/active-directory-app-gallery-listing/updateorremove.png)
    
    * Als u een bestaande toepassing bijwerken wilt, selecteert u **bijwerken van bestaande toepassing aanbieding**.

    * Als u verwijderen van een bestaande toepassing in de Azure AD-galerie wilt, selecteert u **bestaande toepassing aanbieding verwijderen**

    * Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

## <a name="submit-the-request-in-the-portal"></a>Dien de aanvraag in de portal

Nadat u hebt getest of de toepassingsintegratie van uw met Azure AD werkt, uw aanvraag indienen voor toegang op onze [Portal voor App-netwerk](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Als u een Office 365-account hebt, kunt u die zich aanmeldt bij deze portal gebruiken. Als dat niet het geval is, moet u uw Microsoft-account (zoals Outlook of Hotmail) gebruikt om te melden.

Nadat u zich hebt aangemeld, wordt de volgende pagina wordt weergegeven. Een zakelijke reden voor toegang in het tekstvak te hoeven opgeven en selecteer vervolgens **verzoeken tot toegang**. Ons team controleert de details en hebt die u toegang dienovereenkomstig. Hierna kunt u zich aanmeldt bij de portal en uw gedetailleerde indienen voor de toepassing.

Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![Aanvraag voor toegang tot op SharePoint-portal](./media/active-directory-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Tijdlijnen
    
De tijdlijn voor het proces van de aanbieding van een SAML 2.0 of WS-Federation-toepassing in de galerie is 7-10 werkdagen.

   ![Tijdlijn van saml-toepassing in de galerie vermelden](./media/active-directory-app-gallery-listing/timeline.png)

De tijdlijn voor het proces van de aanbieding van een toepassing OpenID Connect in de galerie is 2 tot 5 dagen terugbetaald.

   ![Tijdlijn van saml-toepassing in de galerie vermelden](./media/active-directory-app-gallery-listing/timeline2.png)

De tijdlijn voor het proces van de aanbieding van de toepassing in de galerie met ondersteuning voor gebruikersinrichting is 40-45 dagen.

   ![Tijdlijn van saml-toepassing in de galerie vermelden](./media/active-directory-app-gallery-listing/provisioningtimeline.png)

## <a name="escalations"></a>Escalaties

Voor elke escalaties e-mailbericht verzendt de [Team van Azure AD-eenmalige aanmelding integratie](mailto:SaaSApplicationIntegrations@service.microsoft.com) die SaaSApplicationIntegrations@service.microsoft.com en ontvangt zo snel mogelijk een antwoord.