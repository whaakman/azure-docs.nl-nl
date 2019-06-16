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
ms.openlocfilehash: 5ce0f18c1ec7a0fcb6465ab20e774976552687f1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133230"
---
## <a name="set-up-your-web-server-or-project"></a>Instellen van uw webserver of project

> Voorkeur voor het downloaden van dit voorbeeldproject in plaats daarvan? Voer een van de volgende bewerkingen uit:
> 
> - Het project wilt uitvoeren met een lokale webserver, zoals Node.js, [downloaden van de projectbestanden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip).
>
> - (Optioneel) Het project wilt uitvoeren met de IIS-server, [downloaden van het Visual Studio-project](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip).
>
> En, als u wilt configureren in het codevoorbeeld voordat u deze uitvoert, gaat u naar de [configuratiestap](#register-your-application).

## <a name="prerequisites"></a>Vereisten

* Als u wilt uitvoeren in deze zelfstudie, moet u een lokale webserver, zoals [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core), of integratie met IIS Express [Visual Studio 2017](https://www.visualstudio.com/downloads/).

* Als u Node.js gebruikt om uit te voeren van het project, installeert u een geïntegreerde ontwikkelomgeving (IDE), zoals [Visual Studio Code](https://code.visualstudio.com/download), de projectbestanden te bewerken.

* Instructies in deze handleiding zijn gebaseerd op Node.js en Visual Studio 2017, maar u kunt een andere ontwikkelomgeving of webserver.

## <a name="create-your-project"></a>Uw project maken

> ### <a name="option-1-nodejs-or-other-web-servers"></a>Optie 1: Node.js- of andere webservers
> Zorg ervoor dat u hebt geïnstalleerd [Node.js](https://nodejs.org/en/download/), en vervolgens het volgende doen:
> - Maak een map voor het hosten van uw toepassing.
>
> ### <a name="option-2-visual-studio"></a>Optie 2: Visual Studio
> Als u Visual Studio en een nieuw project maakt, het volgende doen:
> 1. Selecteer in Visual Studio **Bestand** > **Nieuw** > **Project**.
> 1. Selecteer onder **Visual C#\Web** de optie **ASP.NET-webtoepassing (.NET Framework)** .
> 1. Voer een naam voor uw toepassing en selecteer vervolgens **OK**.
> 1. Onder **nieuwe ASP.NET-webtoepassing**, selecteer **leeg**.

## <a name="create-the-spa-ui"></a>De beveiligd-WACHTWOORDVERIFICATIE-gebruikersinterface maken
1. Maak een *index.html* -bestand voor uw JavaScript beveiligd-WACHTWOORDVERIFICATIE. Als u Visual Studio gebruikt, selecteert u het project (project-basismap), klik met de rechtermuisknop en selecteer **toevoegen** > **Nieuw Item** > **HTML-pagina**, en Noem het bestand *index.html*.

1. In de *index.html* bestand, voeg de volgende code toe:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Quickstart for MSAL JS</title>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
       <script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.js"></script>
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

   > [!TIP]
   > U kunt de versie van MSAL.js in het vorige script vervangen door de meest recente releaseversie onder [MSAL.js releases](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases).
