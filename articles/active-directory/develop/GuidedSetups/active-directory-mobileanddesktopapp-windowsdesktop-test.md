---
title: Azure AD v2 Windows Desktop aan de slag - testen | Microsoft Docs
description: Hoe Windows Desktop .NET (XAML) toepassingen een API waarvoor toegangstokens door Azure Active Directory-v2-eindpunt kunnen aanroepen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: e9fbfc301fb987c72605f8b16a707513661a1b65
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
## <a name="test-your-code"></a>Testen van uw code

Testen van uw toepassing, drukt u op `F5` uw project in Visual Studio wilt uitvoeren. Het hoofdvenster moet worden weergegeven:

![Schermopname van een steekproef](media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

Wanneer u klaar bent om te testen, klikt u op *Microsoft Graph-API aanroepen* en gebruik van een Microsoft Azure Active Directory (organisatieaccount) of een Microsoft-Account (live.com, outlook.com)-account aan te melden. Dit is de eerste keer, ziet u een venster waarin de gebruiker die zich aanmeldt:

![Aanmelden](media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### <a name="consent"></a>Toestemming
De eerste keer dat u zich aanmeldt bij uw toepassing worden weergegeven met een scherm toestemming vergelijkbaar met de waarin u wilt accepteren expliciet hieronder:

![Toestemming scherm](media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### <a name="expected-results"></a>Verwachte resultaten
U ziet gebruikersprofielgegevens geretourneerd door de Microsoft Graph API-aanroep op het scherm API aanroepen resultaten.

U ziet ook basisinformatie over het token dat is verkregen `AcquireTokenAsync` of `AcquireTokenSilentAsync` in het vak Token Info:

|Eigenschap  |Indeling  |Beschrijving |
|---------|---------|---------|
|Naam | {De volledige gebruikersnaam} |De voor- en achternaam van de gebruiker|
|Gebruikersnaam |<span>user@domain.com</span> |De gebruikersnaam die wordt gebruikt voor het identificeren van de gebruiker|
|Token is verlopen |{Datum-/}         |De tijd waarop het token verloopt. MSAL wordt de verloopdatum u door het vernieuwen van het token indien nodig|
|Toegangstoken |{Tekenreeks}         |De tokentekenreeks verzonden die worden verzonden naar HTTP-aanvragen waarvoor de autorisatie-header|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereikwaarden en gedelegeerde machtigingen
Graph API kan alleen de `user.read` bereik gebruikersprofiel lezen. Deze scope wordt automatisch toegevoegd standaard in elke toepassing die wordt geregistreerd op onze portal voor wachtwoordregistratie. Vereist extra scopes sommige andere Graph API's, evenals aangepaste API's voor uw back-endserver. Bijvoorbeeld: voor de grafiek, `Calendars.Read` is vereist tot agenda's van de gebruiker van de lijst. Voor toegang tot de agenda van de gebruiker in een context van een toepassing, moet u toevoegen `Calendars.Read` toepassing registratiegegevens gedelegeerd en voeg vervolgens `Calendars.Read` naar de `AcquireTokenAsync` aanroepen. Gebruiker kan worden gevraagd om extra toestemmingen verhogen van het aantal scopes.

<!--end-collapse-->

[!INCLUDE  [Help and Support Options](../../../../includes/active-directory-develop-help-support-include.md)]