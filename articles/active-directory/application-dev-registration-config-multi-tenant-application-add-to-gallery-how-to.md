---
title: Het toevoegen van een toepassing met meerdere tenants naar de galerie van Azure AD-toepassing | Microsoft Docs
description: Legt uit hoe u kunt uw aangepaste ontwikkelde multitenant toepassing weergeven in de Azure AD-Toepassingsgalerie
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92c1651a-675d-42c8-b337-f78e7dbcc40d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: jeedes
ms.openlocfilehash: f29f7cbf118d4d70c1ea2cca174ff0cf0ba9bd14
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-add-a-multi-tenant-application-to-the-azure-ad-application-gallery"></a>Het toevoegen van een multitenant-toepassing naar de Azure AD-toepassingsgalerie

## <a name="what-is-the-azure-ad-application-gallery"></a>Wat is de Azure AD-Toepassingsgalerie?

Azure AD is een cloud-gebaseerde Identity-service. [App-galerie van Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/) is een algemene store waar alle connectors van de toepassing voor eenmalige aanmelding en gebruikersaanvragen worden gepubliceerd. Onze wederzijdse klanten die van Azure AD als id-provider gebruikmaken zoekt naar verschillende SaaS-toepassing connectors, die hier worden gepubliceerd. IT-beheerder voegt de connector uit de app-galerie en configureert en gebruikt voor eenmalige aanmelding en de inrichting. Azure AD ondersteunt alle belangrijke federation-protocollen, zoals SAML 2.0, OpenID Connect, OAuth en WS-Fed voor eenmalige aanmelding. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Als uw toepassing biedt ondersteuning voor SAML- of OpenIDConnect
Als u een multitenant-toepassing die u wilt weergeven in de Azure AD-Toepassingsgalerie hebt, moet u eerst controleren of uw toepassing een van de volgende eenmalige aanmelding technologieën ondersteunt:

1. **OpenID Connect** : de multitenant-toepassing in Azure AD maken en implementeren van [Azure AD toestemming framework](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework) voor uw toepassing. Het aanmeldingsverzoek naar algemene eindpunt verzenden zodat elke klant toestemming voor de toepassing kunt bieden. U kunt de klanttoegang op basis van de tenant-ID en van de gebruiker UPN in het token ontvangen en beheren. Verzend de toepassing zoals vermeld in dit [artikel](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing).

2. **SAML** : als uw toepassing SAML 2.0 ondersteunt en we kunnen uw toepassing in de galerie vermelden en de instructies worden vermeld [hier](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing)

Als uw toepassing ondersteuning biedt voor een van deze modi voor eenmalige aanmelding en u wilt uw toepassing met meerdere tenants in de Azure AD-Toepassingsgalerie weergeven, kunt u de stappen die worden vermeld [dit](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing) artikel. 

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Als uw toepassing biedt geen ondersteuning voor SAML of OpenIDConnect
Zelfs als uw toepassing biedt geen ondersteuning voor een van deze modi, kunnen we het nog steeds integreren in onze galerie met onze wachtwoord Single Sign-on-technologie.

**Wachtwoord SSO** : Maak een webtoepassing met een HTML-aanmeldingspagina configureren [op basis van wachtwoorden eenmalige aanmelding](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis). Op basis van wachtwoorden SSO, ook bekend als wachtwoord vaulting, kunt u voor het beheren van toegang voor gebruikers en wachtwoorden tot webtoepassingen die geen ondersteuning voor identiteitsfederatie. Het is ook nuttig voor scenario's waarin meerdere gebruikers één account moeten, zoals delen op uw organisatie sociale media app accounts. 

Als u wilt weergeven van uw toepassing met deze technologie vervolgens verzend de aanvraag zoals beschreven in [dit](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing) artikel.

## <a name="escalations"></a>Escalaties

Voor elke escalaties neerzetten een e-mail naar [SSO van Azure AD-integratie Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) en we u zo snel beperking Zo snel mogelijk.

## <a name="next-steps"></a>Volgende stappen
[Hoe u uw toepassing in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
