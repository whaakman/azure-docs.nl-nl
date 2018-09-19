---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 9817e94ba77c83b8620274ba41f9d862b6a5ce6d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293524"
---
## <a name="test-your-code"></a>Testen van uw code

### <a name="test-with-visual-studio"></a>Testen met Visual Studio
Als u Visual Studio, drukt u op **F5** om uit te voeren van uw project. De browser wordt geopend op de http://<span></span>localhost: {poort} locatie en u ziet de **Microsoft Graph-API aanroepen** knop.

<p/><!-- -->

### <a name="test-with-node-or-other-web-server"></a>Test met knooppunt- of andere webserver
Als u Visual Studio niet gebruikt, zorg er dan voor dat uw webserver is gestart. Configureer de server om te luisteren naar een TCP-poort die gebaseerd op de locatie van uw **index.html** bestand. Start de server om te luisteren naar de poort door te voeren van de volgende opdrachten op de opdrachtregel vanuit de map voor knooppunt:

```bash
npm install
node server.js
```
Open de browser en typt u http://<span></span>localhost:30662 of http://<span></span>localhost: {poort} waar **poort** is de poort die uw webserver luistert. U ziet de inhoud van uw index.html-bestand en de **Microsoft Graph-API aanroepen** knop.

## <a name="test-your-application"></a>Uw toepassing testen

Nadat uw index.html-bestand in de browser geladen, selecteert u **Microsoft Graph-API aanroepen**. De eerste keer is dat u de toepassing uitvoert de browser wordt u omgeleid naar het v2.0-eindpunt voor Microsoft Azure Active Directory (Azure AD) en u wordt gevraagd aan te melden bij:

![Aanmelden bij uw account beveiligd-WACHTWOORDVERIFICATIE JavaScript](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="provide-consent-for-application-access"></a>Geef toestemming voor toegang tot toepassingen

De eerste keer dat u zich aanmelden bij uw toepassing, bent u gevraagd te geven van toestemming van de toepassing om toegang tot uw profiel en u zich aanmeldt:

![Geef uw toestemming voor toegang tot toepassingen](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Resultaten van de toepassing weergeven
Nadat u zich hebt aangemeld, ziet u de gegevens van uw gebruikersprofiel in de **Graph API-aanroepen reactie** vak.
 
![Verwachte resultaten van Microsoft Graph API-aanroep](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

U ziet ook basisinformatie over het token dat is verkregen in de **toegangstoken** en **Token-ID-Claims** vakken.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereiken en gedelegeerde machtigingen

De Microsoft Graph API vereist de **user.read** scope om te lezen van het gebruikersprofiel. Dit bereik wordt automatisch toegevoegd in elke toepassing die geregistreerd op de portal voor wachtwoordregistratie standaard. Andere voor Microsoft Graph-API's, evenals de aangepaste API's voor uw back-endserver, mogelijk extra scopes. De Microsoft Graph API vereist de **Calendars.Read** bereik om agenda's van de gebruiker weer te geven.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing moet toevoegen de **Calendars.Read** overgedragen machtigingen voor de registratiegegevens van de toepassing. Voeg vervolgens de **Calendars.Read** bereik instellen op de **acquireTokenSilent** aanroepen. 

>[!NOTE]
>De gebruiker mogelijk voor extra toestemmingen gevraagd als u het aantal bereiken verhogen.

Als een back-end-API niet vereist een bereik (niet aanbevolen), kunt u de **clientId** als het bereik in de **acquireTokenSilent** en **acquireTokenRedirect** aanroepen.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
