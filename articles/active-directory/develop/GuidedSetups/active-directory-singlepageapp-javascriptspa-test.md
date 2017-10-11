---
title: Azure AD v2 JS SPA begeleide Setup - Test | Microsoft Docs
description: Hoe een API waarvoor toegangstokens door Azure Active Directory-v2-eindpunt kunnen aanroepen in JavaScript SPA-toepassingen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: c888760ab311e8ac08b1e625bb837f91047db645
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
## <a name="test-your-code"></a>Testen van uw code

> ### <a name="testing-with-visual-studio"></a>Testen met Visual Studio
> Als u van Visual Studio gebruikmaakt, drukt u op `F5` aan uw project uitvoeren: de browser wordt geopend en wordt u verwezen *http://localhost: {poort}* waarin u zien hoe de *Microsoft Graph-API aanroepen* knop.

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a>Testen met Python of een andere webserver
> Als u Visual Studio niet gebruikt, zorg er dan de webserver wordt gestart en deze is geconfigureerd om te luisteren naar een TCP-poort op basis van de map met uw *index.html* bestand. Voor Python, u kunt beginnen met luisteren op poort door het uitvoeren van de in de opdracht vragen / terminal, vanuit de map van de app:
> 
> ```bash
> python -m http.server 8080
> ```
>  Open de browser en typ vervolgens *http://localhost: 8080* of *http://localhost: {poort}* - waar de *poort* komt overeen met de poort die uw webserver luistert. Ziet u de inhoud van uw pagina index.html met de *Microsoft Graph-API aanroepen* knop.

## <a name="test-your-application"></a>Uw toepassing testen

Nadat de browser wordt geladen uw *index.html*, klikt u op de *Microsoft Graph-API aanroepen* knop. Als dit de eerste keer, de browser wordt doorgestuurd naar het Microsoft Azure Active Directory v2 eindpunt, waarbij u wordt gevraagd aan te melden.
 
![Schermopname van een steekproef](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a>Toestemming
De eerste keer dat u zich aanmeldt bij uw toepassing krijgt u een toestemming van de volgende strekking scherm waarin u wilt accepteren:

 ![Toestemming scherm](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a>Verwachte resultaten
U ziet gebruikersprofielgegevens geretourneerd door de reactie van Microsoft Graph API-aanroep.
 
 ![Resultaten](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

Zie ook basisinformatie over het token dat is verkregen de *Access Token* en *Token ID-Claims* vakken.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereikwaarden en gedelegeerde machtigingen

De Microsoft Graph API vereist de `user.read` bereik van het gebruikersprofiel lezen. Deze scope wordt automatisch toegevoegd standaard in elke toepassing die wordt geregistreerd op onze portal voor wachtwoordregistratie. Sommige andere API's voor Microsoft Graph evenals aangepaste API's voor uw back-endserver moet mogelijk extra scopes. Bijvoorbeeld: voor het bereik van Microsoft Graph `Calendars.Read` is vereist voor een lijst met agenda's van de gebruiker. Voor toegang tot de agenda van de gebruiker in de context van een toepassing, moet u toevoegen de `Calendars.Read` overgedragen machtigingen voor de toepassingsregistratie-informatie en voeg vervolgens de `Calendars.Read` bereik voor de `acquireTokenSilent` aanroepen. De gebruiker kan gevraagd om extra toestemmingen als u het aantal scopes verhogen.

Als u een back-end API niet vereist een bereik (niet aanbevolen), kunt u de `clientId` als het bereik in de `acquireTokenSilent` en/of `acquireTokenRedirect` aanroepen.

<!--end-collapse-->
