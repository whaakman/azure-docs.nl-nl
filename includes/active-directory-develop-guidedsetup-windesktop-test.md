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
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 6345eca674086801f8bb0f45476009f04a10f2e3
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58214032"
---
## <a name="test-your-code"></a>Uw code testen

Als u wilt uitvoeren van uw project in Visual Studio, selecteer **F5**. Uw toepassing **MainWindow** wordt weergegeven, zoals hier wordt weergegeven:

![Uw toepassing testen](./media/active-directory-develop-guidedsetup-windesktop-test/samplescreenshot.png)

De eerste keer is dat u de toepassing uitvoert en selecteer de **Microsoft Graph-API aanroepen** knop, wordt u gevraagd aan te melden. Gebruik Azure Active Directory-account (werk of school-account) of een Microsoft-account (live.com, outlook.com) om dit te testen.

![Aanmelden bij Azure Portal](./media/active-directory-develop-guidedsetup-windesktop-test/signinscreenshot.png)

### <a name="provide-consent-for-application-access"></a>Geef toestemming voor toegang tot toepassingen

De eerste keer dat u zich aanmelden bij uw toepassing u ook wordt gevraagd voor toestemming geven om toe te staan van de aanvraag voor toegang tot uw profiel en meld u aan u in, zoals hier wordt weergegeven:

![Geef uw toestemming voor toegang tot toepassingen](./media/active-directory-develop-guidedsetup-windesktop-test/consentscreen.png)

### <a name="view-application-results"></a>Resultaten van de toepassing weergeven

Nadat u zich hebt aangemeld, ziet u de gebruikersgegevens van het profiel dat wordt geretourneerd door de aanroep naar de Microsoft Graph API. De resultaten worden weergegeven in de **API Gespreksresultaten** vak. Algemene informatie over het token dat is verkregen via de aanroep van `AcquireTokenAsync` of `AcquireTokenSilentAsync` moeten worden weergegeven in de **Token Info** vak. De resultaten bevatten de volgende eigenschappen:

|Eigenschap  |Indeling  |Description |
|---------|---------|---------|
|**Naam** |Volledige naam van gebruiker |En achternaam van eerst de gebruiker.|
|**Gebruikersnaam** |<span>user@domain.com</span> |De gebruikersnaam die wordt gebruikt om de gebruiker te identificeren.|
|**Token is verlopen** |DateTime |De tijd waarop het token is verlopen. MSAL vervolg op de vervaldatum vernieuwen van het token indien nodig.|
|**Toegangstoken** |String |De token tekenreeks die wordt verzonden naar HTTP-aanvragen waarvoor een *autorisatie-header*.|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereiken en gedelegeerde machtigingen

De Microsoft Graph API vereist de *user.read* scope om te lezen van het gebruikersprofiel. Dit bereik wordt automatisch toegevoegd in elke toepassing die geregistreerd in de Portal voor Appregistratie standaard. Andere voor Microsoft Graph-API's, evenals de aangepaste API's voor uw back-endserver, mogelijk extra scopes. De Microsoft Graph API vereist de *Calendars.Read* bereik om agenda's van de gebruiker weer te geven.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing moet toevoegen de *Calendars.Read* overgedragen machtigingen voor de registratiegegevens van de toepassing. Voeg vervolgens de *Calendars.Read* bereik instellen op de `acquireTokenSilent` aanroepen.

>[!NOTE]
>De gebruiker mogelijk voor extra toestemmingen gevraagd als u het aantal bereiken verhogen.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
