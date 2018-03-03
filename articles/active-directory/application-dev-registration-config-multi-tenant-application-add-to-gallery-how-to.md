---
title: Een multitenant toepassing toevoegen aan de galerie van Azure AD-toepassing | Microsoft Docs
description: Legt uit hoe u uw aangepaste ontwikkelde multitenant toepassing in de galerie van Azure AD-toepassing kunt weergeven.
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
ms.openlocfilehash: 82f7abbe5814f9b154b6888d5b599e7706eb879b
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="add-a-multitenant-application-to-the-azure-ad-application-gallery"></a>Een multitenant toepassing toevoegen aan de Azure AD-toepassingsgalerie

## <a name="what-is-the-azure-ad-application-gallery"></a>Wat is de Azure AD-toepassingsgalerie?

Azure Active Directory (Azure AD) is een cloud-gebaseerde identity-service. De [Azure AD-toepassingsgalerie](https://azure.microsoft.com/marketplace/active-directory/all/) is in de Azure Marketplace appstore, waar alle toepassing connectors voor eenmalige aanmelding en gebruikersaanvragen worden gepubliceerd. Klanten die gebruikmaken van Azure AD als id-provider vinden voor de verschillende SaaS-toepassing connectors hier gepubliceerd. IT-beheerders connectors van de app-galerie toevoegen en vervolgens configureren en gebruiken van de connectors voor eenmalige aanmelding en de inrichting. Azure AD ondersteunt alle belangrijke federation-protocollen, zoals SAML 2.0, OpenID Connect, OAuth en WS-Fed voor eenmalige aanmelding. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Als uw toepassing biedt ondersteuning voor SAML- of OpenIDConnect
Als u een multitenant-toepassing die u wilt weergegeven in de galerie van Azure AD-toepassing hebt, moet u eerst controleren of uw toepassing een van de volgende eenmalige aanmelding technologieën ondersteunt:

- **OpenID Connect**: als u uw app vermeld, de multitenant-toepassing in Azure AD maken en implementeren van de [Azure AD toestemming framework](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework) voor uw toepassing. Het aanmeldingsverzoek verzenden naar een gemeenschappelijk eindpunt zodat elke klant toestemming voor de toepassing kunt bieden. U kunt de toegang van een gebruiker op basis van de tenant-ID en de gebruiker UPN in het token wordt ontvangen. De aanvraag indienen met behulp van het proces beschreven in [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing).

- **SAML**: als uw toepassing SAML 2.0 ondersteunt, de app kan worden weergegeven in de galerie. Volg de instructies in [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Als uw toepassing biedt geen ondersteuning voor SAML of OpenIDConnect
Toepassingen die geen ondersteuning voor SAML- of OpenIDConnect bieden kunnen nog steeds worden geïntegreerd in de app-galerie via één wachtwoord aanmelding technologie.

Wachtwoord eenmalige aanmelding, ook wel wachtwoordkluizen, kunt u gebruikerstoegang en wachtwoorden tot webtoepassingen die geen ondersteuning voor identiteitsfederatie beheren. Het is ook nuttig voor scenario's waarin meerdere gebruikers moeten één account, zoals delen op uw organisatie sociale media app accounts. 

Als u wilt dat uw toepassing met deze technologie weergeven:
1. Maak een webtoepassing met een HTML-aanmeldingspagina configureren [eenmalige aanmelding wachtwoord](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis). 
2. De aanvraag indient zoals beschreven in [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="escalations"></a>Escalaties

Voor elke escalaties e-mailbericht verzendt [SSO van Azure AD-integratie Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) en je krijgt u zo snel mogelijk.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe [uw toepassing vermelden in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).
