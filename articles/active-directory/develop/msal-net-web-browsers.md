---
title: Webbrowsers in Microsoft Authentication Library voor .NET | Azure
description: Meer informatie over specifieke aandachtspunten bij het gebruik van Xamarin Android met de Microsoft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6e13ec3d822ba8a8cd2484f42ea81e615bae268
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190990"
---
# <a name="using-web-browsers-in-msalnet"></a>Met behulp van webbrowsers in MSAL.NET
Webbrowsers zijn vereist voor interactieve verificatie. Standaard MSAL.NET ondersteunt de [system-webbrowser](#system-web-browser-on-xamarinios-and-xamarinandroid) op Xamarin.iOS en [Xamarin.Android](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/system-browser). Maar [kunt u ook de ingesloten webbrowser](#enable-embedded-webviews) afhankelijk van uw vereisten (UX, eenmalige aanmelding (SSO), beveiliging nodig) in [Xamarin.iOS](#choosing-between-embedded-web-browser-or-system-browser-on-xamarinios) en [Xamarin.Android](#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid) Apps. En u kunt zelfs [dynamisch Kies](#detecting-the-presence-of-custom-tabs-on-xamarinandroid) webbrowser te gebruiken op basis van de aanwezigheid van Chrome of een browser Chrome aangepaste tabbladen in Android ondersteunen.

## <a name="web-browsers-in-msalnet"></a>Webbrowsers in MSAL.NET

Het is belangrijk om te begrijpen dat interactief te werk om een token te verkrijgen, de inhoud van het dialoogvenster niet is opgegeven door de bibliotheek, maar door de STS (Security Token Service). De verificatie-eindpunt verzendt terug sommige HTML en JavaScript die bepaalt de interactie, die wordt weergegeven in een webbrowser of het webbesturingselement voor. De STS voor het afhandelen van de HTML-interactie zodat biedt veel voordelen:

- Het wachtwoord (als een is opgegeven) is nooit worden opgeslagen door de toepassing, noch de verificatiebibliotheek.
- Hiermee kunt omleidingen naar andere id-providers (bijvoorbeeld aanmelding-in met een werk of school-account of een persoonlijk account met MSAL of met een sociaalnetwerkaccount met Azure AD B2C).
- Hiermee kunt de STS bepalen voor voorwaardelijke toegang, bijvoorbeeld door de gebruiker wilt meerdere factor authentication (MFA) tijdens de verificatiefase (een Windows Hello-pincode of die wordt aangeroepen op hun telefoon, of op een authentication-app op hun telefoon). In gevallen waar de vereiste multi-factor authentication is niet ingesteld het nog, kunt de gebruiker instellen het just-in-tijd in het dialoogvenster voor dezelfde.  De gebruiker voert het mobiele telefoonnummer en geleid voor het installeren van een toepassing voor verificatie en scan een QR-code als hun account wilt toevoegen. Deze interactie aangedreven server is een fantastische ervaring!
- Kan de gebruiker het wachtwoord in dit dialoogvenster dezelfde wijzigen wanneer het wachtwoord is verlopen (inclusief extra velden voor het oude wachtwoord en het nieuwe wachtwoord).
- Hiermee huisstijl van de tenant, of de toepassing (installatiekopieën) wordt beheerd door de Azure AD-tenant-beheerder / toepassingseigenaar van de.
- Hiermee kunnen de gebruikers toestemming geven zodat de toepassing toegang krijgen tot bronnen / scopes in hun naam direct na de verificatie.

## <a name="system-web-browser-on-xamarinios-and-xamarinandroid"></a>Systeem-webbrowser op Xamarin.iOS en Xamarin.Android

MSAL.NET biedt standaard ondersteuning voor de webbrowser van het systeem op Xamarin.iOS en Xamarin.Android. Voor alle platforms waarmee u de gebruikersinterface (dat wil zeggen, niet .NET Core), wordt een dialoogvenster geboden door de bibliotheek voor het insluiten van een browser-besturingselement. Een ingesloten webweergave MSAL.NET ook gebruikt voor de .NET-Desktop- en Windows-Adresboek voor het platform UWP. Echter, wordt standaard gebruikgemaakt van de **system-webbrowser** voor Xamarin iOS- en Xamarin Android-toepassingen. Voor iOS geldt dat ook gekozen de webweergave te gebruiken, afhankelijk van de versie van het besturingssysteem (iOS12, iOS11, en eerder).

Met behulp van de browser van het systeem is het belangrijk voordeel van het delen van de status voor eenmalige aanmelding met andere toepassingen en webtoepassingen zonder een broker (bedrijfsportal / Authenticator). De browser system is gebruikt, in de MSAL.NET voor de Xamarin iOS- en Xamarin Android-platformen wordt standaard omdat op deze platforms, het hele scherm in beslag neemt de webbrowser van het systeem en de gebruikerservaring is beter. De webweergave van het systeem is niet in een dialoogvenster te onderscheiden. Op iOS-, maar de gebruiker mogelijk toestemming geven voor de browser naar de toepassing, die irritant zijn worden kan voor de terugbelfunctie.

### <a name="uwp-does-not-use-the-system-webview"></a>UWP maakt geen gebruik van de System Webview

Voor bureaubladtoepassingen hebben echter leidt een System Webview starten tot een subpar gebruikerservaring als de gebruiker de browser ziet, waar ze mogelijk al hebben andere tabbladen geopend. En als verificatie is opgetreden, de gebruikers een pagina om dit venster sluiten opgehaald. Als de gebruiker geen aandacht heeft, kunnen ze het hele proces (met inbegrip van andere tabbladen die niet gerelateerd aan de verificatie zijn) sluiten. Gebruik te maken van het systeem in een browser op bureaublad ook vereist lokale poorten te openen en te hebben geluisterd op, die over geavanceerde machtigingen voor de toepassing kan vereisen. Het is mogelijk dat u als ontwikkelaar, gebruiker of beheerder, opwekken over deze vereiste.

## <a name="enable-embedded-webviews"></a>Ingesloten webweergaven inschakelen 
U kunt ook de ingesloten webweergaven in Xamarin.iOS en Xamarin.Android-apps inschakelen. Beginnen met MSAL.NET 2.0.0-preview, MSAL.NET ook ondersteunt het gebruik van de **ingesloten** webview-optie. Ingesloten webview is voor ADAL.NET, de enige optie ondersteund.

Als een ontwikkelaar met behulp van MSAL.NET die gericht is op Xamarin, besluiten kunt u ingesloten webweergaven of systeem browsers gebruiken. Dit is uw keuze, afhankelijk van de gebruiker en problemen die u wilt targeten.

MSAL.NET biedt de Android- en iOS-brokers op dit moment nog geen ondersteuning. Dus als u nodig hebt voor eenmalige aanmelding (SSO), de browser van het systeem nog steeds mogelijk een betere optie. Ondersteunende brokers met de ingesloten webbrowser is op de achterstand van MSAL.NET.

### <a name="differences-between-embedded-webview-and-system-browser"></a>Verschillen tussen ingesloten webview en system-browser 
Er zijn enkele visuele verschillen tussen ingesloten webview en systeem browser in MSAL.NET.

**Interactieve aanmelding met MSAL.NET met behulp van de ingesloten webweergave:**

![Ingesloten](media/msal-net-web-browsers/embedded-webview.png)

**Interactieve aanmelding met MSAL.NET met behulp van de Browser System:**

![Systeem-browser](media/msal-net-web-browsers/system-browser.png)

### <a name="developer-options"></a>Opties voor ontwikkelaars

Als een ontwikkelaar met behulp van MSAL.NET, hebt u verschillende mogelijkheden voor het weergeven van de interactieve dialoogvenster van de STS:

- **Systeem-browser.** De browser van het systeem is standaard ingesteld in de bibliotheek. Als u Android, leest u [system browsers](msal-net-system-browser-android-considerations.md) voor specifieke informatie over welke browsers worden ondersteund voor verificatie. Wanneer u de browser systeem in Android, wordt u aangeraden dat een browser die ondersteuning biedt voor aangepaste tabbladen Chrome is op het apparaat.  Anders, verificatie kan mislukken.
- **Ingesloten webweergave.** Gebruik alleen de webweergave ingesloten in MSAL.NET, de `AcquireTokenInteractively` parameters builder bevat een `WithUseEmbeddedWebView()` methode.

    iOS

    ```csharp
    AuthenticationResult authResult;
    authResult = app.AcquireTokenInteractively(scopes)
                    .WithUseEmbeddedWebView(useEmbeddedWebview)
                    .ExecuteAsync();
    ```

    Android:

    ```csharp
    authResult = app.AcquireTokenInteractively(scopes)
                .WithParentActivityOrWindow(activity)
                .WithUseEmbeddedWebView(useEmbeddedWebview)
                .ExecuteAsync();
    ```

#### <a name="choosing-between-embedded-web-browser-or-system-browser-on-xamarinios"></a>Kiezen tussen ingesloten webbrowser of systeem in een browser op Xamarin.iOS

In uw iOS-app in `AppDelegate.cs` kunt u kunt initialiseren de `ParentWindow` naar `null`. Het wordt niet gebruikt in iOS

```csharp
App.ParentWindow = null; // no UI parent on iOS
```

#### <a name="choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid"></a>Kiezen tussen ingesloten webbrowser of systeem in een browser op Xamarin.Android

In uw Android-app in `MainActivity.cs` u kunt de bovenliggende activiteit, zo instellen dat de resultaten van de verificatie wordt in toe:

```csharp
 App.ParentWindow = this;
```

Klik in de `MainPage.xaml.cs`:

```csharp
authResult = await App.PCA.AcquireTokenInteractive(App.Scopes)
                      .WithParentActivityOrWindow(App.ParentWindow)
                      .WithUseEmbeddedWebView(true)
                      .ExecuteAsync();
```

#### <a name="detecting-the-presence-of-custom-tabs-on-xamarinandroid"></a>Detecteren van de aanwezigheid van aangepaste tabbladen aan Xamarin.Android

Als u gebruiken de webbrowser van het systeem wilt voor het inschakelen van eenmalige aanmelding met de apps die worden uitgevoerd in de browser, maar zijn vastgesteld over de gebruikerservaring voor Android-apparaten niet met een browser met ondersteuning voor aangepast tabblad, hebt u de optie om te bepalen door het aanroepen van de `IsSystemWebViewAvailable()` methode in < c 2 > `IPublicClientApplication` . Deze methode retourneert `true` als de PackageManager aangepaste tabbladen detecteert en `false` als ze niet op het apparaat worden gedetecteerd.

Op basis van de waarde die wordt geretourneerd door deze methode, en uw behoeften, kunt u een beslissing nemen:

- U kunt een aangepast foutbericht terugkeren naar de gebruiker. Bijvoorbeeld: 'Installeer Chrome om door te gaan met verificatie' - of-
- U kunt terugvallen op het ingesloten webview-optie en de gebruikersinterface als een ingesloten webview starten.

De volgende code toont de ingesloten webview-optie:

```csharp
bool useSystemBrowser = app.IsSystemWebviewAvailable();

authResult = await App.PCA.AcquireTokenInteractive(App.Scopes)
                      .WithParentActivityOrWindow(App.ParentWindow)
                      .WithUseEmbeddedWebView(!useSystemBrowser)
                      .ExecuteAsync();
```

## <a name="net-core-does-not-support-interactive-authentication-out-of-the-box"></a>Interactieve verificatie gebruiksklaar biedt geen ondersteuning voor .NET core

Voor .NET Core is aanschaf van tokens interactief niet beschikbaar. Inderdaad, .NET Core biedt geen gebruikersinterface nog. Als u wilt dat voor interactief aanmelden voor een .NET Core-toepassing, kunt u op de toepassing aanwezig voor de gebruiker een code en een URL naar interactief aanmelden (Zie [apparaat Code Flow](msal-authentication-flows.md#device-code)).

U kunt ook implementeren de [IWithCustomUI](scenario-desktop-acquire-token.md#withcustomwebui) interface en geef uw eigen browser