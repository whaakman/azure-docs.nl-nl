---
title: Microsoft identity-platform Android Quick Start | Azure
description: Informatie over hoe Android-toepassingen een waarvoor toegangstokens door Microsoft identity platform eindpunt API kan aanroepen.
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
ms.date: 04/26/2019
ms.author: ryanwi
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 547eafac8cc1acf2b60416f93804e819a1c549b0
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67702763"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Quickstart: Gebruikers aanmelden en de Microsoft Graph API aanroepen vanuit een Android-app

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Deze snelstart bevat een codevoorbeeld die u laat zien hoe een Android-toepassing persoonlijke, werk- en schoolaccounts kan aanmelden, een toegangstoken kan ophalen en de Microsoft Graph API kan aanroepen.

![Laat zien hoe de voorbeeld-app die is gegenereerd door deze Quick Start werkt](media/quickstart-v2-android/android-intro.svg)

> [!NOTE]
> **Vereisten**
> * Android Studio 
> * Android 16 + is vereist 


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>De quickstart-app registreren en downloaden
> U hebt twee opties voor het starten van de snelstarttoepassing:
> * [Express] [Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Handmatig] [Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> Uw app registreren
> 1. Ga naar de nieuwe [Azure portal - App-registraties](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AndroidQuickstartPage/sourceType/docs) deelvenster.
> 1. Voer een naam in voor de toepassing en selecteer **Registreren**.
> 1. Volg de instructies om de nieuwe toepassing met slechts één klik te downloaden en automatisch te configureren.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> Volg deze stappen om de toepassing te registreren en de registratiegegevens van de app handmatig toe te voegen aan uw oplossing:
>
> 1. Navigeer naar de Microsoft identity-platform voor ontwikkelaars [App-registraties](https://aka.ms/MobileAppReg) pagina.
> 1. Selecteer **registratie van nieuwe**.
> 1. Wanneer de pagina **Een toepassing registreren** verschijnt, voert u de registratiegegevens van de toepassing in:
>      - Voer in de sectie **Naam** een beschrijvende toepassingsnaam. Deze wordt zichtbaar voor gebruikers van de app. Bijvoorbeeld: `AndroidQuickstart`.
>      - Op deze pagina kunt u andere configuraties overslaan. 
>      - Klik op de `Register` knop.
> 1. Klik op de nieuwe app > Ga naar `Authentication`  >  `Add Platform`  >  `Android`.    
>      - Voer de naam van het pakket van uw Android studio-project. 
>      - Een handtekening-Hash genereren. Verwijzen naar de portal voor instructies.
> 1. Selecteer `Configure` en sla de ***MSAL configuratie*** JSON voor later. 

> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-your-application"></a>Stap 1: Uw toepassing configureren
> Voor het codevoorbeeld voor deze Quick Start om te werken, moet u een omleidings-URI die compatibel is met de broker Auth toevoegen. 
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Deze wijziging voor mij maken]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-android/green-check.png) Uw toepassing is al geconfigureerd met deze kenmerken

#### <a name="step-2-download-the-project"></a>Stap 2: Het project downloaden

* [De voorbeeldcode downloaden](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>Stap 3: Het project configureren

> [!div renderon="docs"]
> Als u de optie 1 hierboven hebt geselecteerd, kunt u deze stappen overslaan. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Pak het project uit en open het in Android Studio.
> 1. Binnen **app** > **src** > **belangrijkste** > **res**  >   **onbewerkte**Open **auth_config.json**.
> 1. Bewerken **auth_config.json** en vervang deze door de JSON van de Azure-portal. Als u in plaats daarvan de wijzigingen handmatig aanbrengen wilt:
>    ```javascript
>    {
>       "client_id" : "Enter_the_Application_Id_Here",
>       "authorization_user_agent" : "DEFAULT",
>       "redirect_uri" : "Enter_the_Redirect_Uri_Here",
>       "authorities" : [
>          {
>             "type": "AAD",
>             "audience": {
>                "type": "Enter_the_Audience_Info_Here",
>                "tenant_id": "Enter_the_Tenant_Info_Here"
>             }
>          }
>       ]
>    }
> 1. Inside **app** > **manifests**, open  **AndroidManifest.xml**.
> 1. Paste the following activity to the **manifest\application** node: 
>    ```xml
>    <!--Intent filter to catch Microsoft's callback after Sign In-->
>    <activity
>        android:name="com.microsoft.identity.client.BrowserTabActivity">
>        <intent-filter>
>            <action android:name="android.intent.action.VIEW" />
>            <category android:name="android.intent.category.DEFAULT" />
>            <category android:name="android.intent.category.BROWSABLE" />
>            <data android:scheme="msauth"
>                android:host="Enter_the_Package_Name"
>                android:path="/Enter_the_Signature_Hash" />
>        </intent-filter>
>    </activity>
>    ```
> > 1. De app uitvoeren. 

> [!div renderon="docs"]
> 1. Pak het project uit en open het in Android Studio.
> 1. Binnen **app** > **res** > **onbewerkte**Open **auth_config.json**.
> 1. Bewerken **auth_config.json** en vervang deze door de JSON van de Azure-portal. Als u in plaats daarvan deze wijzigingen handmatig aanbrengen wilt:
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
>            <data android:scheme="msauth"
>                android:host="Enter_the_Package_Name"
>                android:path="/Enter_the_Decoded_Signature_Hash" />
>        </intent-filter>
>    </activity>
>    ```
> 1. Vervang `Enter_the_Package_Name` en `Enter_the_Signature_Hash` door de waarden die u in de Azure-portal hebt geregistreerd. 
> 1. De app uitvoeren. 

## <a name="more-information"></a>Meer informatie

Lees de volgende secties voor meer informatie over deze snelstart.

### <a name="getting-msal"></a>MSAL ophalen

MSAL ([com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal)) is de bibliotheek gebruikt voor het aanmelden van gebruikers en aanvragen van tokens die worden gebruikt voor toegang tot een API die wordt beveiligd door Microsoft identity-platform. U kunt Gradle 3.0 + gebruiken ze deze willen installeren door toe te voegen in het volgende **Gradle-Scripts** > **build.gradle (Module: app)** onder **afhankelijkheden**:

```gradle  
implementation 'com.android.volley:volley:1.1.1'
implementation 'com.microsoft.identity.client:msal:0.3.+'
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
> |`R.raw.auth_config` | Dit bestand bevat de configuraties voor uw toepassing met inbegrip van uw App/Client-ID, aanmelden doelgroep omleidings-URI en verschillende andere opties voor het aanpassen. |

### <a name="requesting-tokens"></a>Tokens aanvragen

MSAL biedt twee methoden om tokens te verkrijgen: `acquireToken` en `acquireTokenSilentAsync`

#### <a name="acquiretoken-getting-a-token-interactively"></a>acquireToken: Ophalen van een token interactief

In sommige situaties moet gebruikers kunnen communiceren met Microsoft identity-platform. In dergelijke gevallen kan de eindgebruiker hun account selecteren, zijn referenties invoeren of toestemming geven voor de machtigingen die uw app heeft aangevraagd worden vereist. Bijvoorbeeld: 

* De eerste keer dat gebruikers zich aanmelden bij de toepassing
* Als een gebruiker hun wachtwoord opnieuw instellen, moet ze hun referenties invoeren 
* Als u toestemming is ingetrokken 
* Als uw app expliciet toestemming nodig. 
* Wanneer uw toepassing toegang aanvraagt tot een resource voor de eerste keer
* Als MFA of andere beleidsregels voor voorwaardelijke toegang vereist zijn

```java
sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
```

> |Waar:||
> |---------|---------|
> | `SCOPES` | Bevat de bereiken die worden aangevraagd (dat wil zeggen `{ "user.read" }` voor Microsoft Graph of `{ "<Application ID URL>/scope" }` voor aangepaste web-API's (dat wil zeggen `api://<Application ID>/access_as_user`) |
> | `getAuthInteractiveCallback` | Callback uitgevoerd wanneer besturing na verificatie aan de toepassing wordt teruggegeven |

#### <a name="acquiretokensilent-getting-a-user-token-silently"></a>acquireTokenSilent: Een gebruikerstoken op de achtergrond ophalen

Apps al dan niet mogen moeten gebruikers melden telkens wanneer ze een token aanvragen. Als de gebruiker al heeft aangemeld, kan deze methode apps aanvragen tokens op de achtergrond.

```java
    sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
        @Override
        public void onAccountsLoaded(final List<IAccount> accounts) {

            if (!accounts.isEmpty()) {
                sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
            } else {
                /* No accounts */
            }
        }
    });
```

> |Waar:||
> |---------|---------|
> | `SCOPES` | Bevat de bereiken die worden aangevraagd (dat wil zeggen `{ "user.read" }` voor Microsoft Graph of `{ "<Application ID URL>/scope" }` voor aangepaste web-API's (dat wil zeggen `api://<Application ID>/access_as_user`) |
> | `getAccounts(...)` | De Account die u wilt ophalen van tokens voor op de achtergrond bevat |
> | `getAuthSilentCallback()` | Callback uitgevoerd wanneer besturing na verificatie aan de toepassing wordt teruggegeven |

## <a name="next-steps"></a>Volgende stappen

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>De stappen voor het maken van de toepassing die wordt gebruikt in deze snelstart

Volg de Android-zelfstudie voor een volledige stapsgewijze handleiding voor het bouwen van toepassingen en nieuwe functies, met inbegrip van een volledige uitleg van deze snelstart.

> [!div class="nextstepaction"]
> [Android-zelfstudie voor Graph API-aanroepen](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-android)

### <a name="msal-for-android-library-wiki"></a>MSAL voor Android-bibliotheek wiki

Meer informatie over MSAL-bibliotheek voor Android:

> [!div class="nextstepaction"]
> [MSAL voor Android-bibliotheek wiki](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
