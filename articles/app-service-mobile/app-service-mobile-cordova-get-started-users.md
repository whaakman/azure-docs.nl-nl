---
title: Verificatie op Apache Cordova met Mobile Apps toevoegen | Microsoft Docs
description: Informatie over het gebruik van Mobile Apps in Azure App Service voor verificatie van gebruikers van uw Apache Cordova-app via een groot aantal identiteitsproviders, waaronder Google, Facebook, Twitter en Microsoft.
services: app-service\mobile
documentationcenter: javascript
author: elamalani
manager: crdun
editor: ''
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: b0634038dbf5771ac1aa0bc00d007e758171b238
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443514"
---
# <a name="add-authentication-to-your-apache-cordova-app"></a>Verificatie toevoegen aan uw Apache Cordova-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

> [!NOTE]
> Visual Studio App Center investeert in nieuwe en geïntegreerde services centraal staat in de ontwikkeling van mobiele Apps. Ontwikkelaars kunnen gebruikmaken van **bouwen**, **Test** en **verdelen** services voor het instellen van de pijplijn voor continue integratie en levering. Zodra de app is geïmplementeerd, ontwikkelaars controleren de status en het gebruik van het gebruik van de app de **Analytics** en **Diagnostics** -services en Communiceer met gebruikers met behulp van de **Push** de service. Ontwikkelaars kunnen ook gebruikmaken van **Auth** om hun gebruikers te verifiëren en **gegevens** service behouden en synchroniseren van app-gegevens in de cloud. Bekijk [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-cordova-get-started-users) vandaag nog.
>

## <a name="summary"></a>Samenvatting
In deze zelfstudie, kunt u verificatie toevoegen aan de todolist Quick Start-project op Apache Cordova met behulp van een ondersteunde id-provider. In deze zelfstudie is gebaseerd op de [aan de slag met Mobile Apps] van de zelfstudie moet u eerst uitvoeren.

## <a name="register"></a>Uw app registreren voor verificatie en de App Service configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Bekijk een video van vergelijkbare stappen](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <a name="permissions"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nu kunt u controleren dat anonieme toegang tot uw back-end is uitgeschakeld. In Visual Studio:

* Open het project dat u hebt gemaakt toen u de zelfstudie voltooid [aan de slag met Mobile Apps].
* Uw toepassing uitvoert in de **Google Android-Emulator**.
* Controleer of een onverwachte fout voor de verbinding wordt weergegeven nadat de app wordt gestart.

Werk vervolgens de app om gebruikers te verifiëren voordat u resources van de back-end van Mobile App aanvraagt.

## <a name="add-authentication"></a>Verificatie toevoegen aan de app.
1. Open het project in **Visual Studio**en open vervolgens de `www/index.html` bestand voor het bewerken van.
2. Zoek de `Content-Security-Policy` meta-code in de sectie head.  De OAuth-host aan de lijst met toegestane bronnen toevoegen.

   | Provider | Naam van de SDK-Provider | OAuth-Host |
   |:--- |:--- |:--- |
   | Azure Active Directory | AAD | https://login.microsoftonline.com |
   | Facebook | facebook | https://www.facebook.com |
   | Google | Google | https://accounts.google.com |
   | Microsoft | microsoftaccount | https://login.live.com |
   | Twitter | twitter | https://api.twitter.com |

    Een voorbeeld van de inhoud-Security-beleid (geïmplementeerd voor Azure Active Directory) is als volgt:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    Vervang `https://login.microsoftonline.com` met de OAuth-host uit de voorgaande tabel.  Zie voor meer informatie over de inhoud beveiligingsbeleid meta-code, de [Documentatie over inhoud-Security-beleid].

    Sommige verificatieproviders vereisen geen dat inhoud-Security-bij beleidswijzigingen bij op de juiste mobiele apparaten.  Bijvoorbeeld, zijn geen wijzigingen inhoud beveiligingsbeleid vereist bij het gebruik van verificatie via Google op een Android-apparaat.

3. Open de `www/js/index.js` voor het bewerken van het bestand, zoek de `onDeviceReady()` methode, en bij het maken van de client code toe te voegen met de volgende code:

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

    Deze code vervangt de bestaande code die de tabelverwijzing maakt en de gebruikersinterface wordt vernieuwd.

    De methode login() begint verificatie met de provider. De methode login() is een asynchrone-functie waarmee een JavaScript-Promise wordt geretourneerd.  De rest van de initialisatie van de wordt in het antwoord van de belofte geplaatst zodat deze wordt niet uitgevoerd totdat de login()-methode is voltooid.

4. Vervang in de code die u zojuist hebt toegevoegd, `SDK_Provider_Name` met de naam van uw provider voor de aanmelding. Bijvoorbeeld: voor Azure Active Directory, gebruiken `client.login('aad')`.
5. Uw project uitvoeren.  Wanneer het project is geïnitialiseerd, ziet uw toepassing de OAuth-aanmeldingspagina voor de gekozen authentication-provider.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie [over verificatie] met Azure App Service.
* Doorgaan met de zelfstudie door toe te voegen [Pushmeldingen] aan uw Apache Cordova-app.

Informatie over het gebruik van de SDK's.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- URLs. -->
[Aan de slag met Mobile Apps]: app-service-mobile-cordova-get-started.md
[Documentatie over inhoud-Security-beleid]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Pushmeldingen]: app-service-mobile-cordova-get-started-push.md
[Over verificatie]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
