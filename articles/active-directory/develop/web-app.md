---
title: Web-apps in Azure Active Directory
description: Hierin wordt beschreven wat web-apps zijn en de basisprincipes van stroom protocol, registratie en verlopen van het token voor dit apptype.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4e9704ce014e520350360c336c7a17940cae95fb
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56197892"
---
# <a name="web-apps"></a>Web-apps

Web-apps zijn toepassingen die een gebruiker in een webbrowser om een webtoepassing te verifiëren. De web-App in dit scenario zorgt ervoor dat de browser van de gebruiker ze zich aanmelden bij Azure AD. Azure AD retourneert een antwoord aanmelden via de browser van de gebruiker, met claims over de gebruiker in een beveiligingstoken. In dit scenario biedt ondersteuning voor aanmelding met behulp van de OpenID Connect, SAML 2.0 en WS-Federation-protocollen.

## <a name="diagram"></a>Diagram

![Verificatie-stroom voor de browser naar de web-App](./media/authentication-scenarios/web_browser_to_web_api.png)

## <a name="protocol-flow"></a>Stroom protocol

1. Wanneer een gebruiker bezoekt de toepassing en behoeften aan te melden, worden ze omgeleid via een aanmeldingsaanvraag voor de verificatie-eindpunt in Azure AD.
1. De gebruiker meldt zich aan op de pagina aanmelden.
1. Als verificatie geslaagd is, wordt Azure AD maakt u een verificatietoken en retourneert een antwoord aanmelding aan van de toepassing antwoord-URL die is geconfigureerd in Azure portal. Voor een productietoepassing moet deze antwoord-URL HTTPS. Het geretourneerde token bevat claims over de gebruiker en de Azure AD die nodig zijn voor de toepassing voor het valideren van het token.
1. De toepassing valideert het token met behulp van een openbare ondertekeningssleutel en informatie over de uitgever beschikbaar op het document met federatieve metagegevens voor Azure AD. Nadat de toepassing het token valideert, wordt een nieuwe sessie gestart met de gebruiker. Deze sessie kan de gebruiker toegang tot de toepassing totdat het verloopt.

## <a name="code-samples"></a>Codevoorbeelden

Zie de codevoorbeelden voor scenario's voor web application webbrowser. En controleer regelmatig back-wanneer er nieuwe voorbeelden worden regelmatig toegevoegd.

## <a name="app-registration"></a>App-registratie

Zie voor het registreren van een web-app, [een app registreren bij de Azure AD-eindpunt v1.0](quickstart-v1-add-azure-ad-app.md).

* Één tenant - als u een toepassing voor uw organisatie bouwt, moet deze worden geregistreerd in de directory van uw bedrijf met behulp van de Azure-portal.
* Multi-tenan - als u een toepassing die kan worden gebruikt door gebruikers buiten uw organisatie wilt maken, deze moet worden geregistreerd in de directory van uw bedrijf, maar ook moet worden geregistreerd in de adreslijst van elke organisatie die van de toepassing gebruikmaakt. Als u uw toepassing in de directory, kunt u een aanmeldingsproces opnemen voor uw klanten waarmee ze akkoord gaan met uw toepassing. Wanneer ze zich registreren voor uw toepassing, wordt deze weergegeven met een dialoogvenster waarin de machtigingen die voor de toepassing moet worden weergegeven, en vervolgens de optie om in te stemmen. Afhankelijk van de vereiste machtigingen kan een beheerder in de andere organisatie worden vereist om toestemming te geven. Wanneer de gebruiker of beheerder hiermee akkoord gaat, wordt de toepassing is geregistreerd in de directory.

## <a name="token-expiration"></a>Geldigheidsduur van het token

Sessie van de gebruiker is verlopen wanneer de levensduur van het token dat is uitgegeven door Azure AD is verlopen. Uw toepassing kunt korter deze periode, indien gewenst, zoals het afmelden van gebruikers op basis van een periode van inactiviteit. Wanneer de sessie is verlopen, wordt de gebruiker gevraagd zich opnieuw aanmelden.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [toepassingstypen en scenario's](app-types.md)
* Meer informatie over de Azure AD [de basisbeginselen van verificatie](authentication-scenarios.md)
