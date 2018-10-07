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
ms.openlocfilehash: 038ea48bedeb31416627f99b38ebb083846747e4
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843326"
---
## <a name="register-your-application"></a>Uw toepassing registreren

Er zijn meerdere manieren voor het maken van een toepassing, selecteert u een van deze:

### <a name="option-1-register-your-application-express-mode"></a>Optie 1: Registreren van uw toepassing (snelle modus)
Nu moet u uw toepassing registreren in de *Portal voor Appregistratie Microsoft*:

1.  Registreren van uw toepassing via de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure).
2.  Voer een naam voor uw toepassing en uw e-mailadres.
3.  Zorg ervoor dat de optie voor **begeleide installatie** is ingeschakeld.
4.  Volg de instructies voor het verkrijgen van de toepassings-ID en plak deze in uw code.

### <a name="option-2-register-your-application-advanced-mode"></a>Optie 2: Registreer uw toepassing (geavanceerde modus)

1. Ga naar de [Microsoft-portal voor app-registratie](https://apps.dev.microsoft.com/portal/register-app) om een toepassing te registreren.
2. Voer een naam voor uw toepassing en uw e-mailadres.
3. Zorg ervoor dat de optie voor **begeleide installatie** is uitgeschakeld.
4.  Klik op **Platform toevoegen**en selecteer vervolgens **Web**.
5. Voeg de **Omleidings-URL** die overeenkomt met een URL van de toepassing op basis van uw webserver. Zie de secties hieronder voor instructies over het instellen en ophalen van de omleidings-URL in Visual Studio en het knooppunt.
6. Selecteer **Opslaan**.

> #### <a name="setting-redirect-url-for-node"></a>Instelling Omleidings-URL voor knooppunt
> Voor Node.js, kunt u instellen de web server-poort de *server.js* bestand. Deze zelfstudie wordt de poort 30662 voor verwijzing, maar u kunt u een andere poort beschikbaar. In elk geval Volg de onderstaande instructies voor het instellen van een Omleidings-URL in de registratiegegevens van de toepassing:<br/>
> - Ga terug naar de *Portal voor Appregistratie* en stel `http://localhost:30662/` als een `Redirect URL`, of gebruik `http://localhost:[port]/` als u een aangepaste TCP-poort (waarbij *[poort]* is de aangepaste TCP-poort getal) en klik op 'Opslaan'

<p/>

> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Visual Studio-instructies voor het verkrijgen van de omleidings-URL
> Volg deze stappen voor het verkrijgen van de omleidings-URL:
> 1.    In **Solution Explorer**, selecteert u het project en bekijk de **eigenschappen** venster. Als er geen een **eigenschappen** venster, drukt u op **F4**.
> 2.    Kopieer de waarde van **URL** naar het Klembord:<br/> ![Projecteigenschappen](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Ga terug naar de *Portal voor Appregistratie* en plak de waarde als een **Omleidings-URL** en selecteer **opslaan**


#### <a name="configure-your-javascript-spa"></a>Configureren van uw JavaScript-SPA

1.  In de `index.html` bestand gemaakt tijdens het instellen van het project, de registratiegegevens van de toepassing toevoegen. Voeg de volgende code aan de bovenkant binnen de `<script></script>` tags in de hoofdtekst van uw `index.html` bestand:

```javascript
var applicationConfig = {
    clientID: "[Enter the application Id here]",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
<ol start="3">
<li>
Vervang <code>Enter the application Id here</code> met de toepassings-Id die u zojuist hebt geregistreerd.
</li>
</ol>
