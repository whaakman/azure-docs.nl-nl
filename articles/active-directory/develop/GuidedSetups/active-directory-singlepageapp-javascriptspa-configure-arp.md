---
title: Azure AD v2 JS SPA begeleide Setup - configureren (ARP) | Microsoft Docs
description: Hoe een API waarvoor toegangstokens door Azure Active Directory v2-eindpunt (ARP) kunnen aanroepen in JavaScript SPA-toepassingen
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
ms.openlocfilehash: 708f4ff606d79639de979918a9cacd4ed75db311
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
## <a name="add-the-applications-registration-information-to-your-app"></a>De registratiegegevens van de toepassing toevoegen aan uw App

In deze stap moet u de omleidings-URL van de registratiegegevens van de toepassing configureren en vervolgens de toepassings-Id toe te voegen aan uw toepassing JavaScript SPA.

### <a name="configure-redirect-url"></a>Omleidings-URL configureren

Configureer de `Redirect URL` veld boven aan de URL voor uw pagina index.html op basis van uw webserver en klik vervolgens op *Update*.


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio-instructies voor het verkrijgen van de omleidings-URL
> Als u de omleidings-URL, volgt u onderstaande instructies:
> 1.    In *Solution Explorer*, selecteert u het project en bekijk de `Properties` venster (als u een venster met eigenschappen niet ziet, drukt u op `F4`)
> 2.    Kopieer de waarde van `URL` naar het Klembord:<br/> ![Projecteigenschappen](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Plak de waarde als een `Redirect URL` Klik boven aan deze pagina`Update`

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Instelling Omleidings-URL voor Python
> Voor Python, kunt u de web server-poort via de opdrachtregel instellen. Deze begeleide setup gebruikt de poort 8080 voor verwijzing, maar gerust om andere poort beschikbaar te gebruiken. In elk geval Volg de onderstaande instructies voor het instellen van een Omleidings-URL in de registratiegegevens van toepassing:<br/>
> Stel `http://localhost:8080/` als een `Redirect URL` boven aan deze pagina of gebruik `http://localhost:[port]/` als u een aangepaste TCP-poort (waar *[poort]* het aangepaste TCP-poortnummer), en klik op 'Bijwerken'

### <a name="configure-your-javascript-spa-application"></a>Uw toepassing JavaScript SPA configureren

1.  Maak een bestand met de naam `msalconfig.js` met de registratiegegevens van de toepassing. Als u Visual Studio gebruikt, selecteert u het project (basismap project), klik met de rechtermuisknop en selecteer: `Add`  >  `New Item`  >  `JavaScript File`. Geef deze de naam`msalconfig.js`
2.  Voeg de volgende code naar uw `msalconfig.js` bestand:

```javascript
var msalconfig = {
    clientID: "[Enter the application Id here]",
    redirectUri: location.origin
};
``` 
