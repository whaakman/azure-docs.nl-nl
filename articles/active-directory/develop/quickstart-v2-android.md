---
title: Snelstart voor Azure AD v2 Android | Microsoft Docs
description: Informatie over hoe Android-toepassingen een API kunnen aanroepen die toegangstokens nodig heeft van Azure Active Directory v2.0-eindpunt
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/01/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd78e6acd801f3b973cc45609b72f86b257f4d43
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862757"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Quickstart: Gebruikers aanmelden en de Microsoft Graph API aanroepen vanuit een Android-app

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Deze snelstart bevat een codevoorbeeld die u laat zien hoe een Android-toepassing persoonlijke, werk- en schoolaccounts kan aanmelden, een toegangstoken kan ophalen en de Microsoft Graph API kan aanroepen.

![Laat zien hoe de voorbeeld-app die is gegenereerd door deze Quick Start werkt](media/quickstart-v2-android/android-intro-updated.png)

> [!NOTE]
> **Vereisten**
> * Android Studio 3+
> * Android 21 + is vereist 


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>De quickstart-app registreren en downloaden
> U hebt twee opties voor het starten van de snelstarttoepassing:
> * [Express] [Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Handmatig] [Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> Uw app registreren
> 1. Ga naar de [Azure Portal - Toepassingsregistratie (preview)](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AndroidQuickstartPage/sourceType/docs).
> 1. Voer een naam in voor de toepassing en selecteer **Registreren**.
> 1. Volg de instructies om de nieuwe toepassing met slechts één klik te downloaden en automatisch te configureren.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> Volg deze stappen om de toepassing te registreren en de registratiegegevens van de app handmatig toe te voegen aan uw oplossing:
>
> 1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
> 1. Als u via uw account toegang hebt tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en stelt u de portalsessie in op de gewenste Azure Active Directory-tenant.
> 1. Selecteer in het linkernavigatiedeelvenster de **Azure Active Directory**-service en selecteer vervolgens **App-registraties (preview)** > **Nieuwe registratie**.
> 1. Wanneer de pagina **Een toepassing registreren** verschijnt, voert u de registratiegegevens van de toepassing in:
>      - Voer in de sectie **Naam** een beschrijvende toepassingsnaam. Deze wordt zichtbaar voor gebruikers van de app. Bijvoorbeeld: `Android-Quickstart`.
>      - Klik op de `Register` knop.
> 1. Ga naar `Authentication`  >  `Redirect URIs`  >  `Suggested Redirect URIs for public clients`, en selecteert u de omleidings-URI van de indeling **msal {AppId} :/ / auth**. Sla de wijziging.


> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-your-application"></a>Stap 1: Uw toepassing configureren
> Voor een juiste werking van het codevoorbeeld uit deze snelstart, moet u een antwoord-URL als **msal{AppId}://auth** toevoegen (waarbij {AppId} de toepassings-id is van uw app).
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Deze wijziging hebt aangebracht voor mij]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-android/green-check.png) uw toepassing is geconfigureerd met deze kenmerken

#### <a name="step-2-download-the-project"></a>Stap 2: Het project downloaden

* [Download de Android Studio-Project](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>Stap 3: Het project configureren

> [!div renderon="docs"]
> Als u de optie 1 hierboven hebt geselecteerd, kunt u deze stappen overslaan. Open het project in Android Studio en de app uitvoeren. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Pak het project uit en open het in Android Studio.
> 1. Binnen **app** > **res** > **onbewerkte**Open **auth_config.json**.
> 1. Bewerken **auth_config.json** en vervang de `client_id` en `tenant_id`:
>    ```javascript
>    "client_id" : "Enter_the_Application_Id_Here",
>    "type": "Enter_the_Audience_Info_Here",
>    "tenant_id" : "Enter_the_Tenant_Info_Here"
>    ```
> 1. Binnen **app** > **manifesten**Open **AndroidManifest.xml**.
> 1. Voeg de volgende activiteit toe aan het knooppunt **manifest\application**. Deze code kan Microsoft voor retouraanroep aan uw app:   
>    ```xml
>    <!--Intent filter to catch Microsoft's callback after Sign In-->
>    <activity
>        android:name="com.microsoft.identity.client.BrowserTabActivity">
>        <intent-filter>
>            <action android:name="android.intent.action.VIEW" />
>            <category android:name="android.intent.category.DEFAULT" />
>            <category android:name="android.intent.category.BROWSABLE" />
> 
>            <!--Add in your scheme/host from registered redirect URI-->
>            <!--By default, the scheme should be similar to 'msal[appId]' -->
>            <data android:scheme="msalEnter_The_Application_Id_Here"
>                android:host="auth" />
>        </intent-filter>
>    </activity>
>    ```

> [!div renderon="docs"]
> 1. Pak het project uit en open het in Android Studio.
> 1. Binnen **app** > **res** > **onbewerkte**Open **auth_config.json**.
> 1. Bewerken **auth_config.json** en vervang de `client_id` en `redirect_uri`:
>    ```javascript
>    "client_id" : "ENTER_YOUR_APPLICATION_ID",
>    "redirect_uri": "ENTER_YOUR_REDIRECT_URI", 
>     ```
> 1. Binnen **app** > **manifesten**Open **AndroidManifest.xml**.
> 1. Voeg de volgende activiteit toe aan het knooppunt **manifest\application**. Met dit codefragment wordt een **BrowserTabActivity** geregistreerd, zodat het besturingssysteem uw toepassing kan hervatten wanneer de verificatie is voltooid:
>    ```xml
>    <!--Intent filter to catch Microsoft's callback after Sign In-->
>    <activity
>        android:name="com.microsoft.identity.client.BrowserTabActivity">
>        <intent-filter>
>            <action android:name="android.intent.action.VIEW" />
>            <category android:name="android.intent.category.DEFAULT" />
>            <category android:name="android.intent.category.BROWSABLE" />
> 
>            <!--Add in your scheme/host from registered redirect URI-->
>            <!--By default, the scheme should be similar to 'msal[appId]' -->
>            <data android:scheme="msal<ENTER_YOUR_APPLICATION_ID>"
>                android:host="auth" />
>        </intent-filter>
>    </activity>
>    ```
> 1. Vervang * door de *toepassings-id voor uw toepassing. Als u de *toepassings-id* wilt zoeken, gaat u naar de pagina *Overzicht*.

## <a name="more-information"></a>Meer informatie

Lees de volgende secties voor meer informatie over deze snelstart.

### <a name="msal"></a>MSAL

MSAL ([com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal)) is de bibliotheek die wordt gebruikt voor het aanmelden van gebruikers en het aanvragen van tokens die worden gebruikt voor toegang tot een API die wordt beveiligd met Microsoft Azure AD (Azure Active Directory). U kunt Gradle gebruiken om deze te installeren door het volgende toe te voegen in **Gradle-Scripts** > **build.gradle (Module: app)** onder **Afhankelijkheden**:

```gradle  
implementation 'com.android.volley:volley:1.1.1'
implementation 'com.microsoft.identity.client:msal:0.2.+'
```

### <a name="msal-initialization"></a>MSAL initialiseren

U kunt de verwijzing voor MSAL toevoegen door de volgende code toe te voegen:

```java
import com.microsoft.identity.client.*;
```

Vervolgens initialiseert u MSAL met de volgende code:

```java
    sampleApp = new PublicClientApplication(
        this.getApplicationContext(),
        R.raw.auth_config);
```

> |Waar: ||
> |---------|---------|
> |`R.raw.auth_config` | Dit bestand bevat de configuraties voor uw toepassing met inbegrip van uw App/Client-ID aanmelden doelgroep en verschillende andere opties voor het aanpassen. |

### <a name="requesting-tokens"></a>Tokens aanvragen

MSAL heeft twee methoden die worden gebruikt voor het verkrijgen van tokens: `acquireToken` en `acquireTokenSilentAsync`

#### <a name="getting-a-user-token-interactively"></a>Een gebruikerstoken interactief ophalen

In sommige situaties moeten gebruikers worden gedwongen te communiceren met Azure AD v2.0-eindpunt. Dit leidt tot een contextomschakeling in de systeembrowser om de referenties van de gebruiker te valideren of om toestemming te vragen. Voorbeelden zijn:

* De eerste keer dat gebruikers zich aanmelden bij de toepassing
* Wanneer gebruikers mogelijk hun referenties opnieuw moeten opgeven omdat het wachtwoord is verlopen
* Wanneer via de toepassing toegang wordt aangevraagd tot een resource waarvoor de gebruiker toestemming moet geven
* Wanneer tweeledige verificatie is vereist

```java
sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
```

> |Waar:||
> |---------|---------|
> | `SCOPES` | Bevat de bereiken die worden aangevraagd (dat wil zeggen `{ "user.read" }` voor Microsoft Graph of `{ "<Application ID URL>/scope" }` voor aangepaste web-API's (dat wil zeggen `api://<Application ID>/access_as_user`) |
> | `getAuthInteractiveCallback` | Callback uitgevoerd wanneer besturing na verificatie aan de toepassing wordt teruggegeven |

#### <a name="getting-a-user-token-silently"></a>Een gebruikerstoken op de achtergrond ophalen

U wilt niet dat de gebruiker telkens wanneer deze toegang nodig heeft tot een resource, de referenties moet laten valideren. In de meeste gevallen wilt u tokens ophalen en verlengen zonder tussenkomst van de gebruiker. U kunt methode `AcquireTokenSilentAsync` gebruiken voor het verkrijgen van tokens voor toegang tot beveiligde resources na de eerste methode `acquireToken`:

```java
List<IAccount> accounts = sampleApp.getAccounts();
if (sample.size() == 1) {
    sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
} else {
    // No or multiple accounts
}
```

> |Waar:||
> |---------|---------|
> | `SCOPES` | Bevat de bereiken die worden aangevraagd (dat wil zeggen `{ "user.read" }` voor Microsoft Graph of `{ "<Application ID URL>/scope" }` voor aangepaste web-API's (dat wil zeggen `api://<Application ID>/access_as_user`) |
> | `accounts.get(0)` | De Account die u wilt ophalen van tokens voor op de achtergrond bevat |
> | `getAuthInteractiveCallback` | Callback uitgevoerd wanneer besturing na verificatie aan de toepassing wordt teruggegeven |

## <a name="next-steps"></a>Volgende stappen

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>De stappen voor het maken van de toepassing die wordt gebruikt in deze snelstart

Volg de Android-zelfstudie voor een volledige stapsgewijze handleiding voor het bouwen van toepassingen en nieuwe functies, met inbegrip van een volledige uitleg van deze snelstart.

> [!div class="nextstepaction"]
> [Zelfstudie voor Graph API Android aanroepen](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-android)

### <a name="msal-for-android-library-wiki"></a>MSAL voor Android-bibliotheek wiki

Meer informatie over MSAL-bibliotheek voor Android:

> [!div class="nextstepaction"]
> [MSAL voor Android-bibliotheek wiki](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
