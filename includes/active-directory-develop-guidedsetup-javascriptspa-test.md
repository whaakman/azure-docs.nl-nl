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
ms.openlocfilehash: 494f5fb6f2bfdec86cad01a37e57c3ec219a77f9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133502"
---
## <a name="test-your-code"></a>Uw code testen

Test uw code met behulp van een van de volgende omgevingen.

### <a name="test-with-nodejs"></a>Testen met behulp van Node.js

Als u Visual Studio niet gebruikt, zorg er dan voor dat uw webserver is gestart.

1. Configureer de server om te luisteren naar een TCP-poort die gebaseerd op de locatie van uw *index.html* bestand. Voor Node.js, start u de webserver om te luisteren naar de poort door te voeren van de volgende opdrachten op een opdrachtregelprompt vanuit de map:

    ```bash
    npm install
    node server.js
    ```
1. Voer in uw browser **http://\<span >\</span > localhost:30662** of **http://\<span >\</span > localhost: {poort}** , waar *poort* is de poort die uw webserver luistert. U ziet de inhoud van uw *index.html* bestand en de **aanmelden** knop.

### <a name="test-with-visual-studio"></a>Testen met Visual Studio

Als u Visual Studio, selecteert u de project-oplossing en selecteer vervolgens op F5 om uit te voeren van uw project. De browser wordt geopend op de http://<span></span>localhost: {poort} locatie, en de **aanmelden** knop moet worden weergegeven.

## <a name="test-your-application"></a>Uw toepassing testen

Nadat de browser wordt geladen uw *index.html* -bestand, selecteer **aanmelden**. U wordt gevraagd of u zich aanmelden met het eindpunt van de Microsoft identity-platform:

![Het account aanmelden venster van de JavaScript-SPA](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)

### <a name="provide-consent-for-application-access"></a>Geef toestemming voor toegang tot toepassingen

De eerste keer dat u zich aanmelden bij uw toepassing, moet u gevraagd het toegang geven tot uw profiel en u aanmelden:

![Het venster "Machtigingen aangevraagd"](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Resultaten van de toepassing weergeven

Nadat u zich hebt aangemeld, wordt de gegevens van uw gebruikersprofiel in de Microsoft Graph API-reactie die wordt weergegeven op de pagina geretourneerd.

![Resultaten van de Microsoft Graph API-aanroep](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereiken en gedelegeerde machtigingen

De Microsoft Graph API vereist de *user.read* scope om te lezen van het gebruikersprofiel. Dit bereik wordt automatisch toegevoegd in elke toepassing die geregistreerd op de portal voor wachtwoordregistratie standaard. Andere voor Microsoft Graph-API's, evenals de aangepaste API's voor uw back-endserver, mogelijk extra scopes. Bijvoorbeeld: de Microsoft Graph-API heeft de *Calendars.Read* bereik om agenda's van de gebruiker weer te geven.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing moet toevoegen de *Calendars.Read* overgedragen machtigingen voor de registratiegegevens van de toepassing. Voeg vervolgens de *Calendars.Read* bereik instellen op de `acquireTokenSilent` aanroepen.

>[!NOTE]
>De gebruiker mogelijk voor extra toestemmingen gevraagd als u het aantal bereiken verhogen.

Als een back-end-API niet vereist een bereik (niet aanbevolen), kunt u de *clientId* als het bereik in de aanroepen te verkrijgen van tokens.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
