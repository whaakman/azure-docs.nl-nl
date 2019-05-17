---
title: Overwegingen voor Xamarin Android (Microsoft Authentication Library voor .NET) | Azure
description: Meer informatie over specifieke aandachtspunten bij het gebruik van Xamarin Android met de Microsoft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c57feb33967732481d78e0ddaba5e90f4f82f327
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544430"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>Overwegingen voor Xamarin Android-specifieke met MSAL.NET
Dit artikel worden bepaalde overwegingen bij het gebruik van de browser systeem in Xamarin Android met de Microsoft Authentication Library voor .NET (MSAL.NET).

Beginnen met MSAL.NET 2.4.0-preview, MSAL.NET ondersteunt andere browsers dan Chrome en vereist niet langer Chrome worden geïnstalleerd op het Android-apparaat voor verificatie.

Het is raadzaam dat browsers die ondersteuning bieden voor aangepaste tabs, zoals deze te gebruiken:

| Browsers met de ondersteuning voor aangepaste tabbladen | Pakketnaam |
|------| ------- |
|Chrome | com.android.chrome|
|Microsoft Edge | com.microsoft.emmx|
|Firefox | org.mozilla.firefox|
|Ecosia | com.ecosia.android|
|Kiwi | com.kiwibrowser.browser|
|Dapper | com.brave.browser|

Naast browsers met de ondersteuning voor aangepaste tabbladen, op basis van onze tests wordt ook enkele browsers die bieden geen ondersteuning voor aangepaste tabbladen werken voor verificatie: Opera, Opera Mini, InBrowser en Maxthon. Lees voor meer informatie, [tabel voor de resultaten van](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Android-system-browser#devices-and-browsers-tested).

## <a name="known-issues"></a>Bekende problemen

- Als de gebruiker geen browser ingeschakeld op het apparaat heeft, MSAL.NET genereert een `AndroidActivityNotFound` uitzondering. 
  - **Risicobeperking**: De gebruiker informeren dat ze een browser (bij voorkeur een met ondersteuning voor aangepaste tabbladen) op hun apparaat moeten inschakelen.

- Als verificatie is mislukt (ex. verificatie wordt gestart met DuckDuckGo), MSAL.NET retourneert een `AuthenticationCanceled MsalClientException`. 
  - **De hoofd-probleem**: Een browser met ondersteuning voor aangepaste tabbladen is niet ingeschakeld op het apparaat. Verificatie wordt gestart met een andere browser, die niet worden gebruikt om verificatie te voltooien. 
  - **Risicobeperking**: De gebruiker informeren dat ze een browser (bij voorkeur een met ondersteuning voor aangepast tabblad) op hun apparaat installeren moeten.

## <a name="devices-and-browsers-tested"></a>Apparaten en browsers getest
De volgende tabel geeft een lijst van de apparaten en browsers die zijn getest.

| | Browser&ast;     |  Resultaat  | 
| ------------- |:-------------:|:-----:|
| Huawei / één + | Chrome&ast; | Geslaagd|
| Huawei / één + | Edge&ast; | Geslaagd|
| Huawei / één + | Firefox&ast; | Geslaagd|
| Huawei / één + | Dapper&ast; | Geslaagd|
| Een + | Ecosia&ast; | Geslaagd|
| Een + | Kiwi&ast; | Geslaagd|
| Huawei / één + | Opera | Geslaagd|
| Huawei | OperaMini | Geslaagd|
| Huawei / één + | InBrowser | Geslaagd|
| Een + | Maxthon | Geslaagd|
| Huawei / één + | DuckDuckGo | Verificatie van de gebruiker geannuleerd|
| Huawei / één + | UC-Browser | Verificatie van de gebruiker geannuleerd|
| Een + | Programma | Verificatie van de gebruiker geannuleerd|
| Een + | CM-browser | Verificatie van de gebruiker geannuleerd|
| Huawei / één + | geen geïnstalleerd | AndroidActivityNotFound ex|

&ast; Biedt ondersteuning voor aangepaste tabbladen

## <a name="next-steps"></a>Volgende stappen
Code-uittreksels en aanvullende informatie over het gebruik van system browser met Xamarin Android-, Lees voor dit [handleiding](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/MSAL.NET-uses-web-browser#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid).  