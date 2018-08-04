---
title: Azure AD-Android aan de slag | Microsoft Docs
description: Over het bouwen van een Android-toepassing die kan worden geïntegreerd met Azure AD voor aanmelden en Azure AD-aanroepen beveiligd met behulp van OAuth 2.0 API's.
services: active-directory
documentationcenter: android
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 04/30/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: b428549d7dc804371368d8c252b949ac2fae62c2
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39506253"
---
# <a name="azure-ad-android-getting-started"></a>Azure AD-Android aan de slag
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Als u een Android-toepassing ontwikkelt, maakt Microsoft het eenvoudig en snel te melden bij Azure Active Directory (Azure AD)-gebruikers. Azure AD kan uw toepassing toegang tot gebruikersgegevens via de Microsoft Graph of uw eigen beveiligde web-API. 

De Azure AD Authentication Library (ADAL) Android-bibliotheek biedt uw app de mogelijkheid om te beginnen met behulp van de [Microsoft Azure-Cloud](https://cloud.microsoft.com) & [Microsoft Graph API](https://graph.microsoft.io) door ondersteuning te [ Microsoft Azure Active Directory-accounts](https://azure.microsoft.com/services/active-directory/) met behulp van de bedrijfstak standard OAuth2 en OpenID Connect. In dit voorbeeld ziet u de normale levenscycli die uw toepassing optreden moet, met inbegrip van:

* Een token verkrijgen voor de Microsoft Graph
* Een token vernieuwen
* De Microsoft Graph aanroepen
* De gebruiker afmelden

Als u wilt beginnen, moet u een Azure AD-tenant waar u kunt gebruikers maken en registreren van een toepassing. Als u nog een tenant hebt [informatie over het verkrijgen van een](quickstart-create-new-tenant.md).

## <a name="scenario-sign-in-users-and-call-the-microsoft-graph"></a>Scenario: Meld u aan gebruikers en de Microsoft Graph aanroepen

![Topologie](./media/quickstart-v1-android/active-directory-android-topology.png)

Deze app kan worden gebruikt voor alle Azure AD-accounts. Het ondersteunt zowel één als meerdere organisatie-scenario's (in de stappen besproken). Hierin wordt beschreven hoe een ontwikkelaar bouwen van apps voor de verbinding met zakelijke gebruikers en toegang tot hun Azure + O365 gegevens via de Microsoft Graph. Tijdens de verificatiestroom eindgebruikers moeten zich aanmelden en instemming met de machtigingen van de toepassing en in sommige gevallen kan een beheerder akkoord gaan met de app vereisen. Het merendeel van de logica in dit voorbeeld laat zien hoe auth een eindgebruiker en maken een eenvoudige aanroepen naar de Microsoft Graph.

## <a name="example-code"></a>Voorbeeldcode

U vindt de volledige voorbeeldcode [op Github](https://github.com/Azure-Samples/active-directory-android). 

```Java
// Initialize your app with MSAL
AuthenticationContext mAuthContext = new AuthenticationContext(
        MainActivity.this, 
        AUTHORITY, 
        false);


// Perform authentication requests
mAuthContext.acquireToken(
    getActivity(), 
    RESOURCE_ID, 
    CLIENT_ID, 
    REDIRECT_URI,  
    PromptBehavior.Auto, 
    getAuthInteractiveCallback());

// ...

// Get tokens to call APIs like the Microsoft Graph
mAuthResult.getAccessToken()
```

## <a name="steps-to-run"></a>Stappen om uit te voeren

### <a name="register-and-configure-your-app"></a>Registreren en uw app configureren 
U moet een systeemeigen clienttoepassing die zijn geregistreerd bij Microsoft met behulp van de [Azure-portal](https://portal.azure.com). 

1. Kennismaking met app-registratie
    - Navigeer naar [Azure Portal](https://aad.portal.azure.com). 
    - Selecteer ***Azure Active Directory*** > ***App-registraties***. 

2. De app maken
    - Selecteer **Nieuwe toepassing registreren**. 
    - Voer de naam van een app in de **naam** veld. 
    - In **toepassingstype** Selecteer **systeemeigen**. 
    - In **omleidings-URI**, voer `http://localhost`. 

3. Configureren van Microsoft Graph
    - Selecteer **instellingen > vereiste machtigingen**.
    - Selecteer **toevoegen**, in **Select an API** Selecteer ***Microsoft Graph***. 
    - Selecteer de machtiging **aanmelden en gebruikersprofiel lezen**, drukt u op **Selecteer** om op te slaan. 
        - Deze machtiging wordt toegewezen aan de `User.Read` bereik. 
    - Optioneel: Binnen **vereiste machtigingen > Windows Azure Active Directory**, verwijder de geselecteerde machtiging **aanmelden en gebruikersprofiel lezen**. Dit voorkomt dat de gebruiker toestemming pagina met de machtiging twee keer. 

4. Gefeliciteerd. Uw app is geconfigureerd. In de volgende sectie, hebt u het volgende nodig:
    - `Application ID`
    - `Redirect URI`

### <a name="get-the-sample-code"></a>De voorbeeldcode halen

1. Kloon de code.
    ```
    git clone https://github.com/Azure-Samples/active-directory-android
    ```
2. Open het voorbeeld in Android Studio.
    - Selecteer **een bestaand Android Studio-project openen**.

### <a name="configure-your-code"></a>Uw code configureren

U vindt de configuratie voor dit codevoorbeeld in de ***src/main/java/com/azuresamples/azuresampleapp/MainActivity.java*** bestand. 

1. Vervang de constante `CLIENT_ID` met de `ApplicationID`.
2. Vervang de constante `REDIRECT URI` met de `Redirect URI` u eerder hebt geconfigureerd (`http://localhost`). 

### <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

1. Selecteer **bouwen > Project opschonen**. 
2. Selecteer **uitvoeren > app uitvoeren**. 
3. De app te bouwen en enkele eenvoudige UX. weergeven Wanneer u klikt op de `Call Graph API` knop klikt, wordt gevraagd om een aanmelding en roep vervolgens de Microsoft Graph-API met het nieuwe token op de achtergrond. 

## <a name="important-info"></a>Belangrijke informatie

1. Bekijk de [ADAL Android Wiki](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki) voor meer informatie over de bibliotheek mechanics en het configureren van nieuwe scenario's en mogelijkheden. 
2. In systeemeigen scenario's, de app een ingesloten webweergave wordt gebruikt en wordt de app niet verlaten. De `Redirect URI` mag willekeurige. 
3. Geen problemen voor het zoeken of aanvragen hebben? U kunt maken van een probleem of posten op Stackoverflow met de tag `azure-active-directory`. 

### <a name="cross-app-sso"></a>SSO voor cross-app
Informatie over [hoe u verschillende Apps SSO inschakelen op Android met behulp van ADAL](active-directory-sso-android.md). 

### <a name="auth-telemetry"></a>Auth-telemetrie
auth-telemetrie om te helpen app-ontwikkelaars inzicht in hoe hun apps zich gedragen en betere ervaringen ontwikkeling wordt aangegeven dat de ADAL-bibliotheek. Hiermee kunt u om vast te leggen van de aanmelding geslaagd, actieve gebruikers en verschillende andere interessante inzichten. Met behulp van de telemetrie van de verificatie is vereist voor app-ontwikkelaars tot stand brengen van een telemetrieservice voor het aggregeren en opslaan van gebeurtenissen.

Voor meer informatie over verificatie telemetrie, afhandeling [ADAL Android auth telemetrie](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Telemetry). 

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
