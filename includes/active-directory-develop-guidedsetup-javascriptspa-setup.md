---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: d7b78519f1e81fed1f74b6f3a2fd2720cd28cce4
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843480"
---
## <a name="setting-up-your-web-server-or-project"></a>Instellen van uw webserver of project

> Voorkeur voor het downloaden van dit voorbeeldproject in plaats daarvan?
> - [Download de projectbestanden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) voor een lokale webserver, zoals knooppunt
>
> of
> - [Het Visual Studio-project downloaden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)
>
> En ga vervolgens verder met de [configuratiestap](#register-your-application) configureren in het codevoorbeeld voordat deze wordt uitgevoerd.

## <a name="prerequisites"></a>Vereisten
Een lokale webserver zoals [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core), of integratie met IIS Express [Visual Studio 2017](https://www.visualstudio.com/downloads/) is vereist voor het uitvoeren van deze zelfstudie.

Instructies in deze handleiding zijn gebaseerd op Node.js en Visual Studio 2017, maar gebruik een andere ontwikkelomgeving of webserver.

## <a name="create-your-project"></a>Uw project maken

> ### <a name="option-1-node-other-web-servers"></a>Optie 1: Knooppunt / andere webservers
> Zorg ervoor dat u hebt geïnstalleerd [Node.js](https://nodejs.org/en/download/), volg de onderstaande stappen:
> - Maak een map voor het hosten van uw toepassing.

<p/><!-- -->

> ### <a name="option-2-visual-studio"></a>Optie 2: Visual Studio
> Als u met behulp van Visual Studio en een nieuw project maakt, volgt u de stappen hieronder om een nieuwe Visual Studio-oplossing maken:
> 1.    In Visual Studio: **bestand > Nieuw > Project**
> 2.    Onder **Visual C# \Web**, selecteer **ASP.NET-webtoepassing (.NET Framework)**
> 3.    Voer een naam voor uw toepassing en selecteer **OK**
> 4.    Onder **nieuwe ASP.NET-webtoepassing**, selecteer **leeg zijn**


## <a name="create-your-single-page-applications-ui"></a>Maken van de gebruikersinterface van uw toepassing met één pagina
1.  Maak een `index.html` -bestand voor uw JavaScript beveiligd-WACHTWOORDVERIFICATIE. Als u van Visual Studio gebruikmaakt, selecteer het project (project-basismap), klik met de rechtermuisknop en selecteer: **toevoegen > Nieuw Item > HTML-pagina** en noem het index.html.

2.  Voeg de volgende code naar uw pagina:
```html
<!DOCTYPE html>
<html>
<head>
        <title>Quickstart for MSAL JS</title>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
        <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.3/js/msal.js"></script>
        <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
</head>
<body>
        <h2>Welcome to MSAL.js Quickstart</h2><br/>
        <h4 id="WelcomeMessage"></h4>
        <button id="SignIn" onclick="signIn()">Sign In</button><br/><br/>
        <pre id="json"></pre>
        <script>
            //JS code
        </script>
</body>
</html>
```
