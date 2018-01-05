---
title: Verificatie toevoegen op Apache Cordova met Mobile Apps | Microsoft Docs
description: "Informatie over het verifiëren van gebruikers van uw Apache Cordova-app via een groot aantal identiteitsproviders, waaronder Google, Facebook, Twitter en Microsoft met Mobile Apps in Azure App Service."
services: app-service\mobile
documentationcenter: javascript
author: conceptdev
manager: crdun
editor: 
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: b5cce832ae7ae83552c2a5ded2f5f5bda0ac76bf
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="add-authentication-to-your-apache-cordova-app"></a>Verificatie toevoegen aan uw Apache Cordova-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Samenvatting
In deze zelfstudie kunt u verificatie toevoegen aan de todolist-Quick Start-project op Apache Cordova met behulp van een ondersteunde id-provider. Deze zelfstudie is gebaseerd op de [aan de slag met Mobile Apps] zelfstudie die u moet eerst te voltooien.

## <a name="register"></a>Uw app registreren voor verificatie en de App Service configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Bekijk een video van vergelijkbare stappen](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <a name="permissions"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nu kunt u controleren of anonieme toegang tot uw back-end is uitgeschakeld. In Visual Studio:

* Open het project dat u hebt gemaakt toen u de zelfstudie voltooid [aan de slag met Mobile Apps].
* Uw toepassing uitvoeren in de **Google Android-Emulator**.
* Controleer of er een onverwachte fout in de verbinding wordt weergegeven nadat de app is gestart.

Werk vervolgens de app om gebruikers te verifiëren voordat u resources van de back-end voor de mobiele App.

## <a name="add-authentication"></a>Verificatie toevoegen aan de app.
1. Open het project in **Visual Studio**, open vervolgens de `www/index.html` bestand voor bewerken.
2. Zoek de `Content-Security-Policy` meta-code in het gedeelte head.  De OAuth host toevoegt aan de lijst met toegestane bronnen.

   | Provider | De naam van de SDK-Provider | OAuth-Host |
   |:--- |:--- |:--- |
   | Azure Active Directory | AAD | https://login.microsoftonline.com |
   | Facebook | Facebook | https://www.Facebook.com |
   | Google | Google | https://accounts.Google.com |
   | Microsoft | MicrosoftAccount | https://login.live.com |
   | Twitter | Twitter | https://API.Twitter.com |

    Een voorbeeld van de inhoud-Security-beleid (toegepast voor Azure Active Directory) is als volgt:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    Vervang `https://login.microsoftonline.com` met de OAuth-host uit de voorgaande tabel.  Zie voor meer informatie over de inhoud beveiligingsbeleid meta-code, de [inhoud beveiligingsbeleid documentatie].

    Sommige verificatieproviders vereisen geen dat inhoud beveiligingsbeleid wijzigingen bij op de juiste mobiele apparaten.  Er zijn geen inhoud beveiligingsbeleid wijzigingen zijn vereist met Google-verificatie op een Android-apparaat.

3. Open de `www/js/index.js` voor het bewerken van het bestand, zoek de `onDeviceReady()` methode, en onder het maken van de client code toe te voegen met de volgende code:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Deze code vervangt de bestaande code die de tabelverwijzing maakt en de gebruikersinterface vernieuwt.

    De methode login() begint verificatie met de provider. De methode login() is een async-functie die een JavaScript-belofte retourneert.  De rest van de initialisatie van de wordt in het antwoord CTP geplaatst zodat deze wordt niet uitgevoerd totdat de login()-methode is voltooid.

4. Vervang in de code die u zojuist hebt toegevoegd, `SDK_Provider_Name` met de naam van de aanmeldingsprovider van uw. Bijvoorbeeld: voor Azure Active Directory, gebruiken `client.login('aad')`.
5. Voer uw project.  Wanneer het project is geïnitialiseerd, ziet uw toepassing de OAuth-aanmeldingspagina voor de gekozen verificatieprovider.

## <a name="next-steps"></a>De volgende stappen
* Meer informatie [over verificatie] met Azure App Service.
* De zelfstudie gaan door toe te voegen [Pushmeldingen] aan uw Apache Cordova-app.

Informatie over het gebruik van de SDK's.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- URLs. -->
[aan de slag met Mobile Apps]: app-service-mobile-cordova-get-started.md
[inhoud beveiligingsbeleid documentatie]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Pushmeldingen]: app-service-mobile-cordova-get-started-push.md
[over verificatie]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
