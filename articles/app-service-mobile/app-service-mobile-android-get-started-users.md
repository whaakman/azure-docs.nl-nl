---
title: Verificatie toevoegen voor Android met Mobile Apps | Microsoft Docs
description: Informatie over het gebruik van de functie Mobile Apps van Azure App Service voor verificatie van gebruikers van uw Android-app via een groot aantal identiteitsproviders, waaronder Google, Facebook, Twitter en Microsoft.
services: app-service\mobile
documentationcenter: android
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 1fc8e7c1-6c3c-40f4-9967-9cf5e21fc4e1
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 11/16/2017
ms.author: crdun
ms.openlocfilehash: 4ee71e00807fcfe698a7e965979434f338f5b870
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38595525"
---
# <a name="add-authentication-to-your-android-app"></a>Verificatie toevoegen aan uw Android-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Samenvatting
In deze zelfstudie hebt toevoegen u verificatie aan de todolist Quick Start-project in Android met behulp van een ondersteunde id-provider. In deze zelfstudie is gebaseerd op de [aan de slag met Mobile Apps] van de zelfstudie moet u eerst uitvoeren.

## <a name="register"></a>Uw app registreren voor verificatie en Azure App Service configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uw app toevoegen aan de toegestane externe Omleidings-URL 's

Veilige verificatie is vereist dat u een nieuwe URL-schema voor uw app definiëren. Hiermee kunt het verificatiesysteem terug te keren naar uw app nadat het verificatieproces voltooid is. In deze zelfstudie gebruiken we het URL-schema _appname_ in. U kunt echter een URL-schema dat u kiest. Deze moet uniek zijn voor uw mobiele App. De omleiding op de server inschakelen:

1. In de [Azure Portal], selecteert u uw App Service.

2. Klik op de **verificatie / autorisatie** menu-optie.

3. In de **toegestane externe Omleidings-URL's**, voer `appname://easyauth.callback`.  De _appname_ in deze reeks wordt het URL-schema voor uw mobiele toepassing.  Het moet normale URL-specificatie voor een protocol (Gebruik letters en cijfers alleen en beginnen met een letter) volgen.  U moet een notitie van de tekenreeks die u kiest, aangezien u nodig hebt om aan te passen van de code van uw mobiele toepassing met de URL-schema op verschillende plaatsen.

4. Klik op **OK**.

5. Klik op **Opslaan**.

## <a name="permissions"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* In Android Studio opent u het project dat u voltooid met de zelfstudie [aan de slag met Mobile Apps]. Uit de **uitvoeren** menu, klikt u op **app uitvoeren**, en controleer of dat een niet-verwerkte uitzondering met een statuscode 401 (niet-gemachtigd) is gegenereerd nadat de app wordt gestart.

     Deze uitzondering komt doordat de app probeert te krijgen tot de back-end als niet-geverifieerde gebruiker, maar de *TodoItem* tabel nu verificatie is vereist.

Vervolgens maakt bijwerken u de app om gebruikers te verifiëren voordat u aanvraagt van resources van de back-end voor mobiele Apps.

## <a name="add-authentication-to-the-app"></a>Verificatie toevoegen aan de app.
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]



## <a name="cache-tokens"></a>Verificatietokens cache op de client
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Volgende stappen
Nu dat u basisverificatie-zelfstudie hebt voltooid, kunt u doorgaan naar een van de volgende zelfstudies:

* [Pushmeldingen toevoegen aan uw Android-app](app-service-mobile-android-get-started-push.md).
  Informatie over het configureren van uw back-end voor mobiele Apps voor het gebruik van Azure notification hubs om pushmeldingen te verzenden.
* [Offlinesynchronisatie inschakelen voor uw Android-app](app-service-mobile-android-get-started-offline-data.md).
  Informatie over het toevoegen van Offlineondersteuning aan uw app met behulp van een back-end voor mobiele Apps. Offline synchroniseren zorgt gebruikers met een mobiele app kunnen werken&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;, zelfs wanneer er geen netwerkverbinding.

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Aan de slag met Mobile Apps]: app-service-mobile-android-get-started.md
[Azure Portal]: https://portal.azure.com/
