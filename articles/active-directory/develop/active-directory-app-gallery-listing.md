---
title: Lijst van uw toepassing in de Azure Active Directory-toepassingsgalerie
description: Hoe u een toepassing die ondersteuning biedt voor eenmalige aanmelding in de galerie van Azure Active Directory | Microsoft Azure
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
ms.openlocfilehash: feb09aa8f8e22ad6fbda6a490d251c500bedf3ee
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Lijst van uw toepassing in de Azure Active Directory-toepassingsgalerie


##  <a name="what-is-azure-ad-app-gallery"></a>Wat is de galerie van Azure AD-app?

Azure AD is een cloud-gebaseerde Identity-service. [App-galerie van Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/) is een algemene store waar alle connectors van de toepassing voor eenmalige aanmelding en gebruikersaanvragen worden gepubliceerd. Onze wederzijdse klanten die van Azure AD als id-provider gebruikmaken zoekt naar verschillende SaaS-toepassing connectors, die hier worden gepubliceerd. IT-beheerder voegt de connector uit de app-galerie en configureert en gebruikt voor eenmalige aanmelding en de inrichting. Azure AD ondersteunt alle belangrijke federation-protocollen, zoals SAML 2.0, OpenID Connect, OAuth en WS-Fed voor eenmalige aanmelding. 

## <a name="what-are-the-benefits-of-listing-the-application-in-the-gallery"></a>Wat zijn de voordelen van het opsommen van de toepassing in de galerie?

*  Geef best mogelijke eenmalige aanmelding gebruikerservaring aan klanten.

*  Eenvoudige en minimale configuratie van de toepassing.

*  Klanten kunnen de toepassing zoeken en deze te vinden in de galerie. 

*  Elke klant kan deze integratie ongeacht de gratis Azure AD-SKU, Basic of Premium kunt gebruiken.

*  Stap door de zelfstudie voor stap de configuratie voor de wederzijdse klanten.

*  De de gebruikersinrichting inschakelen voor dezelfde app als u van SCIM gebruikmaakt.


##  <a name="what-are-the-pre-requisites"></a>Wat zijn de vereisten?

Als u een toepassing in de galerie van Azure AD, moet de toepassing eerst te implementeren op een van de federation-protocollen ondersteund door Azure AD. Lees de voorwaarden en bepalingen van de Azure AD-toepassingsgalerie hiervandaan. Als u gebruikt: 

*   **OpenID Connect** : de multitenant-toepassing in Azure AD maken en implementeren van [Azure AD toestemming framework](active-directory-integrating-applications.md#overview-of-the-consent-framework) voor uw toepassing. Het aanmeldingsverzoek naar algemene eindpunt verzenden zodat elke klant toestemming voor de toepassing kunt bieden. U kunt de klanttoegang op basis van de tenant-ID en van de gebruiker UPN in het token ontvangen en beheren. Voor het integreren van uw toepassing met Azure AD, kunt u de [developer instructies](active-directory-authentication-scenarios.md).

*   **SAML 2.0 of de WS-Fed** – de toepassing moet zijn een functie in de WS-SAML-Fed SSO-integratie in de modus SP of IDP doen. Alle Apps die ondersteuning biedt voor SAML 2.0, kunnen rechtstreeks worden geïntegreerd met een Azure AD-tenant met behulp van de [instructies voor het toevoegen van een aangepaste toepassing](../active-directory-saas-custom-apps.md).

*   **Wachtwoord SSO** : Maak een webtoepassing met een HTML-aanmeldingspagina configureren [op basis van wachtwoorden eenmalige aanmelding](../active-directory-appssoaccess-whatis.md). Op basis van wachtwoorden SSO, ook bekend als wachtwoord vaulting, kunt u voor het beheren van toegang voor gebruikers en wachtwoorden tot webtoepassingen die geen ondersteuning voor identiteitsfederatie. Het is ook nuttig voor scenario's waarin meerdere gebruikers één account moeten, zoals delen op uw organisatie sociale media app accounts. 

## <a name="process-for-submitting-the-request-in-the-portal"></a>Proces voor het indienen van de aanvraag in de portal

Nadat u hebt getest dat de integratie van uw toepassingen met Azure AD werkt, moet u uw aanvraag indienen voor toegang op onze [toepassing netwerk Portal](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Als u een Office 365-account hebt, kunt u die kunt gebruiken om te melden bij deze portal anders, gebruikt u uw Microsoft-ID (Live ID, Outlook, Hotmail enz.) om aan te melden. U kunt zien volgende pagina om de toegang te vragen. Geef een zakelijke rechtvaardiging in het tekstvak en klik op **toegang aanvragen**. Ons team wordt de gedetailleerde gegevens bekijken en geeft u de toegang dienovereenkomstig. Daarna kunt u zich aanmeldt bij de portal en dient u de gedetailleerde aanvraag voor de toepassing.

Als u een probleem met betrekking tot de toegang wordt geconfronteerd, neem dan contact op met [SSO van Azure AD-integratie Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![Aanvraag voor toegang op de SharePoint-portal](./media/active-directory-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Tijdlijnen
    
*   Proces van aanbieding SAML 2.0 of de WS-Fed-toepassing in de galerie - **7-10 dagen**

   ![Tijdlijn van het saml-toepassing in de galerie opsommen](./media/active-directory-app-gallery-listing/timeline.png)

*   Proces van het opsommen van OpenID Connect toepassing in de galerie - **2-5 werkdagen**

   ![Tijdlijn van het saml-toepassing in de galerie opsommen](./media/active-directory-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Escalaties

Voor elke escalaties neerzetten een e-mail naar [SSO van Azure AD-integratie Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) en we u zo snel beperking Zo snel mogelijk.

