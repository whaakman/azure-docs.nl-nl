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
ms.openlocfilehash: eead4c6a66a317c7404205415cbf04c442ffe8d1
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060717"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>De registratiegegevens van de toepassing toevoegen aan uw App

In deze stap moet u de omleidings-URL van de registratie-informatie van uw toepassing configureren en vervolgens de toepassings-ID toevoegen aan uw JavaScript-SPA-toepassing.

### <a name="configure-redirect-url"></a>Omleidings-URL configureren

Configureer de `Redirect URL` veld met de URL voor uw index.html-pagina op basis van uw webserver en klik vervolgens op *Update*.


> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Visual Studio-instructies voor het verkrijgen van de omleidings-URL
> Volg deze stappen voor het verkrijgen van de omleidings-URL:
> 1.    In **Solution Explorer**, selecteert u het project en bekijk de **eigenschappen** venster. Als er geen een **eigenschappen** venster, drukt u op **F4**.
> 2.    Kopieer de waarde van **URL** naar het Klembord:<br/> ![Projecteigenschappen](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Plak de waarde als een **Omleidings-URL** boven aan deze pagina, klikt u vervolgens op **Update**

<p/>

> #### <a name="setting-redirect-url-for-node"></a>Instelling Omleidings-URL voor knooppunt
> Voor Node.js, kunt u instellen de web server-poort de *server.js* bestand. In deze zelfstudie wordt de poort 30662 ter referentie, maar kunt u een beschikbare poort. Volg de onderstaande instructies voor het instellen van een Omleidings-URL in de registratiegegevens van de toepassing:<br/>
> Stel `http://localhost:30662/` als een **Omleidings-URL** boven aan deze pagina, of gebruik `http://localhost:[port]/` als u een aangepaste TCP-poort (waar *[poort]* is de aangepaste TCP-poortnummer) en klik vervolgens op  **Update**

### <a name="configure-your-javascript-spa-application"></a>Configureren van uw toepassing JavaScript-SPA

1.  In de `index.html` bestand gemaakt tijdens het instellen van het project, de registratiegegevens van de toepassing toevoegen. Voeg de volgende code aan de bovenkant binnen de `<script></script>` tags in de hoofdtekst van uw `index.html` bestand:

```javascript
var applicationConfig = {
    clientID: "[Enter the application Id here]",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
