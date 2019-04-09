---
title: Verificatie in Microsoft identity-platform | Azure
description: Meer informatie over verificatie in Microsoft identity-platform, de app modelleren, API, inrichting, en dat Microsoft identity-platform biedt ondersteuning voor de meest voorkomende scenario's voor verificatie.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 0c84e7d0-16aa-4897-82f2-f53c6c990fd9
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/05/2019
ms.author: celested
ms.reviewer: saeeda, sureshja, hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b1d54347b9a3ccc72cfd5b88400d699d93132fbf
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59266298"
---
# <a name="what-is-authentication"></a>Wat is verificatie?

Bij *verificatie* wordt een partij gevraagd om legitieme referenties. Op basis hiervan kan een beveiligingsprincipal worden gemaakt voor identiteits- en toegangsbeheer. Eenvoudiger gezegd wordt bij dit proces bewezen dat u bent wie u zegt te zijn. Verificatie wordt soms afgekort tot 'AuthN'.

Bij *autorisatie* krijgt een geverifieerde beveiligingsprincipal toestemming om iets te doen. Er wordt aangegeven welke gegevens mogen worden gebruikt en wat ermee mag worden gedaan. Autorisatie wordt soms afgekort tot 'AuthZ'.

Microsoft identity-platform vereenvoudigt-verificatie voor ontwikkelaars van toepassingen door op te geven van de identiteit als een service, met ondersteuning voor standaardprotocollen zoals OAuth 2.0 en OpenID Connect, evenals open source-bibliotheken voor verschillende platforms te kennis helpt u snel coderen.

Er zijn twee primaire gebruiksscenario's in het programmeermodel van Microsoft identity-platform:

* Tijdens een OAuth 2.0-stroom voor autorisatie - wanneer de resource-eigenaar autorisatie verleent aan de clienttoepassing, zodat de client toegang verkrijgt tot de resources van de resource-eigenaar.
* Wanneer de client resources opent - zoals geïmplementeerd door de resourceserver, met gebruik van de claimwaarden in het toegangstoken, om toegangsbeheerbeslissingen te nemen op basis van die waarden.

## <a name="authentication-basics-in-microsoft-identity-platform"></a>De basisbeginselen van verificatie in Microsoft identity-platform

Bedenk het meest eenvoudige scenario waarin identiteiten zijn vereist: een gebruiker moet zich in een webbrowser verifiëren bij een webtoepassing. In het volgende diagram is dit scenario te zien:

![Overzicht van het aanmelden bij een webtoepassing](./media/authentication-scenarios/auth-basics-microsoft-identity-platform.svg)

Hier volgt wat u moet weten over de verschillende onderdelen in het diagram:

* Microsoft identity-platform is de id-provider. De identiteitsprovider is verantwoordelijk voor het verifiëren van de identiteit van gebruikers en toepassingen uit de directory van een organisatie. Er worden ook beveiligingstokens uitgegeven nadat die gebruikers en toepassingen zijn geverifieerd.
* Een toepassing die wil uitbesteden identiteitsplatform van Microsoft-verificatie moet zijn geregistreerd in Azure Active Directory (Azure AD). Azure AD registreert de app in de directory en geeft deze een unieke id.
* Ontwikkelaars kunnen de open-source Microsoft identity-platform-verificatiebibliotheken gebruiken voor verificatie vereenvoudigen doordat de gegevens van het protocol voor u. Zie voor meer informatie, Microsoft identity-platform [v2.0-verificatiebibliotheken](reference-v2-libraries.md) en [v1.0-verificatiebibliotheken](active-directory-authentication-libraries.md).
* Wanneer een gebruiker is geverifieerd, moet de toepassing het beveiligingstoken van de gebruiker valideren om er zeker van te zijn dat de verificatie is gelukt. U kunt snelstartgidsen, zelfstudies, codevoorbeelden in verschillende talen en frameworks bekijken waarin u ziet wat de toepassing moet doen.
  * Om snel een app te bouwen en functionaliteit toe te voegen, zoals tokens ophalen, tokens vernieuwen, gebruikers aanmelden, bepaalde gebruikersgegevens weergeven, enzovoort, gaat u naar het gedeelte **Snelstartgidsen** in de documentatie.
  * Ga naar het gedeelte **Zelfstudies** van de documentatie voor uitgebreide, op scenario's gebaseerde procedures voor ontwikkelaarstaken (zoals toegangstokens verkrijgen en deze gebruiken in aanroepen naar de Microsoft Graph-API en andere API's en aanmelden met Microsoft implementeren in een traditionele webbrowser-app via OpenID Connect).
  * Ga naar [GitHub](https://github.com/Azure-Samples?q=active-directory) om codevoorbeelden te downloaden.
* De stroom van aanvragen en antwoorden in het verificatieproces wordt bepaald aan de hand van het gebruikte verificatieprotocol, zoals OAuth 2.0, OpenID Connect, WS-Federation of SAML 2.0. Zie het gedeelte **Concepten > Protocollen** van de documentatie voor meer informatie over protocollen.

In het bovenstaande voorbeeldscenario kunt u apps classificeren aan de hand van deze twee rollen:

* Apps die veilig toegang nodig hebben tot resources
* Apps die zelf de rol van resource hebben

Nu dat u een overzicht van de basisbeginselen hebt, lees verder voor informatie over de app-model voor identiteit en de API, hoe inrichting in Microsoft identity-platform werkt, en naar gedetailleerde informatie over de algemene scenario's die Microsoft identity-platform koppelingen ondersteunt.

## <a name="application-model"></a>Toepassingsmodel

Microsoft identity-platform staat voor toepassingen die een specifiek model die is ontworpen om te voldoen aan de twee belangrijkste functies te volgen:

* **De app identificeren op basis van de ondersteunde verificatieprotocollen** - dit omvat het inventariseren van alle id's, URL's, geheimen en gerelateerde informatie die nodig is tijdens de verificatie. Hier kunt Microsoft identity-platform:

    * Bewaart alle gegevens die nodig zijn om verificatie tijdens runtime te ondersteunen.
    * Bewaart alle gegevens om te beslissen welke resources een app mogelijk nodig heeft en of aanvragen moeten worden uitgevoerd (en onder welke omstandigheden).
    * Biedt de infrastructuur voor het implementeren van app-inrichting in de tenant van de app-ontwikkelaar en in andere Azure AD-tenants.

* **Toestemming van de gebruiker tijdens tokenaanvraag verwerken en de dynamische toewijzing van apps voor tenants te vergemakkelijken** -, Microsoft identity-platform:

    * Stelt gebruikers en beheerders in staat om de app dynamisch toestemming te geven of weigeren om resources namens hen te gebruiken.
    * Stelt beheerders in staat om te beslissen wat apps mogen doen, welke gebruikers gebruik mogen maken van specifieke apps en hoe de directoryresources kunnen worden benaderd.

In Microsoft identity-platform, een **toepassingsobject** beschrijving van een toepassing als een abstracte entiteit. Ontwikkelaars werken met toepassingen. Tijdens de implementatie, Microsoft identity-platform gebruikt een bepaalde toepassing-object als een blauwdruk maken een **service-principal**, die een concrete exemplaar van een toepassing in een map of een tenant vertegenwoordigt. De service-principal bepaalt wat de app daadwerkelijk kan doen in een specifieke doeldirectory, wie deze mag gebruiken, tot welke resources deze toegang heeft en meer. Microsoft identity-platform maakt een service-principal van een toepassingsobject via **toestemming geven**.

Het volgende diagram toont een vereenvoudigde identiteitsplatform van Microsoft flow aangestuurd door toestemming wordt ingericht.  In deze twee tenants bestaan (A en B), waarbij tenant A eigenaar is van de toepassing en tenant B is het instantiëren van de toepassing via een service-principal.  

![Vereenvoudigde inrichtingsstroom op basis van toestemming](./media/authentication-scenarios/simplified-provisioning-flow-consent-driven.svg)

De inrichtingsstroom verloopt als volgt:

|   |   |
|---|---|
| 1 | Een gebruiker van de tenant B wil zich aanmelden met de app. |
| 2 | De gebruikersreferenties worden verkregen en geverifieerd |
| 3 | De gebruiker wordt gevraagd de app toestemming te geven om toegang te verkrijgen tot tenant B |
| 4 | Microsoft identity-platform gebruikt het toepassingsobject in A als een blauwdruk voor het maken van een service principal in tenant B |
| 5 | De gebruiker ontvangt de aangevraagde token |
|   |   |

U kunt dit proces zo vaak herhalen als u wilt (voor andere tenants, zoals C, D, enzovoort). Tenant A behoudt de blauwdruk voor de app (toepassingsobject). De gebruikers en beheerders van de andere tenants waarvoor de app toestemming heeft, behouden controle over wat de toepassing mag doen. Dit kan worden beheerd met het bijbehorende service-principalobject in elke tenant. Zie voor meer informatie, [toepassing en service-principalobjecten in Microsoft identity-platform](app-objects-and-service-principals.md).

## <a name="claims-in-microsoft-identity-platform-security-tokens"></a>Claims in beveiligingstokens van Microsoft identity-platform

Beveiligingstokens (toegang en ID-tokens) dat is uitgegeven door Microsoft identity-platform bevatten claims of asserties van informatie over het onderwerp dat is geverifieerd. Toepassingen kunnen voor verschillende taken gebruikmaken van claims, waaronder:

* Het token valideren
* De directorytenant van het onderwerp identificeren
* Gebruikersgegevens weergeven
* De autorisatie van het onderwerp bepalen

Welke claims aanwezig zijn in een bepaald beveiligingstoken, is afhankelijk van het type token, het type referentie dat is gebruikt om de gebruiker te verifiëren en de configuratie van de toepassing.

Een korte beschrijving van elk type claim verzonden door Microsoft identity-platform is opgegeven in de onderstaande tabel. Zie voor meer informatie, de [toegangstokens](access-tokens.md) en [ID-tokens](id-tokens.md) dat is uitgegeven door Microsoft identity-platform.

| Claim | Description |
| --- | --- |
| Toepassings-id | Identificeert de toepassing die gebruikmaakt van het token. |
| Doelgroep | Identificeert de ontvangende resource waar het token voor is bedoeld. |
| Application Authentication Context Class Reference | Geeft aan hoe de client is geverifieerd (openbare client of vertrouwelijke client). |
| Verificatiemoment | Registreert de datum en tijd waarop de verificatie heeft plaatsgevonden. |
| Verificatiemethode | Geeft aan hoe het onderwerp van het token is geverifieerd (wachtwoord, certificaat, enzovoort). |
| Voornaam | Biedt de naam van de gebruiker zoals ingesteld in Azure AD. |
| Groepen | Bevat de object-id's van de Azure AD-groepen waar de gebruiker lid van is. |
| Identiteitsprovider | Registreert de identiteitsprovider waarmee het onderwerp van het token is geverifieerd. |
| Uitgegeven om | Registreert de tijd waarop het token is uitgegeven (vaak gebruikt om te controleren hoe nieuw het token is). |
| Verlener | Identificeert de beveiligingstokenservice die het token heeft uitgegeven en de Azure AD-tenant. |
| Achternaam | Biedt de achternaam van de gebruiker, zoals ingesteld in Azure AD. |
| Name | Biedt een voor mensen leesbare waarde waarmee het onderwerp van het token wordt geïdentificeerd. |
| Object-id | Bevat een onveranderbare, unieke id voor het onderwerp in Azure AD. |
| Rollen | Bevat beschrijvende namen voor de Azure AD-toepassingsrollen die zijn toegewezen aan de gebruiker. |
| Bereik | Geeft aan welke machtigingen zijn verleend aan de clienttoepassing. |
| Onderwerp | Geeft aan over welke principal het token informatie bevat. |
| Tenant-id | Bevat een onveranderbare, unieke id voor de directorytenant die het token heeft uitgegeven. |
| Levensduur van token | Definieert gedurende welke periode een token geldig is. |
| User principal name | Bevat de user principal name van het onderwerp. |
| Versie | Bevat het versienummer van het token. |

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [toepassingstypen en scenario's die worden ondersteund in Microsoft identity-platform](app-types.md)
