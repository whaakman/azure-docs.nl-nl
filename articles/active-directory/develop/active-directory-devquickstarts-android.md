---
title: Azure AD-Android aan de slag | Microsoft Docs
description: Het bouwen van een Android-toepassing die met Azure AD voor aanmelden en Azure AD-aanroepen integreert beveiligd met behulp van OAuth2.0 API's.
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mtillman
editor: ''
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 04/30/2018
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 7af33005d5984dbf76a16a0daa92f5e1ebf85f9e
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="azure-ad-android-getting-started"></a>Azure AD-Android aan de slag
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Als u een Android-toepassing ontwikkelt, kunt Microsoft u eenvoudig en snel aanmelden van gebruikers van Azure Active Directory (Azure AD). Azure AD kan uw toepassing gebruikersgegevens toegang via het Microsoft Graph of uw eigen beveiligde web-API.  

De Android-bibliotheek voor Azure AD Authentication Library (ADAL), krijgt uw app de mogelijkheid om te beginnen met de [Microsoft Azure Cloud](https://cloud.microsoft.com) & [Microsoft Graph API](https://graph.microsoft.io) door ondersteuning te [ Microsoft Azure Active Directory-accounts](https://azure.microsoft.com/services/active-directory/) met behulp van de branche standard OAuth2 en OpenID Connect. In dit voorbeeld wordt de normale levenscycli die uw toepassing moeten ondervinden zouden, met inbegrip van:

* Ophalen van een token voor Microsoft Graph
* Vernieuwen van een token
* Aanroepen van de Microsoft Graph
* De gebruiker afmelden

Om te beginnen, moet u een Azure AD-tenant kunt u gebruikers maken en een toepassing registreren. Als u niet al een tenant [Lees hoe u een](active-directory-howto-tenant.md).

## <a name="scenario-sign-in-users-and-call-the-microsoft-graph"></a>Scenario: Meld u aan gebruikers en roept u de Microsoft Graph

![Topologie](./media/active-directory-android-topology.png)

Deze app kan worden gebruikt voor alle Azure AD-accounts. Het ondersteunt zowel één als meerdere organisatie-scenario's (in de stappen besproken). Dit laat zien hoe een ontwikkelaar apps om te verbinden met enterprise-gebruikers en toegang tot hun Azure + O365 kunt samenstellen gegevens via het Microsoft Graph. Tijdens de stroom auth eindgebruikers wordt vereist voor aanmelding en toestemming voor de machtigingen van de toepassing en in sommige gevallen kan een beheerder toe te staan de app vereisen. Het merendeel van de logica in dit voorbeeld ziet u hoe auth een eindgebruiker en maak een basic-aanroep om Microsoft Graph.

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

### <a name="register-and-configure-your-app"></a>Registreer en configureer uw app 
U moet een native client-toepassing geregistreerd bij Microsoft via het [Azure-portal](https://portal.azure.com). 

1. Ophalen van app-registratie
    - Navigeer naar [Azure Portal](https://aad.portal.azure.com).  
    - Selecteer ***Azure Active Directory*** > ***App registraties***. 

2. De app maken
    - Selecteer **Nieuwe toepassing registreren**.  
    - Voer de appnaam van een in de **naam** veld. 
    - In **toepassingstype** Selecteer **systeemeigen**. 
    - In **omleidings-URI**, voer `http://localhost`.  

3. Microsoft Graph configureren
    - Selecteer **instellingen > machtigingen vereist**.
    - Selecteer **toevoegen**, in **selecteert u een API** Selecteer ***Microsoft Graph***. 
    - Selecteer de machtiging **aanmelden en gebruikersprofiel lezen**, druk vervolgens op **Selecteer** om op te slaan. 
        - Deze machtiging is toegewezen aan de `User.Read` bereik. 
    - Optioneel: Binnen **vereist machtigingen > Windows Azure Active Directory**, verwijder de machtigingen voor de geselecteerde **aanmelden en gebruikersprofiel lezen**. Hiermee worden de gebruiker toestemming pagina met de machtiging tweemaal voorkomen.   

4. Gefeliciteerd! Uw app is geconfigureerd. In de volgende sectie, hebt u het volgende nodig:
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
2. Selecteer **uitvoeren > Voer app**. 
3. De app moet bouwen en sommige basic UX weergeven Wanneer u klikt op de `Call Graph API` knop klikt, wordt gevraagd om een teken in, en roept u de Microsoft Graph API met het nieuwe token achtergrond.  

## <a name="important-info"></a>Belangrijke informatie

1. Bekijk de [ADAL Android Wiki](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki) voor meer informatie over het mechanisme voor de bibliotheek en het configureren van nieuwe scenario's en mogelijkheden. 
2. In systeemeigen scenario's, is de app een ingesloten Webweergave gebruikt, en laat de app niet. De `Redirect URI` willekeurige kan zijn. 
3. Geen problemen gevonden of aanvragen? U kunt maken van een probleem of post een bericht op Stackoverflow met het label `azure-active-directory`. 

### <a name="cross-app-sso"></a>Cross-app eenmalige aanmelding
Meer informatie over [het inschakelen van eenmalige aanmelding voor cross-app voor Android met behulp van ADAL](active-directory-sso-android.md).  

### <a name="auth-telemetry"></a>Auth-telemetrie
de ADAL-bibliotheek wordt auth telemetrie om te helpen app-ontwikkelaars begrijpen hoe hun apps zich gedragen en betere ervaring bouwen.  Hiermee kunt u voor het vastleggen van aanmelding geslaagd en actieve gebruikers verschillende interessante inzichten. Met behulp van auth telemetrie vereist app-ontwikkelaars tot stand brengen van een service telemetrie voor het aggregeren en opslaan van gebeurtenissen.

Voor meer informatie over auth-telemetrie afhandeling [ADAL Android auth telemetrie](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Telemetry). 

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
