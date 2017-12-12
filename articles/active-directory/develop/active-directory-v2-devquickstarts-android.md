---
title: Azure Active Directory v2.0 Android-app | Microsoft Docs
description: Het bouwen van een Android-app die gebruikers met zowel persoonlijke Microsoft-account en werk of schoolaccounts en de Graph API-aanroepen worden aangemeld met behulp van de bibliotheken van derden.
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mtillman
editor: 
ms.assetid: 16294c07-f27d-45c9-833f-7dbb12083794
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: b1c30362a7b14c8f7f0c44d911c46c491b3de3c0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Aanmelden toevoegen aan een Android-app met behulp van een derde partij-bibliotheek met Graph API met behulp van het v2.0-eindpunt
Op het Microsoft Identity-platform wordt gebruikgemaakt van open standaarden, zoals OAuth2 en OpenID Connect. Ontwikkelaars kunnen een bibliotheek die ze willen integreren in onze services gebruiken. Om te helpen ons platform gebruiken met andere bibliotheken ontwikkelaars, hebben we enkele scenario's zoals deze voorbeelden van het configureren van derden bibliotheken verbinding maken met het identiteitsplatform van Microsoft geschreven. De meeste bibliotheken die implementeren [de RFC6749 OAuth2-specificatie](https://tools.ietf.org/html/rfc6749) verbinding kunnen maken met het identiteitsplatform van Microsoft.

Met de toepassing die in dit scenario maakt, kunnen gebruikers zich aanmelden bij hun organisatie en zoekt u naar zichzelf in hun organisatie met behulp van de Graph API.

Als u geen ervaring met OAuth2 of OpenID Connect, wellicht veel van de configuratie van deze niet verstandig aan u. Het is raadzaam dat u leest [2.0-protocollen - stromen van OAuth 2.0 autorisatie Code](active-directory-v2-protocols-oauth-code.md) voor de achtergrond.

> [!NOTE]
> Sommige functies van ons platform die u een expressie in de OAuth2 of OpenID Connect standaarden, zoals voorwaardelijke toegang en beheer van Intune-beleid hebt, moeten u onze open-source bibliotheken voor Microsoft Azure identiteit gebruiken.
> 
> 

Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen.

> [!NOTE]
> Meer informatie over om te bepalen of moet u het v2.0-eindpunt, [v2.0 beperkingen](active-directory-v2-limitations.md).
> 
> 

## <a name="download-the-code-from-github"></a>De code van GitHub downloaden
De code voor deze zelfstudie wordt onderhouden in [GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).  Als u wilt volgen, kunt u [basis van de app downloaden als een ZIP-](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) of het geraamte:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

U kunt ook het voorbeeld downloaden en meteen aan de slag:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Een app registreren
Maakt een nieuwe app op de [toepassing registratieportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of de gedetailleerde stappen op [het registreren van een app met het v2.0-eindpunt](active-directory-v2-app-registration.md).  Zorg ervoor dat:

* Kopieer de **toepassings-Id** die toegewezen aan uw app, omdat u hebt deze snel nodig.
* Voeg de **Mobile** platform voor uw app.

> Opmerking: De registratieportal toepassing biedt een **omleidings-URI** waarde. In dit voorbeeld moet u de standaardwaarde van gebruiken `https://login.microsoftonline.com/common/oauth2/nativeclient`.
> 
> 

## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a>De derde partij NXOAuth2 bibliotheek downloaden en een werkruimte maken
Voor dit scenario gebruikt u de OIDCAndroidLib vanuit GitHub, namelijk een OAuth2-bibliotheek op basis van de code OpenID Connect van Google. Het systeemeigen toepassingsprofiel implementeert en biedt ondersteuning voor het eindpunt voor autorisatie van de gebruiker. Dit zijn de dingen die u wilt integreren met het identiteitsplatform van Microsoft.

Kloon de opslagplaats OIDCAndroidLib op uw computer.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](../media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Uw Android Studio-omgeving instellen
1. Maak een nieuw Android Studio-project en accepteer de standaardinstellingen in de wizard.
   
    ![Nieuw project in Android Studio maken](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)
   
    ![Doel Android-apparaten](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)
   
    ![Een activiteit aan mobile toevoegen](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)
2. Verplaats de gekloonde opslagplaats naar de projectlocatie voordat u uw projectmodules kunt instellen. U kunt ook het project te maken en te klonen rechtstreeks naar de locatie van het project.
   
    ![Projectmodules](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)
3. Open de projectinstellingen modules met behulp van het contextmenu of met behulp van de sneltoets Ctrl + Alt + rimaire + S.
   
    ![Modules projectinstellingen](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)
4. De standaard app-module niet verwijderen omdat u wilt dat alleen de instellingen van de container project.
   
    ![De standaard app-module](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)
5. Modules importeren uit de gekloonde opslagplaats op het huidige project.
   
    ![Project importeren in gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![nieuwe modulepagina maken](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)
6. Herhaal deze stappen voor het `oidlib-sample` module.
7. De afhankelijkheden oidclib controleren op de `oidlib-sample` module.
   
    ![oidclib afhankelijkheden van de module oidlib-voorbeeld](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)
8. Klik op **OK** en wacht tot gradle synchroniseren.
   
    Uw settings.gradle moet eruitzien als:
   
    ![Schermopname van settings.gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)
9. Maken van de voorbeeld-app om ervoor te zorgen dat het voorbeeld juist worden uitgevoerd.
   
    Het niet mogelijk nog gebruiken met Azure Active Directory. Er moet eerst een aantal eindpunten configureren. Dit is om te controleren of er geen problemen met een Android Studio laten we beginnen met het aanpassen van de voorbeeld-app.
10. Bouwen en uitvoeren van `oidlib-sample` als het doel in Android Studio.
    
    ![Voortgang van de build oidlib-voorbeeld](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)
11. Verwijder de `app ` map die is gegeven wanneer u de module verwijderd uit het project omdat Android Studio niet worden verwijderd voor de veiligheid.
    
    ![Structuur van een bestand met de app-map](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)
12. Open de **configuraties bewerken** menu te verwijderen van de configuratie van de uitvoeren die ook is gegeven wanneer u de module verwijderd uit het project.
    
    ![Configuraties menu Bewerken](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![configuratie van de app uitvoeren](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-the-endpoints-of-the-sample"></a>De eindpunten van het voorbeeld configureren
Nu dat u hebt de `oidlib-sample` met succes is uitgevoerd, gaan we sommige eindpunten om deze te kunnen gebruiken met Azure Active Directory bewerken.

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a>Clientcomputers configureren met het bewerken van het bestand oidc_clientconf.xml
1. Omdat u van OAuth2-stromen gebruikmaakt alleen voor een token verkrijgen en de Graph-API aanroepen, moet u de client te doen OAuth2 alleen instellen. OIDC wordt geleverd in een voorbeeld van een hoger.
   
    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```
2. Configureer uw client-ID die u hebt ontvangen van de portal voor wachtwoordregistratie.
   
    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```
3. Configureer uw omleidings-URI met de versie die hieronder.
   
    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```
4. Configureer uw scopes die u nodig hebt voor toegang tot de Graph API.
   
    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

De `User.Read` waarde in `oidc_scopes` kunt u de basisprofiel de ondertekende lezen in het dialoogvenster gebruiker.
U kunt meer informatie over de beschikbare scopes op [Microsoft Graph-machtigingsbereiken](https://graph.microsoft.io/docs/authorization/permission_scopes).

Als u wilt uitleg over `openid` of `offline_access` als scopes in het OpenID Connect, Zie [2.0-protocollen - stromen van OAuth 2.0 autorisatie Code](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a>Uw clienteindpunten configureren met het bewerken van het bestand oidc_endpoints.xml
* Open de `oidc_endpoints.xml` -bestand en de volgende wijzigingen aanbrengen:
  
    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Deze eindpunten moeten nooit wijzigen als u OAuth2 als protocol gebruikt.

> [!NOTE]
> De eindpunten voor `userInfoEndpoint` en `revocationEndpoint` worden momenteel niet ondersteund door Azure Active Directory. Als u deze met de standaardwaarde voorbeeld.com laat, wordt u eraan herinnerd dat ze niet beschikbaar in de steekproef zijn :-)
> 
> 

## <a name="configure-a-graph-api-call"></a>Configureer een Graph API-aanroep
* Open de `HomeActivity.java` -bestand en de volgende wijzigingen aanbrengen:
  
    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Hier wordt een eenvoudige Graph API-aanroep onze informatie geretourneerd.

Dit zijn alle wijzigingen die u moet doen. Voer de `oidlib-sample` toepassing en klik op **aanmelden**.

Nadat u hebt geverifieerd, selecteert u de **een beveiligde bron aanvragen** knop voor het testen van de aanroep van de Graph API.

## <a name="get-security-updates-for-our-product"></a>Beveiligingsupdates voor onze product
We raden u meldingen wilt ontvangen over beveiligingsincidenten in via de [Security TechCenter](https://technet.microsoft.com/security/dd252948) en u te abonneren op Security Advisory Alerts.

