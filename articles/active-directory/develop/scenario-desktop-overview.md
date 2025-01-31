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
ms.openlocfilehash: 476703b52813e6b3081dcfb3ab5a2fb4f3a7bfc5
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67785641"
---
# <a name="scenario-desktop-app-that-calls-web-apis"></a>Scenario: Desktop-app die web-API's aanroept

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

- U kunt het ophalen van interactieve tokens gebruiken:

  - Als uw bureaubladtoepassing grafische besturingselementen ondersteunt, bijvoorbeeld als het een Windows.Form toepassing of een WPF-toepassing.
  - Van dat als het is een .NET Core-toepassing en u gaat akkoord met de verificatie-interactie met Azure AD gebeuren in de browser system

- Voor toepassingen van Windows die wordt gehost, het is ook mogelijk voor toepassingen die worden uitgevoerd op computers die zijn gekoppeld aan een Windows-domein of AAD zijn toegevoegd voor het verkrijgen van een token op de achtergrond met behulp van geïntegreerde Windows-verificatie.
- Ten slotte, en hoewel dit niet wordt aangeraden, kunt u de gebruikersnaam en wachtwoord gebruiken in openbare clienttoepassingen. Deze nog steeds in sommige scenario's (zoals DevOps) nodig is, maar houd er rekening mee dat het met behulp van deze beperkingen met betrekking tot uw toepassing wordt opleggen. Bijvoorbeeld, aanmelden niet het gebruiker die nodig zijn voor meervoudige verificatie (voor voorwaardelijke toegang). Uw toepassing wordt niet ook profiteren van eenmalige aanmelding (SSO).

  Het is ook op basis van de beginselen van moderne verificatie en is alleen van toepassing op verouderde redenen.

  ![Bureaubladtoepassing](media/scenarios/desktop-app.svg)

- Als u bij het schrijven van een draagbare opdrachtregelprogramma - waarschijnlijk een .NET Core-toepassing die wordt uitgevoerd op Linux of Mac - en gaat u akkoord met dat de verificatie worden gedelegeerd naar de browser van het systeem, kunt u zich voor interactieve verificatie. (.NET core biedt geen nog een [webbrowser](https://aka.ms/msal-net-uses-web-browser) en daarom de verificatie plaatsvindt in de browser system), anders wordt de beste optie in dat geval is het gebruik van de stroom van apparaat. Deze stroom wordt ook gebruikt voor toepassingen zonder een browser, zoals IoT-toepassingen

  ![Browserless toepassing](media/scenarios/device-code-flow-app.svg)

## <a name="specifics"></a>Specifieke informatie

Bureaubladtoepassingen hebben een aantal specifieke kenmerken, die afhankelijk is vooral van of uw toepassing gebruikmaakt van de interactieve verificatie of niet.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Desktop-app - app-registratie](scenario-desktop-app-registration.md)
