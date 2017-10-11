---
title: Verificatie toevoegen voor Android met Mobile Apps | Microsoft Docs
description: "Informatie over het verifiëren van gebruikers van uw Android-app via een groot aantal identiteitsproviders, waaronder Google, Facebook, Twitter en Microsoft met de functie Mobile Apps van Azure App Service."
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 1fc8e7c1-6c3c-40f4-9967-9cf5e21fc4e1
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 81331142aa6110d4e29e6fb30a90ce6e3a853439
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-android-app"></a>Verificatie toevoegen aan uw Android-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Samenvatting
In deze zelfstudie maakt toevoegen u verificatie aan de todolist-Quick Start-project voor Android met behulp van een ondersteunde id-provider. Deze zelfstudie is gebaseerd op de [aan de slag met Mobile Apps] zelfstudie die u moet eerst te voltooien.

## <a name="register"></a>Uw app registreren voor verificatie en Azure App Service configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uw app toevoegen aan de toegestane externe Omleidings-URL 's

Veilige verificatie vereist dat u een nieuwe URL-schema voor uw app definiëren. Hierdoor kan de verificatiesysteem terug te keren naar uw app zodra het verificatieproces voltooid is. In deze zelfstudie gebruiken we het URL-schema _appname_ in. U kunt echter een URL-schema dat u kiest. Deze moet uniek zijn voor uw mobiele App. De omleiding op de server inschakelen:

1. Selecteer in de [Azure-portal] uw App Service.

2. Klik op de **verificatie / autorisatie** menuoptie.

3. In de **toegestaan externe Omleidings-URL's**, voer `appname://easyauth.callback`.  De _appname_ in deze tekenreeks wordt het URL-schema voor uw mobiele toepassing.  Deze moet voldoen aan de normale URL-specificatie voor een protocol (Gebruik letters en cijfers alleen en begin met een letter).  U moet een notitie van de tekenreeks die u naar wens aanpassen van uw mobiele toepassingscode met het URL-schema op verschillende plaatsen.

4. Klik op **OK**.

5. Klik op **Opslaan**.

## <a name="permissions"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* In Android Studio, opent u het project u voltooid met de zelfstudie [aan de slag met Mobile Apps]. Van de **uitvoeren** menu, klikt u op **app uitvoeren**, en controleer of dat een niet-verwerkte uitzondering met een statuscode van 401 (niet-geautoriseerd) treedt op nadat de app wordt gestart.

     Deze uitzondering treedt op omdat de app probeert te krijgen tot de back-end als niet-geverifieerde gebruiker, maar de *TodoItem* tabel nu is verificatie vereist.

Werk vervolgens de app om gebruikers te verifiëren voordat u resources van de back-end van Mobile Apps. 

## <a name="add-authentication-to-the-app"></a>Verificatie toevoegen aan de app.
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]



## <a name="cache-tokens"></a>Verificatietokens cache op de client
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Volgende stappen
Nu dat u deze basisverificatie-zelfstudie hebt voltooid, overweeg dan u verder gaat u aan bij een van de volgende zelfstudies:

* [Pushmeldingen toevoegen aan uw Android-app](app-service-mobile-android-get-started-push.md).
  Informatie over het configureren van uw back-end van Mobile Apps voor het gebruik van Azure notification hubs pushmeldingen verzendt.
* [Offlinesynchronisatie voor uw Android-app inschakelen](app-service-mobile-android-get-started-offline-data.md).
  Informatie over het toevoegen van Offlineondersteuning aan uw app met behulp van een back-end van Mobile Apps. Met het offline synchroniseren gebruikers kunnen communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs wanneer er geen netwerkverbinding.

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[aan de slag met Mobile Apps]: app-service-mobile-android-get-started.md
