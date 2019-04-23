---
title: Microsoft identity platform Windows UWP quickstart | Azure
description: Ontdek hoe een Universal Windows Platform (XAML)-toepassing kan een toegangstoken en een API die wordt beveiligd door Microsoft identity platform eindpunt aanroepen.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d9d2e9aa5e5e805b302763f5417110cdd078eb3b
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997594"
---
# <a name="quickstart-call-the-microsoft-graph-api-from-a-universal-windows-platform-uwp-application"></a>Quickstart: De Microsoft Graph-API aanroepen vanuit de Universeel Windows-platformtoepasing (UWP)

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

In deze snelstartgids bevat een codevoorbeeld van die laat zien hoe een Universal Windows Platform (UWP)-toepassing kunt aanmelden van gebruikers met persoonlijke accounts of werk en schoolaccounts, een toegangstoken en de Microsoft Graph-API aanroepen.

![Laat zien hoe de voorbeeld-app die is gegenereerd door deze Quick Start werkt](media/quickstart-v2-uwp/uwp-intro.svg)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>De quickstart-app registreren en downloaden
> [!div renderon="docs" class="sxs-lookup"]
> U hebt twee opties voor het starten van de snelstarttoepassing:
> * [Express] [Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Handmatig] [Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld
>
> 1. Ga naar de nieuwe [Azure portal - App-registraties](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/UwpQuickstartPage/sourceType/docs) deelvenster.
> 1. Voer een naam in voor de toepassing en klik op **Registreren**.
> 1. Volg de instructies om de nieuwe toepassing met slechts één klik te downloaden en automatisch te configureren.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
> [!div renderon="docs"]
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> Volg deze stappen om de toepassing te registreren en de registratiegegevens van de app toe te voegen aan de oplossing:
> 1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
> 1. Als u via uw account toegang hebt tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en stelt u de portalsessie in op de gewenste Azure Active Directory-tenant.
> 1. Navigeer naar de Microsoft identity-platform voor ontwikkelaars [App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) pagina.
> 1. Selecteer **registratie van nieuwe**.
> 1. Wanneer de pagina **Een toepassing registreren** verschijnt, voert u de registratiegegevens van de toepassing in:
>      - Voer in de sectie **Naam** een beschrijvende toepassingsnaam. Deze wordt zichtbaar voor gebruikers van de app. Bijvoorbeeld: `UWP-App-calling-MsGraph`.
>      - Selecteer in de sectie **Ondersteunde accounttypen** de optie **Accounts in alle organisatiemappen en persoonlijke Microsoft-accounts (bijvoorbeeld Skype, Xbox, Outlook.com**.
>      - Selecteer **Registreren** om de toepassing te maken.
> 1. Selecteer in de lijst met pagina’s voor de app de optie **Verificatie**.
> 1. Ga in de sectie **Omleidings-URL's** naar de sectie **Voorgestelde Omleidings-URI's voor openbare clients (mobiel, desktop)**, en selecteer **"urn:ietf:wg:oauth:2.0:oob**.
> 1. Selecteer **Opslaan**.

> [!div renderon="portal" class="sxs-lookup alert alert-info"]
> #### <a name="step-1-configure-your-application"></a>Stap 1: Uw toepassing configureren
> Voor een juiste werking van het codevoorbeeld uit deze quickstart moet u een omleidings-URI toevoegen zoals **urn:ietf:wg:oauth:2.0:oob**.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Deze wijziging voor mij maken]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-uwp/green-check.png) Uw toepassing is al geconfigureerd met deze kenmerken.

#### <a name="step-2-download-your-visual-studio-project"></a>Stap 2: uw Visual Studio-project downloaden

 - [Download het Visual Studio-project](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>Stap 3: uw Visual Studio-project configureren

1. Pak het zip-bestand uit in een lokale map dicht bij de hoofdmap van de schijf, bijvoorbeeld **C:\Azure-Samples**.
1. Open het project in Visual Studio. U mogelijk gevraagd een UWP-SDK installeren. In dat geval accepteren.
1. Bewerken **MainPage.Xaml.cs** en vervang de waarden van de `ClientId` veld:

    ```csharp
    private const string ClientId = "Enter_the_Application_Id_here";
    ```

> [!div renderon="docs"]
> Waar:
> - `Enter_the_Application_Id_here`: de toepassings-id voor de toepassing die u hebt geregistreerd.
>
> > [!TIP]
> > De waarde van *toepassings-ID*, gaat u naar de **overzicht** sectie in de portal

#### <a name="step-4-run-your-application"></a>Stap 4: Uw toepassing uitvoeren

Als u proberen de Quick Start op uw Windows-computer wilt:

1. In de werkbalk van Visual Studio, kies het juiste platform (waarschijnlijk **x64** of **x86**, niet ARM).
   > Zien dat het doelapparaat van verandert *apparaat* naar *lokale computer*
1. Selecteer Foutopsporing | **Starten zonder foutopsporing**

## <a name="more-information"></a>Meer informatie

Deze sectie biedt meer informatie over de quickstart.

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)) is de bibliotheek gebruikt voor het aanmelden van gebruikers en aanvragen van beveiligingstokens. De beveiligingstokens worden gebruikt voor toegang tot een API die wordt beveiligd door Microsoft Identity-platform voor ontwikkelaars. U kunt MSAL installeren door de volgende opdracht uit te voeren in *Package Manager Console* van Visual Studio:

```powershell
Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="msal-initialization"></a>MSAL initialiseren

U kunt de verwijzing voor MSAL toevoegen door de volgende code toe te voegen:

```csharp
using Microsoft.Identity.Client;
```

Vervolgens is MSAL geïnitialiseerd met de volgende code:

```csharp
public static IPublicClientApplication PublicClientApp;
PublicClientApp = new PublicClientApplicationBuilder.Create(ClientId)
                                                    .Build();
```

> |Waar: ||
> |---------|---------|
> | `ClientId` | Is de **Toepassings-id (client-id)** voor de toepassing die is geregistreerd in de Azure-portal. U vindt deze waarde op de pagina **Overzicht** in de Azure-portal. |

### <a name="requesting-tokens"></a>Tokens aanvragen

MSAL biedt twee methoden voor het verkrijgen van tokens in een UWP-app: `AcquireTokenInteractive` en `AcquireTokenSilent`.

#### <a name="get-a-user-token-interactively"></a>Een gebruikerstoken interactief ophalen

In sommige situaties moet dat gebruikers kunnen communiceren met het eindpunt van de Microsoft identity-platform via een pop-upvenster ofwel hun om referenties te valideren of om toestemming te geven. Voorbeelden zijn:

- De eerste keer dat gebruikers zich aanmelden bij de toepassing
- Wanneer gebruikers mogelijk hun referenties opnieuw moeten opgeven omdat het wachtwoord is verlopen
- Wanneer uw toepassing toegang aanvraagt tot een resource, die de gebruiker moet toestemming geven
- Wanneer tweeledige verificatie is vereist

```csharp
authResult = await App.PublicClientApp.AcquireTokenInteractive(scopes)
                      .ExecuteAsync();
```

> |Waar:||
> |---------|---------|
> | `scopes` | Bevat de bereiken die worden aangevraagd, bijvoorbeeld `{ "user.read" }` voor Microsoft Graph of `{ "api://<Application ID>/access_as_user" }` voor aangepaste web-API's. |

#### <a name="get-a-user-token-silently"></a>Een gebruikerstoken op de achtergrond ophalen

Gebruik de `AcquireTokenSilent` methode voor het verkrijgen van tokens voor toegang tot beveiligde resources na de eerste `AcquireTokenAsync` methode. U wilt niet dat de gebruiker voor het valideren van hun referenties telkens wanneer ze nodig hebben voor toegang tot een resource. De meeste gevallen wilt u token aankopen en verlenging zonder tussenkomst van de gebruiker

```csharp
var accounts = await App.PublicClientApp.GetAccountsAsync();
var firstAccount = accounts.FirstOrDefault();
authResult = await App.PublicClientApp.AcquireTokenSilent(scopes, firstAccount)
                                      .ExecuteAsync();
```

> |Waar: ||
> |---------|---------|
> | `scopes` | Bevat de bereiken die worden aangevraagd, bijvoorbeeld `{ "user.read" }` voor Microsoft Graph of `{ "api://<Application ID>/access_as_user" }` voor aangepaste web-API's |
> | `firstAccount` | Hiermee geeft u het eerste gebruikersaccount in de cache (MSAL ondersteunt meerdere gebruikers in een enkele app) |

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Volgende stappen

Volg de zelfstudie voor Windows-bureaublad voor een volledige stapsgewijze handleiding voor het bouwen van toepassingen en nieuwe functies, waaronder een volledige uitleg van deze quickstart.

> [!div class="nextstepaction"]
> [UWP - Zelfstudie voor het aanroepen van Graph API](tutorial-v2-windows-uwp.md)
