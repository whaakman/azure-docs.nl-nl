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
ms.date: 05/04/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: faa3ad2376935aee4508b814f1b67fdacb98cf6e
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843600"
---
## <a name="register-your-application"></a>Uw toepassing registreren

Voor het registreren van uw toepassing en de registratie-informatie van uw toepassing toevoegen aan uw oplossing, hebt u twee opties:

### <a name="option-1-express-mode"></a>Optie 1: Snelle modus

U kunt snel uw toepassing registreren met het volgende te doen:

1. Registreren van uw toepassing via de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Voer een naam voor uw toepassing en uw e-mailadres
3.  Zorg ervoor dat de optie voor begeleide instelling is ingeschakeld
4.  Volg de instructies voor het toevoegen van een Omleidings-URL aan uw toepassing

### <a name="option-2-advanced-mode"></a>Optie 2: Geavanceerde modus

Ga als volgt te werk om de toepassing te registreren en de registratiegegevens van de toepassing toe te voegen aan uw oplossing:

1. Ga naar de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/portal/register-app) een toepassing registreren
2. Voer een naam voor uw toepassing en uw e-mailadres 
3. Zorg ervoor dat de optie voor begeleide instelling is uitgeschakeld
4. Klik op `Add Platform`en selecteer vervolgens `Web`
5. Ga terug naar Visual Studio en klik in Solution Explorer, selecteert u het project en bekijk het venster Eigenschappen (als er geen een venster met eigenschappen, druk op F4)
6. Wijzig SSL ingeschakeld in `True`
7. Klik met de rechtermuisknop op het project in Visual Studio, en kies vervolgens **eigenschappen**, en de **Web** tabblad. In de *Servers* sectie wijzigen de *Project-Url* moet het SSL-URL
8. Kopieer de URL van SSL en voeg deze URL toe aan de lijst met Omleidings-URL's in de lijst van de Portal van de registratie van de omleidings-URL's:<br/><br/>![Projecteigenschappen](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
9. Voeg de volgende in `web.config` zich in de hoofdmap in de sectie `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

10. Vervang `ClientId` met de toepassings-ID die u zojuist hebt geregistreerd
11. Vervang `redirectUri` met de SSL-URL van uw project

