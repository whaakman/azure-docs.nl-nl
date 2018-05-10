---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 7c052f30e564700b0a99b76342dc6c34ec5983b7
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
## <a name="register-your-application"></a>Uw toepassing registreren

Voor het registreren van uw toepassing en de registratiegegevens van uw toepassing toevoegen aan uw oplossing, hebt u twee opties:

### <a name="option-1-express-mode"></a>Optie 1: Snelle modus

U kunt snel uw toepassing registreren als volgt:

1. Registreren van uw toepassingen via de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Voer een naam voor uw toepassing en uw e-mailadres
3.  Zorg ervoor dat de optie voor begeleide Setup is ingeschakeld
4.  Volg de instructies voor het toevoegen van een Omleidings-URL voor uw toepassing

### <a name="option-2-advanced-mode"></a>Optie 2: Geavanceerde modus

Uw toepassing registreren en de registratiegegevens van uw toepassing toevoegen aan uw oplossing, kunt u het volgende:

1. Ga naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app) een toepassing registreren
2. Voer een naam voor uw toepassing en uw e-mailadres 
3. Zorg ervoor dat de optie voor begeleide Setup is uitgeschakeld
4. Klik op `Add Platform`, selecteer vervolgens `Web`
5. Ga terug naar Visual Studio en klik in Solution Explorer, selecteert u het project en kijk in het venster Eigenschappen (als er geen een venster met eigenschappen, druk op F4)
6. SSL ingeschakeld om te wijzigen `True`
7. Klik met de rechtermuisknop op het project in Visual Studio en kies vervolgens **eigenschappen**, en de **Web** tabblad. In de *Servers* sectie wijzigen de *Project Url* worden de SSL-URL
8. De SSL-URL kopiëren en deze URL toevoegen aan de lijst van de omleidings-URL's in de lijst van de Portal van de registratie van de omleidings-URL's:<br/><br/>![Projecteigenschappen](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
9. Voeg de volgende in `web.config` zich in de hoofdmap in de sectie `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

10. Vervang `ClientId` met de toepassings-ID die u zojuist hebt geregistreerd
11. Vervang `redirectUri` met de SSL-URL van uw project

