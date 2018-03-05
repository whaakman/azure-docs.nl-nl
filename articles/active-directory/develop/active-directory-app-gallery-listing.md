---
title: Uw toepassing in de Azure Active Directory-toepassingsgalerie aanbieding | Microsoft Docs
description: Hoe u een toepassing die ondersteuning biedt voor eenmalige aanmelding in de app-galerie van Azure Active Directory
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/09/2018
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 502fb555bd3b381c9be0ff04e210cc07f9bf6cd8
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
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

*   **SAML 2.0** of **WS Fed**: uw toepassing moet de mogelijkheid om u te doen de WS-SAML-Fed SSO-integratie in de modus SP of IDP hebben. Als uw app SAML 2.0 ondersteunt, kunt u deze integreren rechtstreeks met een Azure AD-tenant met behulp van de [instructies voor het toevoegen van een aangepaste toepassing](../active-directory-saas-custom-apps.md).

*   **Wachtwoord SSO**: Maak een webtoepassing met een HTML-aanmeldingspagina configureren [op basis van wachtwoorden eenmalige aanmelding](../active-directory-appssoaccess-whatis.md). Op basis van wachtwoorden SSO, ook bekend als wachtwoord vaulting, kunt u voor het beheren van toegang voor gebruikers en wachtwoorden tot webtoepassingen die geen ondersteuning voor identiteitsfederatie. Het is ook nuttig voor scenario's waarin meerdere gebruikers moeten één account, zoals delen op uw organisatie sociale media app accounts. 

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

## <a name="escalations"></a>Escalaties

Voor elke escalaties e-mailbericht verzendt de [SSO van Azure AD-integratie Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) en reageren zo snel mogelijk.

