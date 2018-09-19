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
ms.openlocfilehash: 724166d402f81fa3a2c977d107111f5a0c32571d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293645"
---
## <a name="setting-up-your-web-server-or-project"></a>Instellen van uw webserver of project

> Voorkeur voor het downloaden van dit voorbeeldproject in plaats daarvan?
> - [Het Visual Studio-project downloaden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)
>
> of
> - [Download de projectbestanden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) voor een lokale webserver, zoals knooppunt
>
> En ga vervolgens verder met de [configuratiestap](#register-your-application) configureren in het codevoorbeeld voordat deze wordt uitgevoerd.

## <a name="prerequisites"></a>Vereisten
Een lokale webserver zoals [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core), of integratie met IIS Express [Visual Studio 2017](https://www.visualstudio.com/downloads/) is vereist voor het uitvoeren van deze zelfstudie.

Instructies in deze handleiding zijn gebaseerd op Node.js en Visual Studio 2017, maar gebruik een andere ontwikkelomgeving of webserver.

## <a name="create-your-project"></a>Uw project maken

> ### <a name="option-1-visual-studio"></a>Optie 1: Visual Studio
> Als u met behulp van Visual Studio en een nieuw project maakt, volgt u de stappen hieronder om een nieuwe Visual Studio-oplossing maken:
> 1.    In Visual Studio:  `File` > `New` > `Project`
> 2.    Onder `Visual C#\Web`, selecteer `ASP.NET Web Application (.NET Framework)`
> 3.    Geef uw toepassing en klikt u op *OK*
> 4.    Onder `New ASP.NET Web Application`, selecteer `Empty`

<p/><!-- -->

> ### <a name="option-2-node-other-web-servers"></a>Optie 2: Knooppunt / andere webservers
> Zorg ervoor dat u hebt geïnstalleerd [Node.js](https://nodejs.org/en/download/), volg de onderstaande stappen:
> - Maak een map voor het hosten van uw toepassing.


## <a name="create-your-single-page-applications-ui"></a>Maken van de gebruikersinterface van uw toepassing met één pagina
1.  Maak een *index.html* -bestand voor uw JavaScript beveiligd-WACHTWOORDVERIFICATIE. Als u van Visual Studio gebruikmaakt, selecteer het project (project-basismap), klik met de rechtermuisknop en selecteer: `Add`  >  `New Item`  >  `HTML page` en noem het index.html
2.  Voeg de volgende code naar uw pagina:
```html
<!DOCTYPE html>
<html>
<head>
    <!-- bootstrap reference used for styling the page -->
    <link href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <title>JavaScript SPA Guided Setup</title>
</head>
<body style="margin: 40px">
    <button id="callGraphButton" type="button" class="btn btn-primary" onclick="callGraphApi()">Call Microsoft Graph API</button>
    <div id="errorMessage" class="text-danger"></div>
    <div class="hidden">
        <h3>Graph API Call Response</h3>
        <pre class="well" id="graphResponse"></pre>
    </div>
    <div class="hidden">
        <h3>Access Token</h3>
        <pre class="well" id="accessToken"></pre>
    </div>
    <div class="hidden">
        <h3>ID Token Claims</h3>
        <pre class="well" id="userInfo"></pre>
    </div>
    <button id="signOutButton" type="button" class="btn btn-primary hidden" onclick="signOut()">Sign out</button>

    <!-- This app uses cdn to reference msal.js (recommended). 
         You can also download it from: https://github.com/AzureAD/microsoft-authentication-library-for-js -->
    <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.1.3/js/msal.min.js"></script>

    <!-- The 'bluebird' and 'fetch' references below are required if you need to run this application on Internet Explorer -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.3/fetch.min.js"></script>

    <script type="text/javascript" src="msalconfig.js"></script>
    <script type="text/javascript" src="app.js"></script>
</body>
</html>
````
