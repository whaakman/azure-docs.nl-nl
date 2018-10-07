---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 991709ee635872e33dc89dcededc7f6ac3b28ea3
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843508"
---
## <a name="test-your-app"></a>Uw app testen

1. De code uitvoeren op uw apparaat/de emulator.

2. Probeer aan te melden met een Azure Active Directory-account (werk of school-account) of een Microsoft-account (live.com, outlook.com). 

    ![Uw toepassing testen](media/active-directory-develop-guidedsetup-android-test/mainwindow.png)
    <br/><br/>
    ![Gebruikersnaam en wachtwoord invoeren](media/active-directory-develop-guidedsetup-android-test/usernameandpassword.png)

### <a name="consent-to-your-app"></a>Toestemming geven voor uw app
De eerste keer dat een gebruiker zich aanmeldt bij uw toepassing, wordt ze gevraagd akkoord gaan met de machtigingen voor de behoeften voor uw app, zoals hier wordt weergegeven: 

![Geef uw toestemming voor toegang tot toepassingen](media/active-directory-develop-guidedsetup-android-test/androidconsent.png)


### <a name="success"></a>Gelukt!
Nadat u zich aanmeldt & toestemming geven, wordt de app de reactie van de Microsoft Graph API weer. Deze specifieke aanroep is de **/me** eindpunt en retourneert de [gebruikersprofiel](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_get). Zie voor een lijst van andere Microsoft Graph-eindpunten, [Microsoft Graph API-documentatie voor ontwikkelaars](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="scopes-and-delegated-permissions"></a>Bereiken en gedelegeerde machtigingen

De Microsoft Graph API vereist de *User.Read* scope om te lezen van het gebruikersprofiel. Dit bereik wordt automatisch in elke app die geregistreerd in de Portal voor Appregistratie. Andere API's moet extra scopes. Bijvoorbeeld: de Microsoft Graph-API heeft de *Calendars.Read* bereik om agenda's van de gebruiker weer te geven. 

Voor toegang tot agenda's van de gebruiker moet toevoegen de *Calendars.Read* overgedragen machtigingen voor de registratiegegevens van de toepassing. Voeg vervolgens de *Calendars.Read* bereik instellen op de `acquireTokenSilent` aanroepen. 

>[!NOTE]
>Uw gebruikers mogelijk gevraagd om aanvullende toestemming als u de registratie van uw app wijzigt.

<!--end-collapse-->

[!INCLUDE [Help and support](active-directory-develop-help-support-include.md)]
