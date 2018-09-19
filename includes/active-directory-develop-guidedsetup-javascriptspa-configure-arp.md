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
ms.openlocfilehash: fc06da3b1ad66aa15237a25d2f50374043c860ba
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293634"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>De registratiegegevens van de toepassing toevoegen aan uw App

In deze stap moet u de omleidings-URL van de registratie-informatie van uw toepassing configureren en vervolgens de toepassings-ID toevoegen aan uw JavaScript-SPA-toepassing.

### <a name="configure-redirect-url"></a>Omleidings-URL configureren

Configureer de `Redirect URL` veld met de URL voor uw index.html-pagina op basis van uw webserver en klik vervolgens op *Update*.


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio-instructies voor het verkrijgen van de omleidings-URL
> Als u de omleidings-URL:
> 1.    In *Solution Explorer*, selecteert u het project en bekijk de `Properties` venster (als u een venster met eigenschappen niet ziet, drukt u op `F4`)
> 2.    Kopieer de waarde van `URL` naar het Klembord:<br/> ![Projecteigenschappen](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Plak de waarde als een `Redirect URL` boven aan deze pagina, klikt u vervolgens op `Update`

<p/>

> #### <a name="setting-redirect-url-for-node"></a>Instelling Omleidings-URL voor knooppunt
> Voor Node.js, kunt u instellen de web server-poort de *server.js* bestand. Deze zelfstudie wordt de poort 30662 voor verwijzing, maar u kunt u een andere poort beschikbaar. In elk geval, gebruik de volgende instructies voor het instellen van een Omleidings-URL in de registratiegegevens van de toepassing:<br/>
> Stel `http://localhost:30662/` als een `Redirect URL` boven aan deze pagina, of gebruik `http://localhost:[port]/` als u een aangepaste TCP-poort (waar *[poort]* is de aangepaste TCP-poortnummer) en klik vervolgens op 'Update'

### <a name="configure-your-javascript-spa-application"></a>Configureren van uw toepassing JavaScript-SPA

1.  Maak een bestand met de naam `msalconfig.js` met informatie over voor registratie bij de toepassing. Als u Visual Studio gebruikt, selecteert u het project (project-basismap), klik met de rechtermuisknop en selecteer: `Add`  >  `New Item`  >  `JavaScript File`. Geef het de naam `msalconfig.js`
2.  Voeg de volgende code aan uw `msalconfig.js` bestand:

```javascript
var msalconfig = {
    clientID: "[Enter the application Id here]",
    redirectUri: location.origin
};
``` 
