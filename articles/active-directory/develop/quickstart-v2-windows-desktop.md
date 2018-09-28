---
title: Quickstart voor Azure AD v2 Windows-bureaublad | Microsoft Docs
description: Leer hoe met een .NET-toepassing (XAML) voor Windows-bureaublad een token kan worden opgehaald en een API kan worden aangeroepen die is beveiligd met een Azure Active Directory v2.0-eindpunt
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 28c5f025b59b4adbb33a59edd225a839e11dad97
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46965089"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-from-a-windows-desktop-app"></a>Quickstart: Een token verkrijgen en Microsoft Graph API aanroepen vanuit een Windows-bureaublad-app

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

In deze quickstart leert u hoe met een .NET-toepassing (WPF) voor Windows-bureaublad privé-, werk- en schoolaccounts kunnen worden aangemeld, een toegangstoken kan worden opgehaald en de Microsoft Graph API kan worden aangeroepen.

![Werking van de voorbeeld-app die is gegenereerd met deze quickstart](media/quickstart-v2-windows-desktop/windesktop-intro.png)

> [!div renderon="docs"]
> ## <a name="register-and-download"></a>Registreren en downloaden
> ### <a name="register-and-configure-your-application-and-code-sample"></a>Uw toepassing en codevoorbeeld registreren en configureren
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> Ga als volgt te werk om de toepassing te registreren en de registratiegegevens van de toepassing toe te voegen aan uw oplossing:
> 1. Ga naar de [Microsoft-portal voor app-registratie](https://apps.dev.microsoft.com/portal/register-app) om een toepassing te registreren.
> 1. Typ een naam voor de toepassing in het vak **Toepassingsnaam**.
> 1. Zorg ervoor dat het selectievakje **Stapsgewijze instelling** is uitgeschakeld en selecteer vervolgens **Maken**.
> 1. Selecteer **Platform toevoegen**, selecteer **Systeemeigen toepassing** en selecteer vervolgens **Opslaan**.

> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-your-application"></a>Stap 1: Uw toepassing configureren
> Voor een juiste werking van het codevoorbeeld uit deze quickstart moet u een antwoord-URL toevoegen als **urn:ietf:wg:oauth:2.0:oob**.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Deze wijziging voor mij maken]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-windows-desktop/green-check.png) Uw toepassing is al geconfigureerd met deze kenmerken

#### <a name="step-2-download-your-visual-studio-project"></a>Stap 2: Uw Visual Studio-project downloaden

[Het Visual Studio 2017-project downloaden](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>Stap 3: Uw Visual Studio-project configureren

1. Pak het ZIP-bestand uit in een lokale map (bijvoorbeeld **C:\Azure-Samples**)
1. Open het project in Visual Studio.
1. Bewerk **App.Xaml.cs** en vervang de regel die begint met `private static string ClientId`, door de toepassings-id van de toepassing die u zojuist hebt geregistreerd:

```csharp
private static string ClientId = "Enter_the_Application_Id_here";
```

## <a name="more-information"></a>Meer informatie

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)) is de bibliotheek die wordt gebruikt voor het aanmelden van gebruikers en het aanvragen van tokens die worden gebruikt voor toegang tot een API die wordt beveiligd met Microsoft Azure AD (Azure Active Directory). U kunt MSAL installeren door de volgende opdracht uit te voeren in **Package Manager Console** van Visual Studio:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

### <a name="msal-initialization"></a>MSAL initialiseren

U kunt de verwijzing voor MSAL toevoegen door de volgende code toe te voegen:

```csharp
using Microsoft.Identity.Client;
```

Vervolgens initialiseert u MSAL met de volgende code:

```csharp
public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);
```

> |Waar: ||
> |---------|---------|
> | `ClientId` | De toepassings-id van de toepassing die is geregistreerd in *portal.microsoft.com* |

### <a name="requesting-tokens"></a>Tokens aanvragen

MSAL biedt twee methoden om tokens te verkrijgen: `AcquireTokenAsync` en `AcquireTokenSilentAsync`.

#### <a name="get-a-user-token-interactively"></a>Een gebruikerstoken interactief ophalen

In sommige situaties is het nodig om gebruiker via een pop-upvenster te dwingen het Azure AD v2.0-eindpunt te gebruiken om hun referenties te valideren of om toestemming te geven. Voorbeelden zijn:

- De eerste keer dat gebruikers zich aanmelden bij de toepassing
- Wanneer gebruikers mogelijk hun referenties opnieuw moeten opgeven omdat het wachtwoord is verlopen
- Wanneer via de toepassing toegang wordt aangevraagd tot een resource waarvoor de gebruiker toestemming moet geven
- Wanneer tweeledige verificatie is vereist

```csharp
authResult = await App.PublicClientApp.AcquireTokenAsync(_scopes);
```

> |Waar:||
> |---------|---------|
> | `_scopes` | Bevat de bereiken die worden aangevraagd (dat wil zeggen `{ "user.read" }` voor Microsoft Graph of `{ "api://<Application ID>/access_as_user" }` voor aangepaste web-API's) |

#### <a name="get-a-user-token-silently"></a>Een gebruikerstoken op de achtergrond ophalen

U wilt niet dat de gebruiker telkens wanneer deze toegang nodig heeft tot een resource, de referenties moet laten valideren. In de meeste gevallen wilt u tokens ophalen en verlengen zonder tussenkomst van de gebruiker. U kunt de methode `AcquireTokenSilentAsync` voor het verkrijgen van tokens gebruiken voor toegang tot beveiligde resources na de eerste methode `AcquireTokenAsync`:

```csharp
var accounts = await App.PublicClientApp.GetAccountsAsync();
authResult = await App.PublicClientApp.AcquireTokenSilentAsync(scopes, accounts.FirstOrDefault());
```

> |Waar: ||
> |---------|---------|
> |bereiken | Bevat de bereiken die worden aangevraagd (dat wil zeggen `{ "user.read" }` voor Microsoft Graph of `{ "api://<Application ID>/access_as_user" }` voor aangepaste web-API's) |
> |accounts.FirstOrDefault() | De eerste gebruiker in de cache (MSAL biedt ondersteuning voor meerdere gebruikers in één app) |

## <a name="next-steps"></a>Volgende stappen

Volg de zelfstudie voor Windows-bureaublad voor een volledige stapsgewijze handleiding voor het bouwen van toepassingen en nieuwe functies, waaronder een volledige uitleg van deze quickstart.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Leer wat de stappen zijn voor het maken van de toepassing die wordt gebruikt in deze quickstart

> [!div class="nextstepaction"]
> [Zelfstudie voor het aanroepen van Graph API](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-windesktop)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
