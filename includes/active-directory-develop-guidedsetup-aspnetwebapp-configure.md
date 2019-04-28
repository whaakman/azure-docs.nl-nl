---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 2cdc6ea01e6c3555740102f319d0f4e8e4fc1c22
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60298112"
---
## <a name="register-your-application"></a>Uw toepassing registreren

Voor het registreren van uw toepassing en de registratie-informatie van uw toepassing toevoegen aan uw oplossing, hebt u twee opties:

### <a name="option-1-express-mode"></a>Optie 1: De Express-modus

U kunt snel uw toepassing registreren met het volgende te doen:

1. Ga naar de nieuwe [Azure portal - App-registraties](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs) deelvenster.
1. Voer een naam in voor de toepassing en klik op **Registreren**.
1. Volg de instructies om de nieuwe toepassing met slechts één klik te downloaden en automatisch te configureren.

### <a name="option-2-advanced-mode"></a>Optie 2: Geavanceerde modus

Volg deze stappen om de toepassing te registreren en de registratiegegevens van de app handmatig toe te voegen aan uw oplossing:

1. Ga naar Visual Studio en:
   1. Selecteer het project in Solution Explorer en bekijk het venster Eigenschappen (als er geen een venster met eigenschappen, druk op F4).
   1. SSL ingeschakeld om te wijzigen `True`.
   1. Met de rechtermuisknop op het project in Visual Studio, en kies vervolgens **eigenschappen**, en de **Web** tabblad. In de *Servers* sectie wijzigen de *Project-Url* moet het SSL-URL.
   1. Kopieer de URL van SSL. U kunt deze URL wordt toevoegen aan de lijst met Omleidings-URL's in de lijst van de Portal van de registratie van de omleidings-URL's in de volgende stap:<br/><br/>![Projecteigenschappen](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Als u via uw account toegang hebt tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en stelt u de portalsessie in op de gewenste Azure Active Directory-tenant.
1. Navigeer naar de Microsoft identity-platform voor ontwikkelaars [App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) pagina.
1. Selecteer **registratie van nieuwe**.
1. Wanneer de pagina **Een toepassing registreren** verschijnt, voert u de registratiegegevens van de toepassing in:
   1. Voer in de sectie **Naam** een beschrijvende toepassingsnaam. Deze wordt zichtbaar voor gebruikers van de app. Bijvoorbeeld: `ASPNET-Tutorial`.
   1. Voeg de SSL-URL die u had gekopieerd vanuit Visual Studio in stap 1 (bijvoorbeeld `https://localhost:44368/`) in **antwoord-URL**, en klikt u op **registreren**.
1. Selecteer **Verificatie**, stel **ID-tokens** in onder **Impliciete toekenning** en selecteer vervolgens **Opslaan**.
1. Voeg de volgende in `web.config` zich in de hoofdmap in de sectie `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

1. Vervang `ClientId` met de toepassings-ID die u zojuist hebt geregistreerd.
1. Vervang `redirectUri` met de SSL-URL van uw project.
