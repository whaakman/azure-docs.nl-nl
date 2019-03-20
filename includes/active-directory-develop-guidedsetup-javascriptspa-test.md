---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: bb73f58c5dca5e49cdb075b046f883ffeb77c95c
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203291"
---
## <a name="test-your-code"></a>Uw code testen

### <a name="test-with-node"></a>Testen met Node

Als u Visual Studio niet gebruikt, zorg er dan voor dat uw webserver is gestart.

1. Configureer de server om te luisteren naar een TCP-poort die gebaseerd op de locatie van uw **index.html** bestand. Start de server om te luisteren naar de poort door te voeren van de volgende opdrachten op de opdrachtregel vanuit de map voor knooppunt:

    ```bash
    npm install
    node server.js
    ```
1. Open de browser en typt u http://<span></span>localhost:30662 of http://<span></span>localhost: {poort} waar **poort** is de poort die uw webserver luistert. U ziet de inhoud van uw index.html-bestand en de **aanmelden** knop.

<p/><!-- -->

### <a name="test-with-visual-studio"></a>Testen met Visual Studio

Als u Visual Studio, zorg ervoor dat u selecteert de project-oplossing en druk op **F5** om uit te voeren van uw project. De browser wordt geopend op de http://<span></span>localhost: {poort} locatie en u ziet de **aanmelden** knop.

## <a name="test-your-application"></a>Uw toepassing testen

Nadat uw index.html-bestand in de browser geladen, klikt u op **aanmelden**. U wordt gevraagd zich aanmelden met het v2.0-eindpunt voor Microsoft Azure Active Directory (Azure AD):

![Aanmelden bij uw account beveiligd-WACHTWOORDVERIFICATIE JavaScript](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)

### <a name="provide-consent-for-application-access"></a>Geef toestemming voor toegang tot toepassingen

De eerste keer dat u zich aanmelden bij uw toepassing, bent u gevraagd te geven van toestemming van de toepassing om toegang tot uw profiel en u zich aanmeldt:

![Geef uw toestemming voor toegang tot toepassingen](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Resultaten van de toepassing weergeven

Nadat u zich aanmeldt, u ziet gegevens van uw gebruikersprofiel geretourneerd in het Microsoft Graph API-antwoord weergegeven op de pagina zijn.

![Verwachte resultaten van Microsoft Graph API-aanroep](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereiken en gedelegeerde machtigingen

De Microsoft Graph API vereist de `user.read` scope om te lezen van het gebruikersprofiel. Dit bereik wordt automatisch toegevoegd in elke toepassing die geregistreerd op de portal voor wachtwoordregistratie standaard. Andere voor Microsoft Graph-API's, evenals de aangepaste API's voor uw back-endserver, mogelijk extra scopes. Bijvoorbeeld: de Microsoft Graph-API heeft de `Calendars.Read` bereik om agenda's van de gebruiker weer te geven.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing moet toevoegen de `Calendars.Read` overgedragen machtigingen voor de registratiegegevens van de toepassing. Voeg vervolgens de `Calendars.Read` bereik instellen op de `acquireTokenSilent` aanroepen.

>[!NOTE]
>De gebruiker mogelijk voor extra toestemmingen gevraagd als u het aantal bereiken verhogen.

Als een back-end-API niet vereist een bereik (niet aanbevolen), kunt u de `clientId` als het bereik in de aanroepen te verkrijgen van tokens.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]