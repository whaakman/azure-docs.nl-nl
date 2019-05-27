---
title: Web-app die gebruikers (overzicht) - Microsoft identity-platform worden aangemeld
description: Informatie over het bouwen van een web-app die zich aanmeldt gebruikers (overzicht)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6ce534c6eeecba220fd829be829caa679df52055
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65833086"
---
# <a name="scenario-web-app-that-signs-in-users"></a>Scenario: Web-app waarmee gebruikers worden aangemeld

Meer informatie over alles die wat u nodig om te maken van een web-app waarmee gebruikers zich aanmeldt met het Microsoft identity-platform.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Aan de slag

Als u maken van uw eerste draagbare (ASP.NET Core) web-apps die zich in gebruikers wilt, voert u in deze Quick Start:

> [!div class="nextstepaction"]
> [Snelstart: ASP.NET Core web-app waarmee gebruikers zich aanmeldt](quickstart-v2-aspnet-core-webapp.md)

Als u liever om te blijven met ASP.NET, probeer de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Snelstart: ASP.NET web-app waarmee gebruikers zich aanmeldt](quickstart-v2-aspnet-webapp.md)

## <a name="overview"></a>Overzicht

U toevoegen verificatie aan uw web-app, zodat deze gebruikers kan aanmelden. Toe te voegen verificatie kunnen uw web-app voor toegang tot beperkte profielgegevens en, bijvoorbeeld de aanpassen die aan de gebruikers te bieden. Web-apps verifiëren van een gebruiker in een webbrowser. De web-App in dit scenario zorgt ervoor dat de browser van de gebruiker ze zich aanmelden bij Azure AD. Azure AD retourneert een antwoord aanmelden via de browser van de gebruiker, met claims over de gebruiker in een beveiligingstoken. Aanmelden-gebruikers profiteren van de [Open ID Connect](./v2-protocols-oidc.md) standaardprotocol zelf vereenvoudigd door het gebruik van middleware [bibliotheken](scenario-web-app-sign-user-app-configuration.md#libraries-used-to-protect-web-apps).

![Web-app zich aanmeldt gebruikers](./media/scenario-webapp/scenario-webapp-signs-in-users.svg)

U kunt ook uw toepassing voor het aanroepen van Web-API's namens de gebruiker is aangemeld als een tweede fase inschakelen. Deze volgende fase is een ander scenario, die u vindt in [Web-App-Web-API's aanroepen](scenario-web-app-call-api-overview.md)

> [!NOTE]
> Aanmelden toevoegen aan een web-app is over het beveiligen van de web-app en het token van een gebruiker wilt valideren, dit is wat **middleware** bibliotheken doen. In dit scenario is niet vereist, maar Microsoft Authentication Libraries (MSAL), die over het ophalen van een token om aan te roepen beveiligd API's. Alleen wordt de verificatiebibliotheken geïntroduceerd in het scenario voor follow-up wanneer de web-app nodig heeft voor het aanroepen van web-API's.

## <a name="specifics"></a>Specifieke informatie

- Tijdens de registratie van de toepassing, moet u voor een of meerdere (als u uw app implementeren op verschillende locaties) URI's beantwoorden. In sommige gevallen (ASP.NET/ASP.NET Core) moet u de IDToken inschakelen. Tot slot moet u een URI zijn afmelding zo instellen dat uw toepassing op gebruikers ondertekening reageert-out.
- In de code voor uw toepassing moet u de instantie waarmee u web-app gemachtigden aanmelden opgeven. Het is raadzaam om aan te passen van validatie van tokens (met name in de ISV-scenario's).
- Web-Apps ondersteuning voor alle accounttypen. Zie voor meer informatie, [accounttypen ondersteund](v2-supported-account-types.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Appregistratie](scenario-web-app-sign-user-app-registration.md)
