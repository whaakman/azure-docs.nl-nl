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
ms.date: 04/27/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: cf25772bd9d92b59401aa5da76e6bbd5fa5ee3e5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Lijst van uw toepassing in de Azure Active Directory-toepassingsgalerie
Voor een lijst met een toepassing die ondersteuning biedt voor eenmalige aanmelding bij Azure Active Directory in de [galerie van Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/), moet eerst de toepassing voor het implementeren van een van de volgende integratie-modi:

* **OpenID Connect** -directe integratie met Azure AD met OpenID Connect voor verificatie en de Azure AD API toestemming voor configuratie. Als u een integratie net begint en SAML biedt geen ondersteuning voor uw toepassing, is dit de aanbevolen modus.
* **SAML** – de toepassing al heeft de mogelijkheid voor het configureren van derden identiteitsproviders met het SAML-protocol.

Aanbieding-vereisten voor elke fase zijn hieronder.

## <a name="openid-connect-integration"></a>OpenID Connect-integratie
Uw toepassing integreren met Azure AD, na de [developer instructies](active-directory-authentication-scenarios.md). Vervolgens voltooien van de onderstaande vragen en verzenden naar waadpartners@microsoft.com.

* Geef referenties voor een testtenant of een account met de toepassing die door het team van Azure AD kan worden gebruikt voor het testen van de integratie.  
* Bieden instructies over hoe het team van Azure AD kunt aanmelden en een instantie van Azure AD verbinden met uw toepassing met de [Azure AD toestemming framework](active-directory-integrating-applications.md#overview-of-the-consent-framework). 
* Geef eventuele verdere instructies nodig voor het Azure AD-team voor het testen van eenmalige aanmelding met uw toepassing. 
* De onderstaande gegevens bieden:

> Bedrijfsnaam:
> 
> De Website van het bedrijf:
> 
> De naam van de toepassing:
> 
> Beschrijving van de toepassing (maximaal 200 tekens):
> 
> De Website van de toepassing is (informatief):
> 
> Website van de toepassing technische ondersteuning of contactgegevens:
> 
> Toepassings-ID van de toepassing, zoals wordt weergegeven in de App-details https://portal.azure.com:
> 
> Wanneer klanten gaat u naar zich aanmelden voor de URL van de toepassing aanmelden en/of kopen de toepassing:
> 
> Kies maximaal drie categorieën voor uw toepassing worden vermeld in (voor beschikbare categorieën Zie Azure Active Directory Marketplace):
> 
> Kleine pictogrammen van toepassing (PNG-bestand, 45px door 45px, effen achtergrondkleur) koppelen:
> 
> Koppelen met grote pictogrammen van toepassing (PNG-bestand, 215px door 215px, effen achtergrondkleur):
> 
> Logo van de toepassing (PNG-bestand, 150px door 122px, transparante achtergrondkleur) koppelen:
> 
> 

## <a name="saml-integration"></a>SAML-integratie
Alle Apps die ondersteuning biedt voor SAML 2.0 kan rechtstreeks worden geïntegreerd met een Azure AD-tenant met behulp van [deze instructies voor het toevoegen van een aangepaste toepassing](../active-directory-saas-custom-apps.md). Nadat u hebt getest dat de integratie van uw toepassingen met Azure AD werkt, de volgende informatie om te verzenden < mailto:waadpartners@microsoft.com >.

* Geef referenties voor een testtenant of een account met de toepassing die door het team van Azure AD kan worden gebruikt voor het testen van de integratie.  
* De SAML aanmeldings-URL, de URL-verlener (entiteit-ID) en de antwoord-URL (assertion consumer-service)-waarden voor uw toepassing bieden, zoals beschreven [hier](../active-directory-saas-custom-apps.md). Als u doorgaans deze waarden als onderdeel van een metagegevensbestand SAML opgeeft, klikt u vervolgens kunt u sturen die ook.
* Geef een korte beschrijving van hoe u Azure AD configureren als een id-provider in uw toepassing met behulp van SAML 2.0. Als uw toepassing ondersteuning biedt voor het configureren van de Azure AD als een id-provider door middel van een administratieve selfserviceportal, vervolgens controleert u de hierboven opgegeven referenties zijn de mogelijkheid om in te stellen dit.
* De onderstaande gegevens bieden:

> Bedrijfsnaam:
> 
> De Website van het bedrijf:
> 
> De naam van de toepassing:
> 
> Beschrijving van de toepassing (maximaal 200 tekens):
> 
> De Website van de toepassing is (informatief):
> 
> Website van de toepassing technische ondersteuning of contactgegevens:
> 
> Wanneer klanten gaat u naar zich aanmelden voor de URL van de toepassing aanmelden en/of kopen de toepassing:
> 
> Kies maximaal drie categorieën voor uw toepassing worden weergegeven onder (voor Zie beschikbare categorieën het [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):
> 
> Kleine pictogrammen van toepassing (PNG-bestand, 45px door 45px, effen achtergrondkleur) koppelen:
> 
> Koppelen met grote pictogrammen van toepassing (PNG-bestand, 215px door 215px, effen achtergrondkleur):
> 
> Logo van de toepassing (PNG-bestand, 150px door 122px, transparante achtergrondkleur) koppelen:
> 
> 

