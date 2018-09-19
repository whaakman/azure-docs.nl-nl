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
ms.openlocfilehash: 569a7e74df3016fae133066607fdc0bc32c0044d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293656"
---
## <a name="register-your-application"></a>Uw toepassing registreren

Er zijn meerdere manieren voor het maken van een toepassing, selecteert u een van deze:

### <a name="option-1-register-your-application-express-mode"></a>Optie 1: Registreren van uw toepassing (snelle modus)
Nu moet u uw toepassing registreren in de *Portal voor Appregistratie Microsoft*:

1.  Registreren van uw toepassing via de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)
2.  Voer een naam voor uw toepassing en uw e-mailadres
3.  Zorg ervoor dat de optie voor *begeleide installatie* is ingeschakeld
4.  Volg de instructies voor het verkrijgen van de toepassings-ID en plak deze in uw code

### <a name="option-2-register-your-application-advanced-mode"></a>Optie 2: Registreer uw toepassing (geavanceerde modus)

1. Ga naar de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/portal/register-app) een toepassing registreren
2. Voer een naam voor uw toepassing en uw e-mailadres 
3. Zorg ervoor dat de optie voor *begeleide installatie* is uitgeschakeld
4.  Klik op `Add Platform`en selecteer vervolgens `Web`
5. Voeg de `Redirect URL` die overeenkomen met de URL van de toepassing op basis van uw webserver. Zie de secties hieronder voor instructies over het instellen / ophalen van de omleidings-URL in Visual Studio en Python.
6. Klik op *Opslaan*.

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio-instructies voor het verkrijgen van de omleidings-URL
> Volg de instructies voor het verkrijgen van de omleidings-URL:
> 1.    In *Solution Explorer*, selecteert u het project en bekijk de `Properties` venster (als u een venster met eigenschappen niet ziet, drukt u op `F4`)
> 2.    Kopieer de waarde van `URL` naar het Klembord:<br/> ![Projecteigenschappen](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Ga terug naar de *Portal voor Appregistratie* en plak de waarde als een `Redirect URL` en klik op 'Opslaan'

<p/>

> #### <a name="setting-redirect-url-for-node"></a>Instelling Omleidings-URL voor knooppunt
> Voor Node.js, kunt u instellen de web server-poort de *server.js* bestand. Deze zelfstudie wordt de poort 30662 voor verwijzing, maar u kunt u een andere poort beschikbaar. In elk geval Volg de onderstaande instructies voor het instellen van een Omleidings-URL in de registratiegegevens van de toepassing:<br/>
> - Ga terug naar de *Portal voor Appregistratie* en stel `http://localhost:30662/` als een `Redirect URL`, of gebruik `http://localhost:[port]/` als u een aangepaste TCP-poort (waarbij *[poort]* is de aangepaste TCP-poort getal) en klik op 'Opslaan'


#### <a name="configure-your-javascript-spa"></a>Configureren van uw JavaScript-SPA

1.  Maak een bestand met de naam `msalconfig.js` met informatie over voor registratie bij de toepassing. Als u Visual Studio gebruikt, selecteert u het project (project-basismap), klik met de rechtermuisknop en selecteer: `Add`  >  `New Item`  >  `JavaScript File`. Geef het de naam `msalconfig.js`
2.  Voeg de volgende code aan uw `msalconfig.js` bestand:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Vervang <code>Enter_the_Application_Id_here</code> met de toepassings-Id die u zojuist hebt geregistreerd
</li>
</ol>
