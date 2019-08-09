---
title: Java script-overzicht van een toepassings scenario met één pagina-micro soft Identity-platform
description: Meer informatie over het bouwen van een toepassing met één pagina (scenario-overzicht) waarin het micro soft Identity-platform wordt geïntegreerd.
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
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: c3c4e671473ff6c6fecdc13fe61dbde1d3fb3809
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852517"
---
# <a name="scenario-single-page-application"></a>Scenario: Toepassing met één pagina

Meer informatie over wat u nodig hebt om een toepassing met één pagina (SPA) te maken.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Aan de slag

U kunt uw eerste toepassing maken met behulp van de Java script SPA Quick Start:

> [!div class="nextstepaction"]
> [Snelstart: Toepassing met één pagina](./quickstart-v2-javascript.md)

## <a name="overview"></a>Overzicht

Veel moderne webtoepassingen zijn gebouwd als toepassingen met één pagina op de client die zijn geschreven met behulp van Java script of een beveiligd-wachtwoord verificatie-Framework zoals een hoek, vue. js en reageren. js. Deze toepassingen worden uitgevoerd in een webbrowser en hebben verschillende verificatie kenmerken dan traditionele webtoepassingen aan de server zijde. Met het micro soft Identity-platform kunnen toepassingen met één pagina worden aangemeld en tokens worden opgehaald om toegang te krijgen tot back-end-services of Web-Api's met behulp van de [impliciete OAuth 2,0-stroom](./v2-oauth2-implicit-grant-flow.md). Met de impliciete stroom kan de toepassing ID-tokens ophalen die de geverifieerde gebruiker vertegenwoordigen en ook toegangs tokens hebben die nodig zijn om beveiligde Api's aan te roepen.

![Toepassingen met één pagina](./media/scenarios/spa-app.svg)

Deze verificatie stroom bevat geen toepassings scenario's die gebruikmaken van platformoverschrijdende java script frameworks, zoals elektroden, reageren op systeem eigen, enzovoort. omdat ze verdere mogelijkheden nodig hebben voor interactie met de systeem eigen platformen.

## <a name="specifics"></a>Opsporingsgegevens

De volgende aspecten zijn vereist om dit scenario in te scha kelen voor uw toepassing:

* Toepassings registratie bij Azure AD omvat het inschakelen van de impliciete stroom en het instellen van een omleidings-URI waarmee tokens worden geretourneerd.
* Toepassings configuratie met de geregistreerde toepassings eigenschappen, zoals de toepassings-ID.
* De MSAL-bibliotheek gebruiken om de verificatie stroom uit te voeren en tokens op te halen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Appregistratie](scenario-spa-app-registration.md)
