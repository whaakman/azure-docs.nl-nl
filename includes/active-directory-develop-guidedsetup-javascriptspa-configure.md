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
ms.openlocfilehash: 2e65c5f0d6c2d493abcd61bea4077cbdd8ff95fa
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52330914"
---
## <a name="register-your-application"></a>Uw toepassing registreren

Er zijn meerdere manieren om een toepassing te registreren. Selecteer de optie die het beste past bij uw behoeften:
* [Express-modus: de Snelstartgids voor beveiligd-WACHTWOORDVERIFICATIE gebruiken voor de app configureren](#option-1-register-your-application-express-mode)
* [Geavanceerde modus - de app-instellingen handmatig configureren](#option-2-register-your-application-advanced-mode)

### <a name="option-1-register-your-application-express-mode"></a>Optie 1: Registreren van uw toepassing (snelle modus)

1. Aanmelden bij de [Azure portal app-registratie (preview)](https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) om een toepassing te registreren.
1. Op de **registreren van een toepassing** pagina, typ een naam voor uw toepassing.
1. Onder **ondersteund accounttypen**, selecteer **Accounts in een organisatie-map en de persoonlijke Microsoft-accounts**.
1. Selecteer **Registreren** wanneer u klaar bent.
1. Volg de instructies van de Snelstartgids om te downloaden en uw nieuwe toepassing voor u het automatisch configureren in één klik.

### <a name="option-2-register-your-application-advanced-mode"></a>Optie 2: Registreer uw toepassing (geavanceerde modus)

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) om een toepassing te registreren.
1. Als u via uw account toegang hebt tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en stelt u de portalsessie in op de gewenste Azure Active Directory-tenant.
1. Selecteer in het linkernavigatiedeelvenster de **Azure Active Directory**-service en selecteer vervolgens **App-registraties (preview) > Nieuwe registratie**.
1. Wanneer de **registreren van een toepassing** pagina wordt weergegeven, voer een naam in voor uw toepassing.
1. Onder **ondersteund accounttypen**, selecteer **Accounts in een organisatie-map en de persoonlijke Microsoft-accounts**.
1. Onder de **omleidings-URI** sectie, selecteer de **Web** platform en stel de waarde in de URL van de toepassing op basis van uw webserver. Zie de secties hieronder voor instructies over het instellen en ophalen van de omleidings-URL in Visual Studio en het knooppunt.
1. Selecteer **Registreren** wanneer u klaar bent.
1. Op de app **overzicht** pagina, noteer de **(client) toepassings-ID** waarde.
1. Deze snelstartgids moet de [impliciet verlenen stroom](../articles/active-directory/develop/v2-oauth2-implicit-grant-flow.md) zijn ingeschakeld. Selecteer in het navigatiedeelvenster links van de geregistreerde toepassing, **verificatie**.
1. In **geavanceerde instellingen**onder **impliciete**, schakel beide **ID-tokens** en **toegangstokens** selectievakjes. ID-tokens en toegangstokens zijn vereist, omdat deze app moet het aanmelden van gebruikers en aanroepen van een API.
1. Selecteer **Opslaan**.

> #### <a name="setting-the-redirect-url-for-node"></a>De omleidings-URL voor knooppunt instellen
> Voor Node.js, kunt u instellen de web server-poort de *server.js* bestand. In deze zelfstudie wordt de poort 30662 ter referentie, maar kunt u een beschikbare poort. Volg de onderstaande instructies voor het instellen van een Omleidings-URL in de registratiegegevens van de toepassing:<br/>
> - Ga terug naar de *Toepassingsregistratie* en stel `http://localhost:30662/` als een `Redirect URL`, of gebruik `http://localhost:[port]/` als u een aangepaste TCP-poort (waar *[poort]* is de aangepaste TCP-poortnummer).

<p/>

> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Visual Studio-instructies voor het verkrijgen van de omleidings-URL
> Volg deze stappen voor het verkrijgen van de omleidings-URL:
> 1. In **Solution Explorer**, selecteert u het project en bekijk de **eigenschappen** venster. Als er geen een **eigenschappen** venster, drukt u op **F4**.
> 2. Kopieer de waarde van **URL** naar het Klembord:<br/> ![Projecteigenschappen](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3. Ga terug naar de *Toepassingsregistratie* en stel de waarde als een **Omleidings-URL**.

#### <a name="configure-your-javascript-spa"></a>Configureren van uw JavaScript-SPA

1. In de `index.html` bestand gemaakt tijdens het instellen van het project, de registratiegegevens van de toepassing toevoegen. Voeg de volgende code aan de bovenkant binnen de `<script></script>` tags in de hoofdtekst van uw `index.html` bestand:

    ```javascript
    var applicationConfig = {
        clientID: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/common",
        graphScopes: ["user.read"],
        graphEndpoint: "https://graph.microsoft.com/v1.0/me"
    };
    ```

<ol start="2">
<li>
Vervang <code>Enter the application Id here</code> met de toepassings-ID die u zojuist hebt geregistreerd.
</li>
</ol>
