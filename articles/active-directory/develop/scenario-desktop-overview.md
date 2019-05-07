---
title: Desktop-app dat aanroepen van web-API's (overzicht) - Microsoft identity-platform
description: Informatie over het bouwen van een bureaublad-app dat aanroepen van web-API's (overzicht)
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
ms.openlocfilehash: 44d31011ca70bbebaf994b5fb80a45eee8dbde40
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076943"
---
# <a name="scenario-desktop-app-that-calls-web-apis"></a>Scenario: Desktop-app of web-API's aanroepen

Leer alles die wat u nodig voor het bouwen van een bureaublad-app die web-API's aanroept

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Aan de slag

Als u niet hebt gedaan, moet u uw eerste toepassing maken door de Snelstartgids voor .NET-desktop of de UWP-snelstartgids te volgen:

> [!div class="nextstepaction"]
> [Snelstart: Een token verkrijgen en Microsoft Graph API aanroepen vanuit een Windows-bureaublad-app](./quickstart-v2-windows-desktop.md)


> [!div class="nextstepaction"]
> [Snelstart: Een token verkrijgen en Microsoft Graph API aanroepen vanuit een UWP-app](./quickstart-v2-uwp.md)

## <a name="overview"></a>Overzicht

U schrijft een bureaubladtoepassing en u wilt dat gebruikers aanmelden bij uw toepassing en het aanroepen van web-API's, zoals de Microsoft Graph, andere Microsoft-APIs of uw eigen web-API. U hebt verschillende mogelijkheden:

- Als uw bureaubladtoepassing grafische besturingselementen ondersteunt, bijvoorbeeld als een toepassing Windows.Form of een WPF-toepassing is, kunt u de interactieve ophalen van tokens.
- Voor toepassingen van Windows die wordt gehost, het is ook mogelijk voor toepassingen die worden uitgevoerd op computers die zijn gekoppeld aan een Windows-domein of AAD zijn toegevoegd voor het verkrijgen van een token op de achtergrond met behulp van geïntegreerde Windows-verificatie.
- Ten slotte, en hoewel dit niet wordt aangeraden, kunt u de gebruikersnaam en wachtwoord gebruiken in openbare clienttoepassingen. Deze nog steeds in sommige scenario's (zoals DevOps) nodig is, maar houd er rekening mee dat het met behulp van deze beperkingen met betrekking tot uw toepassing wordt opleggen. Bijvoorbeeld, aanmelden niet het gebruiker die nodig zijn voor meervoudige verificatie (voor voorwaardelijke toegang). Uw toepassing wordt niet ook profiteren van eenmalige aanmelding (SSO).

  Het is ook op basis van de beginselen van moderne verificatie en is alleen van toepassing op verouderde redenen.

  ![Bureaubladtoepassing](media/scenarios/desktop-app.svg)

- Als u schrijft een draagbare opdrachtregelprogramma - waarschijnlijk een .NET Core-toepassing die wordt uitgevoerd op Linux of Mac - kunt u zich niet aan geen van beide de interactieve verificatie gebruiken (zoals .NET Core biedt geen een [webbrowser](https://aka.ms/msal-net-uses-web-browser)), noch geïntegreerd Windows-verificatie. De beste optie is in dat geval het gebruik van de stroom van apparaat. Deze stroom wordt ook gebruikt voor toepassingen zonder een browser, zoals iOT-toepassingen

  ![Browserless toepassing](media/scenarios/device-code-flow-app.svg)

## <a name="specifics"></a>Specifieke informatie

Bureaubladtoepassingen hebben een aantal specifieke kenmerken, die afhankelijk is vooral van of uw toepassing gebruikmaakt van de interactieve verificatie of niet.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Desktop-app - app-registratie](scenario-desktop-app-registration.md)
