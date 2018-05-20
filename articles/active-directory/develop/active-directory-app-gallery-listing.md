---
title: Uw toepassing vermelden in de Azure Active Directory-toepassingsgalerie | Microsoft Docs
description: Meer informatie over hoe u een toepassing die ondersteuning biedt voor eenmalige aanmelding in de app-galerie van Azure Active Directory
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
ms.date: 05/09/2018
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: 5f42a706bd7cb44162765bb77039cc3173d6941e
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="list-your-application-in-the-azure-active-directory-application-gallery"></a>Uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie


##  <a name="what-is-the-azure-ad-application-gallery"></a>Wat is de Azure AD-toepassingsgalerie?

Azure Active Directory (Azure AD) is een cloud-gebaseerde identity-service. De [Azure AD-toepassingsgalerie](https://azure.microsoft.com/marketplace/active-directory/all/) is in de Azure Marketplace appstore, waar alle toepassing connectors voor eenmalige aanmelding en gebruikersaanvragen worden gepubliceerd. Klanten die gebruikmaken van Azure AD als id-provider vinden voor de verschillende SaaS-toepassing connectors hier gepubliceerd. IT-beheerders connectors van de app-galerie toevoegen en vervolgens configureren en gebruiken van de connectors voor eenmalige aanmelding en de inrichting. Azure AD ondersteunt alle belangrijke federation-protocollen voor eenmalige aanmelding, met inbegrip van SAML 2.0, OpenID Connect, OAuth en WS-Fed.

## <a name="what-are-the-benefits-of-listing-an-application-in-the-gallery"></a>Wat zijn de voordelen van het opsommen van een toepassing in de galerie?

*  Klanten vinden één aanmelding prettig.

*  Configuratie van de toepassing is eenvoudig en minimale. 

*  Uw toepassing zoekt een snelle zoekopdracht in de galerie.

*  Vrijmaken, Basic, en Azure AD Premium klanten kunnen worden gebruikt door deze integratie. 

*  Wederzijdse klanten krijgen een zelfstudie stapsgewijze configuratie. 

*  Klanten die gebruikmaken van SCIM kunnen gebruiken voor dezelfde app wordt ingericht.


##  <a name="prerequisites-implement-federation-protocol"></a>Vereisten: Protocol voor implementeren federation

Als een toepassing in de galerie van Azure AD-app wilt weergeven, moet u eerst een van de volgende federation protocollen die worden ondersteund door Azure AD implementeren. Lees de voorwaarden en bepalingen van de Azure AD-toepassingsgalerie hiervandaan. 

*   **OpenID Connect**: de multitenant-toepassing in Azure AD maken en implementeren van de [Azure AD toestemming framework](active-directory-integrating-applications.md#overview-of-the-consent-framework) voor uw toepassing. Het aanmeldingsverzoek verzenden naar een gemeenschappelijk eindpunt zodat elke klant toestemming voor de toepassing kunt bieden. U kunt toegang van gebruikers op basis van de tenant-ID en de gebruiker UPN in het token wordt ontvangen. Voor het integreren van uw toepassing met Azure AD, volgt u de [instructies van de ontwikkelaar](active-directory-authentication-scenarios.md).

    ![Tijdlijn van het OpenID Connect toepassing in de galerie opsommen](./media/active-directory-app-gallery-listing/openid.png)

    * Als u wilt dat uw toepassing toevoegen aan lijst in de galerie met OpenID Connect, selecteer **OpenID Connect en OAuth 2.0** zoals hierboven.

    * Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [SSO van Azure AD-integratie Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

*   **SAML 2.0** of **WS Fed**: uw toepassing moet de mogelijkheid om u te doen de WS-SAML-Fed SSO-integratie in de modus SP of IDP hebben. Als uw app SAML 2.0 ondersteunt, kunt u deze integreren rechtstreeks met een Azure AD-tenant met behulp van de [instructies voor het toevoegen van een aangepaste toepassing](../active-directory-saas-custom-apps.md).

    ![Tijdlijn van het SAML 2.0 of de WS-Fed toepassing in de galerie opsommen](./media/active-directory-app-gallery-listing/saml.png)

    * Als u wilt dat uw toepassing toevoegen aan lijst aan de galerie met **SAML 2.0** of **WS Fed**, selecteer **SAMl 2.0/WS-Fed** zoals hierboven.

    * Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [SSO van Azure AD-integratie Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

*   **Wachtwoord SSO**: Maak een webtoepassing met een HTML-aanmeldingspagina configureren [op basis van wachtwoorden eenmalige aanmelding](../manage-apps/what-is-single-sign-on.md). Op basis van wachtwoorden SSO, ook bekend als wachtwoord vaulting, kunt u voor het beheren van toegang voor gebruikers en wachtwoorden tot webtoepassingen die geen ondersteuning voor identiteitsfederatie. Het is ook nuttig voor scenario's waarin meerdere gebruikers moeten één account, zoals delen op uw organisatie sociale media app accounts.

    ![Tijdlijn van het wachtwoord SSO-toepassing in de galerie opsommen](./media/active-directory-app-gallery-listing/passwordsso.png)

    * Als u wilt dat uw toepassing toevoegen aan lijst in de galerie met wachtwoord SSO, selecteer **wachtwoord SSO** zoals hierboven.

    * Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [SSO van Azure AD-integratie Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

##  <a name="updateremove-existing-listing"></a>Bestaande aanbieding bijwerken/verwijderen

Als u wilt bijwerken of verwijderen van een bestaande toepassing in de galerie van Azure AD-app, moet u eerst de aanvraag in de [toepassing netwerk Portal](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Als u een Office 365-account hebt, gebruikt u dat aan te melden bij deze portal. Als dat niet het geval is, gebruik je Microsoft-account (zoals Outlook of Hotmail) aan te melden.

* Selecteer relevante optie uit de onderstaande afbeelding

    ![Tijdlijn van het saml-toepassing in de galerie opsommen](./media/active-directory-app-gallery-listing/updateorremove.png)
    
    * Als u bijwerken van een bestaande toepassing wilt, selecteert u **bijwerken van bestaande toepassing aanbieding**.

    * Als u verwijderen van een bestaande toepassing uit de galerie van Azure AD wilt, selecteert u **bestaande vermelding van de toepassing verwijderen**

    * Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [SSO van Azure AD-integratie Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

## <a name="submit-the-request-in-the-portal"></a>Dien de aanvraag in de portal

Nadat u hebt getest dat de integratie van uw toepassingen met Azure AD werkt, dient u de aanvraag voor toegang op onze [toepassing netwerk Portal](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Als u een Office 365-account hebt, gebruikt u dat aan te melden bij deze portal. Als dat niet het geval is, gebruik je Microsoft-account (zoals Outlook of Hotmail) aan te melden.

Nadat u zich aanmeldt, wordt de volgende pagina weergegeven. Geef een zakelijke rechtvaardiging voor de toegang in het tekstvak nodig en selecteer vervolgens **toegang aanvragen**. Ons team beoordeelt de details en hebt die u toegang dienovereenkomstig. Daarna kunt u zich aanmeldt bij de portal en dient u de gedetailleerde aanvraag voor de toepassing.

Als u problemen met betrekking tot toegang hebt, neem dan contact op met de [SSO van Azure AD-integratie Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![Aanvraag voor toegang op de SharePoint-portal](./media/active-directory-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Tijdlijnen
    
De planning voor het proces van het opsommen van een SAML 2.0 of de WS-Fed toepassing in de galerie is 7-10 werkdagen.

   ![Tijdlijn van het saml-toepassing in de galerie opsommen](./media/active-directory-app-gallery-listing/timeline.png)

De planning voor het proces van het opsommen van een toepassing OpenID Connect in de galerie is 2-5 werkdagen.

   ![Tijdlijn van het saml-toepassing in de galerie opsommen](./media/active-directory-app-gallery-listing/timeline2.png)

De planning voor het proces van het opsommen van de toepassing in de galerie met gebruikers inrichten ondersteuning is 40 45 dagen.

   ![Tijdlijn van het saml-toepassing in de galerie opsommen](./media/active-directory-app-gallery-listing/provisioningtimeline.png)

## <a name="escalations"></a>Escalaties

Voor elke escalaties e-mailbericht verzendt de [SSO van Azure AD-integratie Team](mailto:SaaSApplicationIntegrations@service.microsoft.com) die SaaSApplicationIntegrations@service.microsoft.com en reageren zo snel mogelijk.