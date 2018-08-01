---
title: Toevoegen van een multitenant-toepassing naar de Azure AD-toepassingsgalerie | Microsoft Docs
description: Wordt uitgelegd hoe u uw aangepaste multitenant-toepassing in de Azure AD-toepassingsgalerie kunt weergeven.
services: active-directory
documentationCenter: na
author: barbkess
manager: mtillman
ms.assetid: 92c1651a-675d-42c8-b337-f78e7dbcc40d
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: barbkess
ms.reviewer: jeedes
ms.openlocfilehash: 2f112a191770a2b469f4f8d1bce25973999f40d6
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366867"
---
# <a name="add-a-multitenant-application-to-the-azure-ad-application-gallery"></a>Toevoegen van een multitenant-toepassing naar de Azure AD-toepassingsgalerie

## <a name="what-is-the-azure-ad-application-gallery"></a>Wat is de Azure AD-toepassingsgalerie?

Azure Active Directory (Azure AD) is een identiteitsservice in de cloud gebaseerde. De [Azure AD-toepassingsgalerie](https://azure.microsoft.com/marketplace/active-directory/all/) is in de Azure Marketplace appstore, waar alle toepassingsconnectoren voor eenmalige aanmelding en inrichten van gebruikers worden gepubliceerd. Klanten die gebruikmaken van Azure AD als id-provider vinden de verschillende SaaS-toepassing connectors gepubliceerd hier. IT-beheerders connectors toevoegen vanuit de app-galerie, en vervolgens configureren en gebruiken van de connectors voor eenmalige aanmelding en de inrichting. Azure AD biedt ondersteuning voor alle belangrijke federation-protocollen, zoals SAML 2.0, OpenID Connect, OAuth en WS-Federation voor eenmalige aanmelding. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Als uw toepassing SAML of OpenIDConnect ondersteunt
Als u een multitenant-toepassing die u wilt die worden vermeld in de Azure AD-toepassingsgalerie hebt, moet u eerst zorgen dat uw toepassing ondersteuning biedt voor een van de volgende eenmalige aanmelding-technologieën:

- **OpenID Connect**: als u wilt dat uw app weergegeven, de multitenant-toepassing in Azure AD maken en implementeren van de [Azure AD-toestemmingsframework](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework) voor uw toepassing. De aanmeldingsaanvraag naar een gemeenschappelijk eindpunt verzenden, zodat elke klant u toestemming voor de toepassing krijgt. U kunt de toegang van een gebruiker op basis van de tenant-ID en de UPN van de gebruiker in het token ontvangen. De aanvraag indienen met behulp van het proces wordt beschreven in [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

- **SAML**: als uw toepassing SAML 2.0 ondersteunt, de app kan worden weergegeven in de galerie. Volg de instructies in [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Als uw toepassing geen ondersteuning biedt voor SAML of OpenIDConnect
Toepassingen die geen ondersteuning voor SAML of OpenIDConnect kunnen nog steeds worden geïntegreerd in de app-galerie met één wachtwoord aanmeldings-technologie.

Wachtwoord eenmalige aanmelding, ook wel genoemd wachtwoordkluis, kunt u gebruikerstoegang en wachtwoorden voor webtoepassingen die bieden geen ondersteuning voor identiteitsfederatie beheren. Het is ook handig voor scenario's waarin meerdere gebruikers één account moeten, zoals om accounts voor sociale media-app van uw organisatie te delen. 

Als u weergeven van uw toepassing met deze technologie wilt:
1. Maak een webtoepassing met een HTML-aanmeldingspagina configureren [eenmalige aanmelding wachtwoord](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis). 
2. Dien de aanvraag, zoals beschreven in [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="escalations"></a>Escalaties

E-mailbericht verzendt voor elk escalaties [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) en je krijgt u zo snel mogelijk.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).
