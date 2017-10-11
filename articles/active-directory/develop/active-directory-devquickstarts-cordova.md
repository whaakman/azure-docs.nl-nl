---
title: Azure AD-Cordova aan de slag | Microsoft Docs
description: "Het bouwen van een Cordova-toepassing die kan worden geïntegreerd met Azure AD voor aanmelden en Azure AD-beveiligde API aanroept met behulp van OAuth."
services: active-directory
documentationcenter: 
author: vibronet
manager: mbaldwin
editor: 
ms.assetid: b1a8d7bd-7ad6-44d5-8ccb-5255bb623345
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: vittorib
ms.custom: aaddev
ms.openlocfilehash: d9f53148787729d29a0a89cce1b8b2b83ba228f8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Azure AD integreren met een Apache Cordova-app
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

U kunt Apache Cordova HTML5/JavaScript-toepassingen die kunnen worden uitgevoerd op mobiele apparaten als volwaardig systeemeigen toepassingen ontwikkelen. Met Azure Active Directory (Azure AD), kunt u verificatiefuncties bedrijfsniveau toevoegen aan uw Cordova-toepassingen.

Een invoegtoepassing Cordova verpakt Azure AD systeemeigen SDK's op iOS, Android, Windows Store en Windows Phone. Met dat invoegtoepassing, u door uw toepassing verbeteren kunt ter ondersteuning van aanmelden met Windows Server Active Directory-accounts voor uw gebruikers toegang krijgen tot Office 365 en Azure-API's en zelfs bescherming van uw eigen aangepaste web-API aanroepen.

In deze zelfstudie gebruiken we de Apache Cordova-invoegtoepassing voor Active Directory Authentication Library (ADAL) voor het verbeteren van een eenvoudige app door de volgende functies toe te voegen:

* Verifiëren van een gebruiker met een paar regels code, en een token verkrijgen.
* Gebruik dat token aan te roepen de Graph API om te vragen die map en de resultaten weer te geven.  
* Gebruik de ADAL tokencache verificatie wordt u gevraagd om de gebruiker minimaliseren.

Als u deze verbeteringen maken, moet u:

1. U registreert een toepassing met Azure AD.
2. Voeg code toe aan uw app om aanvragen van tokens.
3. Voeg de code voor het gebruik van het token voor query's in de Graph API en resultaten weer te geven.
4. De implementatie Cordova-project maken met de platforms die u wilt richten, de Cordova ADAL invoegtoepassing toevoegen en testen van de oplossing in emulators.

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een Azure AD-tenant waarin u een account met app-ontwikkeling rechten hebben.
* Een ontwikkelingsomgeving die geconfigureerd voor gebruik van Apache Cordova.  

Als u beide al hebt ingesteld, doorgaan met stap 1.

Als u geen Azure AD-tenant, gebruikt u de [instructies voor het ophalen van een](active-directory-howto-tenant.md).

Als u geen Apache Cordova instellen op uw computer, installeert u het volgende:

* [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [Node.js](https://nodejs.org/download/)
* [Cordova CLI](https://cordova.apache.org/) (kan gemakkelijk worden geïnstalleerd via NPM Pakketbeheer: `npm install -g cordova`)

De voorgaande installaties moeten werken zowel op de PC als op de Mac.

Elk doelplatform heeft verschillende vereisten:

* Het bouwen en uitvoeren van een app voor Windows-Tablet/PC of Windows Phone:
  * Installeer [Visual Studio 2013 voor Windows met Update 2 of hoger](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express of een andere versie) of [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).

* Het bouwen en uitvoeren van een app voor iOS:

  * Installeer Xcode 6.x of hoger. Downloaden van de [Apple Developer site](http://developer.apple.com/downloads) of de [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).
  * Installeer [ios-sim](https://www.npmjs.org/package/ios-sim). U kunt het starten van iOS-apps in iOS-Simulator vanaf de opdrachtregel. (U kunt het eenvoudig installeren via de terminal: `npm install -g ios-sim`.)
* Het bouwen en uitvoeren van een app voor Android:

  * Installeer [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) of hoger. Zorg ervoor dat `JAVA_HOME` (omgevingsvariabele) correct is ingesteld in overeenstemming met het installatiepad JDK (bijvoorbeeld C:\Program Files\Java\jdk1.7.0_75).
  * Installeer [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) en voeg de `<android-sdk-location>\tools` locatie (bijvoorbeeld C:\tools\Android\android-sdk\tools) voor uw `PATH` omgevingsvariabele.
  * Android SDK Manager openen (bijvoorbeeld via de terminal: `android`) en te installeren:
    * *Android 5.0.1 (API 21)* platform SDK
    * *Android SDK-Build Tools* versie 19.1.0 of hoger
    * *Android-ondersteuning opslagplaats* (extra's)

  De Android SDK biedt een standaardexemplaar van de emulator niet. Maken van een door het uitvoeren van `android avd` vanaf de terminal en selecteren **maken**, als u wilt uitvoeren van de Android-app op een emulator. U wordt aangeraden een API-niveau van 19 of hoger. Zie voor meer informatie over de Android-emulator en het maken van opties [AVD Manager](http://developer.android.com/tools/help/avd-manager.html) op de Android-site.

## <a name="step-1-register-an-application-with-azure-ad"></a>Stap 1: Een toepassing registreren met Azure AD
Deze stap is optioneel. Deze zelfstudie bevat vooraf is ingericht waarden die u gebruiken kunt om te zien van het voorbeeld in actie zonder eventuele inrichten in uw eigen tenant. We raden echter aan dat u deze stap uitvoeren en vertrouwd met het proces, raken omdat deze vereist is wanneer u uw eigen toepassingen maken.

Azure AD geeft tokens alleen bekende toepassingen. Voordat u van uw app kunt u Azure AD, moet u een vermelding maken voor het in uw tenant. Een nieuwe toepassing registreren in uw tenant:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op uw account op de bovenste balk. In de **Directory** kiest u de Azure AD-tenant waar u uw toepassing registreren.
3. Klik op **meer Services** in het linkerdeelvenster en selecteer vervolgens **Azure Active Directory**.
4. Klik op **App registraties**, en selecteer vervolgens **toevoegen**.
5. Volg de aanwijzingen en maak een **systeemeigen clienttoepassing**. (Hoewel de Cordova-apps zijn op basis van HTML, we creëren een native client-toepassing. De **systeemeigen clienttoepassing** optie moet zijn geselecteerd of de toepassing werkt niet.)
  * **Naam** beschrijving van uw toepassing voor gebruikers.
  * **Omleidings-URI** is de URI die wordt gebruikt voor tokens terug naar uw app. Voer **http://MyDirectorySearcherApp**.

Nadat u de registratie, wijst Azure AD een unieke toepassings-ID toe aan uw app. U moet deze waarde in de volgende secties. U vindt deze op het toepassingstabblad van de nieuwe app.

Om uit te voeren `DirSearchClient Sample`, de zojuist gemaakte app machtiging query uitvoeren op de Azure AD Graph API:

1. Van de **instellingen** pagina **Required Permissions**, en selecteer vervolgens **toevoegen**.  
2. Selecteer voor de toepassing Azure Active Directory **Microsoft Graph** als de API en voeg de **toegang tot de map als de gebruiker is aangemeld** machtiging onder **gedelegeerde machtigingen**.  Hierdoor wordt uw toepassing query uitvoeren op de Graph-API voor gebruikers.

## <a name="step-2-clone-the-sample-app-repository"></a>Stap 2: De voorbeeld-app-opslagplaats klonen
Typ de volgende opdracht uit uw shell of vanaf de opdrachtregel:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="step-3-create-the-cordova-app"></a>Stap 3: De Cordova-app maken
Er zijn meerdere manieren om Cordova-toepassingen te maken. In deze zelfstudie gebruikt u de Cordova-opdrachtregelinterface (CLI).

1. Typ de volgende opdracht uit uw shell of vanaf de opdrachtregel:

        cordova create DirSearchClient

   Deze opdracht maakt u de mapstructuur en steigers voor de Cordova-project.

2. Verplaatsen naar de nieuwe DirSearchClient map:

        cd .\DirSearchClient

3. Kopieer de inhoud van het starter-project in de submap www met behulp van de manager van een bestand of de volgende opdracht in uw shell:

  * Windows:`xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`
  * Mac:`cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`

4. De invoegtoepassing goedgekeurde toevoegen. Dit is nodig om de Graph API aan te roepen.

        cordova plugin add cordova-plugin-whitelist

5. Voeg de platforms die u wilt ondersteunen. Als u een voorbeeld van een werken, moet u ten minste één van de volgende opdrachten uitvoeren. Houd er rekening mee dat u niet kan worden geëmuleerd iOS op Windows of emuleren van Windows op een Mac.

        cordova platform add android
        cordova platform add ios
        cordova platform add windows

6. De ADAL voor Cordova-invoegtoepassing toevoegen aan uw project:

        cordova plugin add cordova-plugin-ms-adal

## <a name="step-4-add-code-to-authenticate-users-and-obtain-tokens-from-azure-ad"></a>Stap 4: Code toevoegen om te verifiëren van gebruikers en tokens verkrijgen bij Azure AD
De toepassing die u in deze zelfstudie ontwikkelt bieden een eenvoudige directory zoekfunctie. De gebruiker kan vervolgens typt u de alias van een gebruiker in de map en visualiseren van enkele eenvoudige kenmerken. Het starter-project bevat de definitie van de algemene gebruikersinterface van de app (in www/index.html) en de steiger die van de basis-Apps gebeurtenis kabels cycli, gebruikersinterface bindingen, en resultaten weergeven logica (in www/js/index.js). De enige taak resteren voor u is het toevoegen van de logica die identiteit taken implementeert.

Het eerste wat dat u moet doen in uw code is introduceren van het protocol-waarden die Azure AD worden gebruikt voor het identificeren van uw app en de resources die u zijn gericht. Deze waarden worden gebruikt om de aanvragen voor beveiligingstokens later samen te stellen. Voeg het volgende codefragment aan de bovenkant van het bestand index.js:

```javascript
var authority = "https://login.microsoftonline.com/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

De `redirectUri` en `clientId` waarden moeten overeenkomen met de waarden die uw app in Azure AD te beschrijven. U kunt vinden die uit de **configureren** tabblad in de Azure portal, zoals beschreven in stap 1 eerder in deze zelfstudie.

> [!NOTE]
> Als u hebt gekozen voor het registreren van een nieuwe app niet in uw eigen tenant, kunt u gewoon de vooraf geconfigureerde waarden ongewijzigd plakken. Vervolgens ziet u het voorbeeld wordt uitgevoerd, hoewel altijd moet u uw eigen vermelding maken voor uw apps die zijn bedoeld voor productie.

Voeg vervolgens de tokenaanvraag-code. Voeg het volgende fragment tussen de `search` en `renderData` definities:

```javascript
    // Shows the user authentication dialog box if required
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize the user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers the authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
Door deze te splitsen in de twee belangrijkste delen behandeld die functie.
Dit voorbeeld is ontworpen voor gebruik met een tenant, in plaats van wordt gekoppeld aan een bepaald. Het eindpunt '/ algemene', die kan de gebruiker een account in verificatie keer invoeren en stuurt de aanvraag naar de tenant waar hoort wordt gebruikt.

Deze eerste deel van de methode inspecteert de ADAL-cache om te zien is al een token opgeslagen. Zo ja, de methode maakt gebruik van de tenants waar het token vandaan komt voor het initialiseren van ADAL. Dit is nodig om te voorkomen dat extra prompts omdat het gebruik van '/ algemene' altijd resulteert in de gebruiker een nieuwe account in te voeren wordt gevraagd.

```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Het tweede gedeelte van de methode voert de juiste tokenaanvraag. De `acquireTokenSilentAsync` methode vraagt ADAL te retourneren van een token voor de opgegeven resource zonder dat u eventuele UX weergegeven Dat kan gebeuren als de cache al een geschikte toegangstoken opgeslagen heeft, of als een vernieuwingstoken kan worden gebruikt om een nieuw toegangstoken ophalen zonder dat een prompt weergegeven. Als dat lukt, we terugvallen op `acquireTokenAsync`--die de gebruiker authenticatie verricht zichtbaar wordt gevraagd.

```javascript
            // Attempt to authorize the user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers the authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Nu dat we het token hebben, kunnen we ten slotte de Graph API aanroepen en de query die we willen uitvoeren. Voeg het volgende fragment hieronder de `authenticate` definitie:

```javascript
// Makes an API call to receive the user list
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
Een eenvoudige UX startpunt bestanden opgegeven voor het invoeren van de alias van een gebruiker in het tekstvak. Deze methode wordt die waarde voor maakt u een query, combineren met het toegangstoken verzenden naar Microsoft Graph en parseren van de resultaten. De `renderData` methode, al aanwezig in het bestand startpunt zorgt voor de resultaten te visualiseren.

## <a name="step-5-run-the-app"></a>Stap 5: De app uitvoeren
Uw app is ten slotte gereed om uit te voeren. Het besturingssysteem is eenvoudig: wanneer de app wordt gestart, voer de alias van de gebruiker die u wilt zoeken en klik vervolgens op de knop. U wordt gevraagd om de verificatie. Bij de verificatie is geslaagd en zoekactie, worden de kenmerken van de gezochte gebruiker weergegeven.

Latere uitvoeringen zal de zoekopdracht uitvoeren zonder dat een prompt dankzij de aanwezigheid van het eerder aangeschafte token in cache worden weergegeven.

De concrete stappen voor het uitvoeren van de app, varieert per platform.

### <a name="windows-10"></a>Windows 10
   Tablet PC:`cordova run windows --archs=x64 -- --appx=uap`

   Mobile (vereist een Windows 10 Mobile-apparaat dat is verbonden met een PC):`cordova run windows --archs=arm -- --appx=uap --phone`

   > [!NOTE]
   > Tijdens de eerste keer uitvoert, wordt u mogelijk gevraagd aan te melden voor een ontwikkelaarslicentie. Zie voor meer informatie [ontwikkelaarslicentie](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

### <a name="windows-81-tabletpc"></a>Windows 8.1 Tablet PC
   `cordova run windows`

   > [!NOTE]
   > Tijdens de eerste keer uitvoert, wordt u mogelijk gevraagd aan te melden voor een ontwikkelaarslicentie. Zie voor meer informatie [ontwikkelaarslicentie](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

### <a name="windows-phone-81"></a>Windows Phone 8.1
   Uitvoeren op een verbonden apparaat:`cordova run windows --device -- --phone`

   Uitvoeren op de standaardemulator:`cordova emulate windows -- --phone`

   Gebruik `cordova run windows --list -- --phone` voor een overzicht van alle beschikbare doelen en `cordova run windows --target=<target_name> -- --phone` voor het uitvoeren van de toepassing op een specifiek apparaat of emulator (bijvoorbeeld `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

### <a name="android"></a>Android
   Uitvoeren op een verbonden apparaat:`cordova run android --device`

   Uitvoeren op de standaardemulator:`cordova emulate android`

   Zorg ervoor dat u hebt een exemplaar van de emulator gemaakt met behulp van AVD Manager, zoals eerder beschreven in de sectie 'Vereisten'.

   Gebruik `cordova run android --list` voor een overzicht van alle beschikbare doelen en `cordova run android --target=<target_name>` voor het uitvoeren van de toepassing op een specifiek apparaat of emulator (bijvoorbeeld `cordova run android --target="Nexus4_emulator"`).

### <a name="ios"></a>iOS
   Uitvoeren op een verbonden apparaat:`cordova run ios --device`

   Uitvoeren op de standaardemulator:`cordova emulate ios`

   > [!NOTE]
   > Zorg ervoor dat u hebt de `ios-sim` pakket geïnstalleerd voor uitvoering van de emulator. Zie de sectie 'Vereisten' voor meer informatie.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run the application on specific device or emulator (for example, `cordova run android --target="iPhone-6"`).

    Use `cordova run --help` to see additional build and run options.

## <a name="next-steps"></a>Volgende stappen
Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) is beschikbaar in [GitHub](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).

U kunt nu verder met geavanceerdere (en interessante)-scenario's. U wilt proberen: [beveiligen van een Node.js-Web-API met Azure AD](active-directory-devquickstarts-webapi-nodejs.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
