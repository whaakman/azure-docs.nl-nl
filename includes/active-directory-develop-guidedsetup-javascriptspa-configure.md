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
ms.openlocfilehash: 050bae64a62e90bdb74c93948109e255b69d7518
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133738"
---
## <a name="register-your-application"></a>Uw toepassing registreren

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Als uw account u toegang tot meer dan één tenant hebt, selecteert u het account in de rechterbovenhoek en stel uw portal-sessie op de Azure AD-tenant die u wilt gebruiken.
1. Ga naar de Microsoft identity-platform voor ontwikkelaars [App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) pagina.
1. Wanneer de pagina **Een toepassing registreren** wordt weergegeven, voert u een naam in voor de toepassing.
1. Selecteer onder **Ondersteunde accounttypen** de optie **Accounts in een organisatieadreslijst en persoonlijke Microsoft-account**.
1. Onder de **omleidings-URI** sectie in de vervolgkeuzelijst, selecteer de **Web** platform, en stel de waarde in de URL van de toepassing die gebaseerd op uw webserver. 

   Zie de volgende twee secties voor meer informatie over het instellen en het verkrijgen van de omleidings-URL in Visual Studio en Node.js.

1. Selecteer **Registreren**.
1. Op de app **overzicht** pagina, houd er rekening mee de **(client) toepassings-ID** waarde voor later gebruik.
1. Voor deze quickstart moet de [Impliciete toekenningsstroom](../articles/active-directory/develop/v2-oauth2-implicit-grant-flow.md) zijn ingeschakeld. Selecteer in het linkerdeelvenster van de geregistreerde toepassing **verificatie**.
1. In **geavanceerde instellingen**onder **impliciete**, selecteer de **ID-tokens** en **toegangstokens** selectievakjes. ID-tokens en toegangstokens zijn vereist, omdat deze app moet het aanmelden van gebruikers en aanroepen van een API.
1. Selecteer **Opslaan**.

> #### <a name="set-a-redirect-url-for-nodejs"></a>Een Omleidings-URL instellen voor Node.js
> Voor Node.js, kunt u instellen de web server-poort de *server.js* bestand. In deze zelfstudie maakt gebruik van poort 30662 ter referentie, maar u kunt een beschikbare poort gebruiken. 
>
> Als u een Omleidings-URL in de registratiegegevens van de toepassing instelt, gaat u terug naar de **Toepassingsregistratie** in het deelvenster en een van de volgende handelingen uit:
>
> - Stel *`http://localhost:30662/`* als de **Omleidings-URL**.
> - Als u een aangepaste TCP-poort, gebruikt u *`http://localhost:<port>/`* (waarbij  *\<poort >* is het nummer van de aangepaste TCP-poort).
>
> #### <a name="set-a-redirect-url-for-visual-studio"></a>Een Omleidings-URL instellen voor Visual Studio
> Als u de omleidings-URL voor Visual Studio, het volgende doen:
> 1. In **Solution Explorer**, selecteert u het project.
>
>    De **eigenschappen** venster wordt geopend. Als deze niet wordt geopend, drukt u op **F4**.
>
>    ![Het venster Eigenschappen voor JavaScriptSPA-Project](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)
>
> 1. Kopieer de **URL** waarde.
 
> 1. Ga terug naar de **Toepassingsregistratie** in het deelvenster en plak de gekopieerde waarde als de **Omleidings-URL**.

#### <a name="configure-your-javascript-spa"></a>Configureren van uw JavaScript-SPA

1. In de *index.html* -bestand dat u hebt gemaakt tijdens het instellen van het project, de registratiegegevens van de toepassing toevoegen. Aan de bovenkant van het bestand, vanuit de `<script></script>` tags, voeg de volgende code toe:

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "<Enter_the_Application_Id_here>",
            authority: "https://login.microsoftonline.com/<Enter_the_Tenant_info_here>"
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

    Waar:
    - *\<Enter_the_Application_Id_here >* is de **(client) toepassings-ID** voor de toepassing die u hebt geregistreerd.
    - *\<Enter_the_Tenant_info_here >* is ingesteld op een van de volgende opties:
       - Als uw toepassing ondersteunt *Accounts in deze organisatie-map*, vervang deze waarde met de **Tenant-ID** of **tenantnaam** (bijvoorbeeld  *Contoso.Microsoft.com*).
       - Als uw toepassing ondersteunt *Accounts in een organisatie-map*, vervang deze waarde met **organisaties**.
       - Als uw toepassing ondersteunt *Accounts in een organisatie-map en de persoonlijke Microsoft-accounts*, vervang deze waarde met **algemene**. De ondersteuning om te beperken *persoonlijke Microsoft-accounts alleen*, vervang deze waarde met **consumenten**.
