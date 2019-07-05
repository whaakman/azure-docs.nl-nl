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
ms.openlocfilehash: 357c83cfd0ae3fed8b13419e72f50fcb90c04186
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67550660"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>Overwegingen voor Xamarin Android-specifieke met MSAL.NET
Dit artikel worden bepaalde overwegingen bij het gebruik van Xamarin Android met de Microsoft Authentication Library voor .NET (MSAL.NET).

In dit artikel is bedoeld voor MSAL.NET 3.x. Als u geïnteresseerd in MSAL.NET bent 2.x gebruikt, Zie [Xamarin Android-specificaties in MSAL.NET 2.x](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Xamarin-Android-specifics-2x).

## <a name="set-the-parent-activity"></a>Instellen van de bovenliggende activiteit

Op Xamarin.Android moet u de bovenliggende activiteit zo instellen dat het token opgehaald terug nadat de interactie is gebeurd.

```csharp
var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .ExecuteAsync();
```

## <a name="ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends"></a>Ervoor te zorgen dat besturingselement gaat terug naar MSAL één keer de interactieve gedeelte van de flow-verificatie is beëindigd
Voor Android geldt dat u wilt overschrijven de `OnActivityResult` -methode van de `Activity` en roep de methode SetAuthenticationContinuationEventArgs van de klasse AuthenticationContinuationHelper MSAL.

```csharp
protected override void OnActivityResult(int requestCode, 
                                         Result resultCode, Intent data)
{
 base.OnActivityResult(requestCode, resultCode, data);
 AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode,
                                                                         resultCode,
                                                                         data);
}

```
Deze regel zorgt ervoor dat het besturingselement terug naar MSAL gaat zodra het interactieve gedeelte van de verificatie-stroom is beëindigd.

## <a name="update-the-android-manifest"></a>Bijwerken van het Android-manifest
De `AndroidManifest.xml` mag de volgende waarden:
```csharp
<activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="msal{client_id}" android:host="auth" />
         </intent-filter>
</activity>
```

## <a name="use-the-embedded-web-view-optional"></a>Gebruik de ingesloten Webweergave (optioneel)

MSAL.NET maakt standaard gebruik van de webbrowser system, waarmee u om eenmalige aanmelding met webtoepassingen en andere apps te verkrijgen. In sommige gevallen is het raadzaam om op te geven dat u wilt gebruiken van de ingesloten webweergave. Zie voor meer informatie, [MSAL.NET maakt gebruik van een webbrowser](msal-net-web-browsers.md) en [browser voor Android-systeem](msal-net-system-browser-android-considerations.md).

```csharp
bool useEmbeddedWebView = !app.IsSystemWebViewAvailable;

var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .WithEmbeddedWebView(useEmbeddedWebView)
 .ExecuteAsync();
```

## <a name="troubleshooting"></a>Problemen oplossen
Als u een nieuwe Xamarin.Forms-toepassing maken en voeg een verwijzing naar het MSAL.Net NuGet-pakket, wordt dit alleen werkt.
Als u wilt upgraden een bestaande Xamarin.Forms toepassing MSAL.NET preview 1.1.2 of hoger kunt u ervaren build-problemen.

Als u wilt oplossen, moet u de:
- Het bestaande MSAL.NET NuGet-pakket naar MSAL.NET preview 1.1.2 bijwerken of hoger
- Controleer dat Xamarin.Forms automatisch naar versie 2.5.0.122203 (zo niet, update in deze versie bijgewerkt)
- Controleer dat Xamarin.Android.Support.v4 automatisch naar versie 25.4.0.2 (zo niet, update in deze versie bijgewerkt)
- Alle Xamarin.Android.Support pakketten moeten zijn gericht op versie 25.4.0.2
- Opruimen/herstellen
- Probeer het instellen van het maximale parallelle project bouwt voort op 1 in Visual Studio (Opties -> projecten en oplossingen -> Build en Run -> het maximale aantal parallelle projecten builds)
- U kunt ook als u vanaf de opdrachtregel bouwt, probeert /m verwijderen uit uw opdracht als u deze gebruikt.


### <a name="error-the-name-authenticationcontinuationhelper-does-not-exist-in-the-current-context"></a>Fout: De naam 'AuthenticationContinuationHelper' niet in de huidige context bestaat

Dit komt waarschijnlijk doordat de Android.csproj*-bestand niet correct door Visual Studio bijgewerkt is. Soms de  **\<HintPath >** filepath bevat onjuist netstandard13 in plaats van **monoandroid90**.

```xml
<Reference Include="Microsoft.Identity.Client, Version=3.0.4.0, Culture=neutral, PublicKeyToken=0a613f4dd989e8ae,
           processorArchitecture=MSIL">
  <HintPath>..\..\packages\Microsoft.Identity.Client.3.0.4-preview\lib\monoandroid90\Microsoft.Identity.Client.dll</HintPath>
</Reference>
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over en voorbeelden vindt u in de [Android specifieke aandachtspunten](https://github.com/azure-samples/active-directory-xamarin-native-v2#android-specific-considerations) lid van het readme.md-bestand voor het volgende voorbeeld:

| Voorbeeld | Platform | Description |
| ------ | -------- | ----------- |
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Een eenvoudige Xamarin Forms-app die laat zien hoe u MSAL MSA en Azure AD via de AADD v2.0-eindpunt te verifiëren en toegang tot de Microsoft Graph met het resulterende token. <br>![Topologie](media/msal-net-xamarin-android-considerations/topology.png) |