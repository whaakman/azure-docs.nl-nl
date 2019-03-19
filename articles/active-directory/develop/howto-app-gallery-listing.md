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
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2019
ms.author: celested
ms.reviewer: elisol, bryanla
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4cda09a6b407621e595b0cb8ed9103b1fbbd5cc5
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58097954"
---
# <a name="how-to-list-your-application-in-the-azure-active-directory-application-gallery"></a>Procedure: Uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie

## <a name="what-is-the-azure-ad-application-gallery"></a>Wat is de Azure AD-toepassingsgalerie?

- Klanten vinden de best mogelijke eenmalige aanmelding ervaring.
- Configuratie van de toepassing is eenvoudig en minimale.
- Uw toepassing zoekt een snelle zoekactie in de galerie.
- Gratis, Basic en Premium, Azure AD-klanten kunnen worden gebruikt door deze integratie.
- Wederzijdse klanten krijgen een stapsgewijze configuratie-zelfstudie.
- Klanten die SCIM gebruiken kunt voor dezelfde app wordt ingericht.

## <a name="prerequisites"></a>Vereisten

- Voor federatieve toepassingen (Open-ID en SAML/WS-Federation), moet de toepassing ondersteunt de SaaS-model voor het ophalen van die worden vermeld in de galerie van Azure AD. De galerie bedrijfstoepassingen moeten configuraties met meerdere klanten en niet voor elke klant met een specifieke ondersteunen.

- Voor de Open ID Connect meerdere tenants van de toepassing moet en [Azure AD-toestemmingsframework](consent-framework.md) moet de juiste wijze worden geïmplementeerd voor de toepassing. De gebruiker kan de aanmeldingsaanvraag verzenden naar een gemeenschappelijk eindpunt zodat elke klant u toestemming voor de toepassing krijgt. U kunt toegang van gebruikers op basis van de tenant-ID en de UPN van de gebruiker in het token ontvangen.

- Voor SAML 2.0/WS-Federation moet uw toepassing de mogelijkheid de SAML/WS-Federation SSO integreren in de modus SP of id-provider. Zorg ervoor dat deze correct werkt voordat de aanvraag is ingediend.

- SSO-wachtwoord, zorg ervoor dat uw toepassing ondersteuning biedt voor formulierverificatie zodat wachtwoordkluis kan worden gedaan om op te halen van eenmalige aanmelding werken zoals verwacht.

- Voor automatische inrichting van gebruikers aanvragen voor moet toepassing worden vermeld in de galerie met eenmalige aanmelding ingeschakeld met behulp van SAML 2.0/WS-Federation. U kunt aanvragen voor eenmalige aanmelding en gebruiker samen in de portal wordt ingericht als deze nog niet wordt weergegeven.

>[!NOTE]
>We worden uitgevoerd met een groot aantal aanvragen van de connector SCIM, zodat we het maken van nieuwe aanvragen op onze portal hebt gestopt. Wacht op uw verzoeken tot nadere kennisgeving. We zeer voor deze vertraging en enig ongemak die hebt ondervonden.

## <a name="submit-the-request-in-the-portal"></a>Dien de aanvraag in de portal

Nadat u hebt getest of de toepassingsintegratie van uw met Azure AD werkt, uw aanvraag indienen voor toegang op onze [Portal voor App-netwerk](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Als u een Office 365-account hebt, kunt u die zich aanmeldt bij deze portal gebruiken. Als dat niet het geval is, moet u uw Microsoft-account (zoals Outlook of Hotmail) gebruikt om te melden.

Als de volgende pagina wordt weergegeven na het aanmelden, neem dan contact op met de [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) en geef het e-mailaccount dat u gebruiken wilt voor het indienen van de aanvraag. Azure AD-team wordt vervolgens het account toegevoegd in de Portal voor Microsoft-toepassing netwerk.

![Aanvraag voor toegang tot op SharePoint-portal](./media/howto-app-gallery-listing/errorimage.png)

Nadat het account is toegevoegd, kunt u zich aanmelden bij de Portal voor Microsoft App-netwerk.

Als de volgende pagina wordt weergegeven na het aanmelden, een zakelijke reden voor toegang in het tekstvak te hoeven opgeven en selecteer vervolgens **verzoeken tot toegang**.

  ![Aanvraag voor toegang tot op SharePoint-portal](./media/howto-app-gallery-listing/accessrequest.png)

Ons team controleert de details en hebt die u toegang dienovereenkomstig. Zodra uw aanvraag is goedgekeurd, kunt u zich aanmeldt bij de portal en dien de aanvraag door te klikken op de **indienen aanvraag (ISV)** tegel formulier de startpagina.

![Startpagina van de SharePoint-portal](./media/howto-app-gallery-listing/homepage.png)

> [!NOTE]
> Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="implementing-sso-using-federation-protocol"></a>Eenmalige aanmelding met behulp van de federation-protocol implementeren

Als u een toepassing in de galerie met Azure AD-app, moet u eerst een van de volgende federation protocollen die worden ondersteund door Azure AD te implementeren en akkoord gaat met Azure AD-toepassing galerie voorwaarden en bepalingen. Lees de voorwaarden en bepalingen van de Azure AD-toepassingsgalerie van [hier](https://azure.microsoft.com/support/legal/active-directory-app-gallery-terms/).

- **OpenID Connect**: Als u wilt uw toepassing integreren met Azure AD met behulp van het Open ID Connect-protocol, volgt u de [ontwikkelaars instructies](authentication-scenarios.md).

    ![Tijdlijn van de OpenID Connect-toepassing in de galerie vermelden](./media/howto-app-gallery-listing/openid.png)

    * Als u wilt uw toepassing toevoegen aan lijst in de galerie met OpenID Connect, selecteer **OpenID Connect en OAuth 2.0** zoals hierboven.
    * Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

- **SAML 2.0** of **WS-Federation**: Als uw app SAML 2.0 ondersteunt, u kunt deze rechtstreeks integreren met een Azure AD-tenant met behulp van de [instructies voor het toevoegen van een aangepaste toepassing](../active-directory-saas-custom-apps.md).

  ![Tijdlijn van SAML 2.0 of WS-Federation-toepassing in de galerie vermelden](./media/howto-app-gallery-listing/saml.png)

  * Als u wilt dat uw toepassing toevoegen aan lijst in de galerie met **SAML 2.0** of **WS-Federation**, selecteer **SAMl 2.0/WS-Federation** zoals hierboven.
  * Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="implementing-sso-using-password-sso"></a>Eenmalige aanmelding met wachtwoord eenmalige aanmelding implementeren

Maak een webtoepassing met een HTML-aanmeldingspagina configureren [wachtwoord gebaseerde eenmalige aanmelding](../manage-apps/what-is-single-sign-on.md). Wachtwoord gebaseerde SSO, ook wel aangeduid als wachtwoord vaulting, kunt u voor het beheren van toegang voor gebruikers en wachtwoorden voor webtoepassingen die bieden geen ondersteuning voor identiteitsfederatie. Het is ook handig voor scenario's waarin meerdere gebruikers één account moeten, zoals om accounts voor sociale media-app van uw organisatie te delen.

![Tijdlijn van de aanbieding wachtwoord SSO-toepassing in de galerie](./media/howto-app-gallery-listing/passwordsso.png)

* Als u wilt uw toepassing toevoegen aan lijst in de galerie met behulp van wachtwoord SSO, selecteer **wachtwoord SSO** zoals hierboven.
* Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="updateremove-existing-listing"></a>Bestaande aanbieding bijwerken/verwijderen

Als u wilt bijwerken of verwijderen van een bestaande toepassing in de galerie van Azure AD-app, moet u eerst dien de aanvraag in de [Portal voor App-netwerk](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Als u een Office 365-account hebt, kunt u die zich aanmeldt bij deze portal gebruiken. Als dat niet het geval is, moet u uw Microsoft-account (zoals Outlook of Hotmail) gebruikt om te melden.

- Selecteer de gewenste optie, zoals wordt weergegeven in de volgende afbeelding:

    ![Tijdlijn van saml-toepassing in de galerie vermelden](./media/howto-app-gallery-listing/updateorremove.png)

    * Als u een bestaande toepassing bijwerken wilt, selecteert u **bijwerken van bestaande toepassing aanbieding**.
    * Als u verwijderen van een bestaande toepassing in de Azure AD-galerie wilt, selecteert u **verwijderen van de bestaande toepassing aanbieding**.
    * Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

## <a name="listing-requests-by-customers"></a>Aanvragen van de aanbieding door klanten

Klanten kunnen dien de aanvraag van de aanbieding van een toepassing door te klikken op **App aanvragen door klanten** -> **nieuwe aanvraag verzenden**.

![Klant aangevraagde apps tegel](./media/howto-app-gallery-listing/customer-submit-request.png)

Hieronder vindt u dat de stroom van de klant aangevraagd toepassingen:

![Klant apps stroom aangevraagd](./media/howto-app-gallery-listing/customer-request.png)

## <a name="timelines"></a>Tijdlijnen

De tijdlijn voor het proces van de aanbieding van een SAML 2.0 of WS-Federation-toepassing in de galerie is 7-10 werkdagen.

   ![Tijdlijn van saml-toepassing in de galerie vermelden](./media/howto-app-gallery-listing/timeline.png)

De tijdlijn voor het proces van de aanbieding van een toepassing OpenID Connect in de galerie is 2 tot 5 dagen terugbetaald.

   ![Tijdlijn van saml-toepassing in de galerie vermelden](./media/howto-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Escalaties

Voor elke escalaties e-mailbericht verzendt de [Team van Azure AD-eenmalige aanmelding integratie](mailto:SaaSApplicationIntegrations@service.microsoft.com) die SaaSApplicationIntegrations@service.microsoft.com en ontvangt zo snel mogelijk een antwoord.
