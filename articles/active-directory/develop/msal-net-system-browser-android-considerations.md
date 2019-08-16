---
title: Xamarin Android-overwegingen (micro soft Authentication Library voor .NET) | Azure
description: Meer informatie over specifieke overwegingen bij het gebruik van Xamarin Android met de micro soft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b0c810097913e896027245b15600ed75aabcd25
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532574"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>Xamarin Android-specifieke overwegingen met MSAL.NET
In dit artikel worden specifieke overwegingen beschreven voor het gebruik van de systeem browser op Xamarin Android met de micro soft Authentication Library voor .NET (MSAL.NET).

Vanaf MSAL.NET 2.4.0-Preview ondersteunt MSAL.NET andere browsers dan Chrome en hoeft niet langer Chrome te worden geïnstalleerd op het Android-apparaat voor verificatie.

We raden u aan om browsers te gebruiken die aangepaste tabbladen ondersteunen, zoals deze:

| Browsers met aangepaste tabbladen ondersteunen | Pakket naam |
|------| ------- |
|Chrome | com. Android. Chrome|
|Microsoft Edge | com.microsoft.emmx|
|Firefox | org. mozilla. Firefox|
|Ecosia | com.ecosia.android|
|Kiwi | com. kiwibrowser. browser|
|Brave | com. Brave. browser|

Naast browsers met aangepaste tabbladen ondersteuning, op basis van onze tests, kunnen enkele browsers die geen aangepaste tabbladen ondersteunen, ook worden gebruikt voor verificatie: Opera, Opera Mini-, inbrowser-en Maxthon. Lees voor meer informatie de [tabel voor test resultaten](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Android-system-browser#devices-and-browsers-tested).

## <a name="known-issues"></a>Bekende problemen

- Als de gebruiker geen browser op het apparaat heeft ingeschakeld, wordt door MSAL.net een `AndroidActivityNotFound` uitzonde ring gegenereerd. 
  - **Risico beperking**: Informeer de gebruiker dat ze een browser (bij voor keur een met ondersteuning voor aangepaste tabbladen) moeten inschakelen op hun apparaat.

- Als verificatie mislukt (bijvoorbeeld verificatie wordt gestart met DuckDuckGo), retourneert MSAL.NET een `AuthenticationCanceled MsalClientException`. 
  - **Hoofd probleem**: Er is geen ondersteuning voor een browser met aangepaste tabbladen ingeschakeld op het apparaat. Verificatie is gestart met een alternatieve browser, waardoor de verificatie niet kan worden voltooid. 
  - **Risico beperking**: Informeer de gebruiker dat ze een browser (bij voor keur een met aangepaste tabblad ondersteuning) moeten installeren op hun apparaat.

## <a name="devices-and-browsers-tested"></a>Apparaten en browsers getest
De volgende tabel geeft een lijst van de apparaten en browsers die zijn getest.

| | Browser&ast;     |  Resultaat  | 
| ------------- |:-------------:|:-----:|
| Huawei/één + | Chrome&ast; | Geslaagd|
| Huawei/één + | Edge&ast; | Geslaagd|
| Huawei/één + | Firefox&ast; | Geslaagd|
| Huawei/één + | Brave&ast; | Geslaagd|
| Eén + | Ecosia&ast; | Geslaagd|
| Eén + | Kiwi&ast; | Geslaagd|
| Huawei/één + | Opera | Geslaagd|
| Huawei | OperaMini | Geslaagd|
| Huawei/één + | InBrowser | Geslaagd|
| Eén + | Maxthon | Geslaagd|
| Huawei/één + | DuckDuckGo | Door de gebruiker geannuleerde authenticatie|
| Huawei/één + | UC-browser | Door de gebruiker geannuleerde authenticatie|
| Eén + | Dolfijnen | Door de gebruiker geannuleerde authenticatie|
| Eén + | CM-browser | Door de gebruiker geannuleerde authenticatie|
| Huawei/één + | geen geïnstalleerd | AndroidActivityNotFound ex|

&ast;Ondersteunt aangepaste tabbladen

## <a name="next-steps"></a>Volgende stappen
Lees deze [hand leiding](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/MSAL.NET-uses-web-browser#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid)voor code fragmenten en aanvullende informatie over het gebruik van System browser met Xamarin Android.  