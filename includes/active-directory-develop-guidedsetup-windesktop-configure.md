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
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 5eaee4f932c4e42f6fed3d839314346b3a93f360
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59498380"
---
## <a name="register-your-application"></a>Uw toepassing registreren

U kunt uw toepassing registreren in een van de twee manieren.

### <a name="option-1-express-mode"></a>Optie 1: De Express-modus

U kunt snel uw toepassing registreren met het volgende te doen:
1. Ga naar de [Azure portal - toepassing registreren](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/WinDesktopQuickstartPage/sourceType/docs).
1. Voer een naam in voor de toepassing en selecteer **Registreren**.
1. Volg de instructies om de nieuwe toepassing met slechts één klik te downloaden en automatisch te configureren.

### <a name="option-2-advanced-mode"></a>Optie 2: Geavanceerde modus

Ga als volgt te werk om de toepassing te registreren en de registratiegegevens van de toepassing toe te voegen aan uw oplossing:
1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Als u via uw account toegang hebt tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en stelt u de portalsessie in op de gewenste Azure Active Directory-tenant.
1. Navigeer naar de Microsoft identity-platform voor ontwikkelaars [App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) pagina.
1. Selecteer **registratie van nieuwe**.
   - Voer in de sectie **Naam** een beschrijvende toepassingsnaam. Deze wordt zichtbaar voor gebruikers van de app. Bijvoorbeeld: `Win-App-calling-MsGraph`.
   - Selecteer in de sectie **Ondersteunde accounttypen** de optie **Accounts in alle organisatiemappen en persoonlijke Microsoft-accounts (bijvoorbeeld Skype, Xbox, Outlook.com**.
   - Selecteer **Registreren** om de toepassing te maken.
1. Selecteer in de lijst met pagina’s voor de app de optie **Verificatie**.
1. Ga in de sectie **Omleidings-URI's** naar de sectie **Voorgestelde omleidings-URI's voor openbare clients (mobiel, desktop)**, en selecteer **"urn:ietf:wg:oauth:2.0:oob**.
1. Selecteer **Opslaan**.
1. Ga naar Visual Studio, open de *App.xaml.cs* bestand en vervang `Enter_the_Application_Id_here` met toepassings-ID die u zojuist hebt geregistreerd en gekopieerd.

    ```csharp
    private static string ClientId = "Enter_the_Application_Id_here";
    ```
