---
title: Web-app dat aanroepen van web-API's (overzicht) - Microsoft identity-platform
description: Informatie over het bouwen van een web-app dat aanroepen van web-API's (overzicht)
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
ms.openlocfilehash: ce45f11a697b72ebdd0fe01166a70e7b47aa8e9f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65076298"
---
# <a name="scenario-web-app-that-calls-web-apis"></a>Scenario: Web-app die web-API's aanroept

Meer informatie over het bouwen van een web-app aanmelden gebruikers op het Microsoft identity-platform en dat aanroepen van web-API's namens de aangemelde gebruiker.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

In dit scenario ver-onderstelt dat u het volgende scenario hebt doorlopen:

> [!div class="nextstepaction"]
> [Web-app waarmee gebruikers zich aanmeldt](scenario-web-app-sign-user-overview.md)

## <a name="overview"></a>Overzicht

U toevoegen verificatie aan uw Web-App, die daarom gebruikers kunnen aanmelden en namens de aangemelde gebruiker een web-API aanroept.

![Web-app die web-API's aanroept](./media/scenario-webapp/web-app.svg)

Web-Apps die worden aangeroepen web-API's:

- zijn vertrouwelijke client-toepassingen.
- Daarom is deze een geheim (toepassingswachtwoord of certificaat) hebt geregistreerd bij Azure AD. Dit geheim wordt doorgegeven in tijdens de oproep aan Azure AD om een token verkrijgen

## <a name="specifics"></a>Specifieke informatie

> [!NOTE]
> Aanmelden toevoegen aan een Web-App gebruikt niet de MSAL-bibliotheken omdat deze over het beveiligen van de Web-App. Beveiliging van bibliotheken wordt bereikt door met de naam Middleware bibliotheken. Dit is het object van het vorige scenario [gebruikers aanmelden voor een Web-App](scenario-web-app-sign-user-overview.md)
>
> Bij het aanroepen van web-API's van een Web-App, moet u toegangstokens ophalen voor deze web-API's. MSAL bibliotheken kunt u deze tokens verkrijgen.

De end-to-end-ervaring van ontwikkelaars voor dit scenario is daarom specifieke aspecten als:

- Tijdens de [toepassingsregistratie](scenario-web-app-call-api-app-registration.md), moet u opgeeft, of verschillende (als u uw app implementeren op verschillende locaties) URI's, geheimen, beantwoorden of certificaten moeten worden gedeeld met Azure AD.
- De [Toepassingsconfiguratie](scenario-web-app-call-api-app-configuration.md) moet de referenties van de client als een gedeeld met Azure AD tijdens de registratie van de toepassing opgeven

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Appregistratie](scenario-web-app-call-api-app-registration.md)
