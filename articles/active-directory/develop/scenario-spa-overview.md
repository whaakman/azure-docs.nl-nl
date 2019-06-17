---
title: JavaScript-overzicht voor het scenario van toepassing met één pagina - identiteitsplatform van Microsoft
description: Meer informatie over het bouwen van een toepassing met één pagina (overzicht van scenario's) die kan worden geïntegreerd Microsoft identity-platform.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07a21e83f304f3e1acc0ed4033d832dd8e901ac9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65076358"
---
# <a name="scenario-single-page-application"></a>Scenario: Toepassing met één pagina

Meer informatie over alles die wat u nodig voor het bouwen van een toepassing met één pagina (SPA).

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Aan de slag

U kunt uw eerste toepassing maken door de beveiligd-WACHTWOORDVERIFICATIE JavaScript-snelstartgids te volgen:

> [!div class="nextstepaction"]
> [Snelstart: Toepassing met één pagina](./quickstart-v2-javascript.md)

## <a name="overview"></a>Overzicht

Veel moderne web-apps worden gebouwd als client-side één pagina toepassingen die zijn geschreven met behulp van JavaScript- of een beveiligd-WACHTWOORDVERIFICATIE framework zoals Angular, Vue.js en React.js. Deze toepassingen worden uitgevoerd in een webbrowser en hebben verschillende kenmerken dan traditionele serverzijde webtoepassingen. Het Microsoft identity-platform kan toepassingen met één pagina aanmelden van gebruikers en ophalen van tokens voor toegang tot back-endservices of web-API's via de [OAuth 2.0-impliciete stroom](./v2-oauth2-implicit-grant-flow.md). De impliciete stroom kan de toepassing om op te halen van ID-tokens te vertegenwoordigen de geverifieerde gebruiker ook toegangstokens die nodig zijn voor de beveiligde API's aanroepen.

![Toepassingen met één pagina](./media/scenarios/spa-app.svg)

Deze authenticatiestroom bevat geen toepassingsscenario's met behulp van platformoverschrijdende JavaScript-frameworks zoals, React Native enzovoort. Aangezien ze vereisen meer mogelijkheden voor interactie met de systeemeigen platforms.

## <a name="specifics"></a>Specifieke informatie

De volgende aspecten zijn vereist voor dit scenario voor uw toepassing:

* Toepassing registreren met Azure AD omvat het inschakelen van de impliciete stroom en het instellen van een omleidings-URI die tokens worden geretourneerd.
* Configuratie van de toepassing met de eigenschappen van de geregistreerde toepassing zoals toepassings-ID.
* Met behulp van MSAL-bibliotheek kan de verificatiestroom aanmelden en tokens verkrijgen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Appregistratie](scenario-spa-app-registration.md)
