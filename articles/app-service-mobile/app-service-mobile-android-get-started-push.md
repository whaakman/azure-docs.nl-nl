---
title: Pushmeldingen toevoegen aan uw Android-app met Mobile Apps | Microsoft Docs
description: Informatie over het gebruik van mobiele Apps voor het verzenden van pushmeldingen aan uw Android-app.
services: app-service\mobile
documentationcenter: android
manager: crdun
editor: ''
author: conceptdev
ms.assetid: 9058ed6d-e871-4179-86af-0092d0ca09d3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 11/17/2017
ms.author: crdun
ms.openlocfilehash: 9e9f7aba49c53a1a6fcc611ed771f266eb49c883
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38547172"
---
# <a name="add-push-notifications-to-your-android-app"></a>Pushmeldingen toevoegen aan uw Android-app
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie voegt u pushmeldingen kunt verzenden naar de [Android snel starten] project, zodat een pushmelding wordt verzonden naar het apparaat telkens wanneer een record wordt ingevoegd.

Als u het gedownloade quick start-serverproject niet gebruikt, moet u de push notification-uitbreidingspakket. Zie voor meer informatie, [werken met de .NET back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Vereisten
U hebt het volgende nodig:

* Een IDE, afhankelijk van de back-end van uw project:

  * [Android Studio](https://developer.android.com/sdk/index.html) als deze app een Node.js-back-end heeft.
  * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) of hoger als u deze app heeft een Microsoft .NET-back-end.
* Android 2.3 of hoger, Google Repository revisie 27 of hoger en Google Play-Services 9.0.2 of hoger voor Firebase Cloud Messaging.
* Voltooi de [Android snel starten].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Een project maken dat Firebase Cloud Messaging ondersteunt
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Een Notification Hub configureren
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure voor het verzenden van pushmeldingen configureren
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Schakel pushmeldingen in voor het serverproject
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Pushmeldingen toevoegen aan uw app
In deze sectie maakt bijwerken u uw client-app voor Android voor het afhandelen van pushmeldingen.

### <a name="verify-android-sdk-version"></a>Controleer of de Android SDK-versie
[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

De volgende stap is het installeren van Google Play-services. Firebase Cloud Messaging heeft bepaalde minimale API-niveau vereisten voor ontwikkelen en testen, die de **minSdkVersion** eigenschap in het manifest moet voldoen aan.

Als u met een ouder apparaat testen wilt, raadpleegt u [Firebase toevoegen aan uw Android-Project] om te bepalen hoe laag u kunt deze waarde hebt ingesteld, en op de juiste wijze ingesteld.

### <a name="add-firebase-cloud-messaging-to-the-project"></a>Firebase Cloud Messaging toevoegen aan het project
[!INCLUDE [Add Firebase Cloud Messaging](../../includes/app-service-mobile-add-firebase-cloud-messaging.md)]

### <a name="add-code"></a>Voeg code toe:
[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>De app testen op de gepubliceerde mobiele service
U kunt de app testen door het koppelen van een Android-telefoon met een USB-kabel rechtstreeks of via een virtueel apparaat in de emulator.

## <a name="next-steps"></a>Volgende stappen
Nu dat u deze zelfstudie hebt voltooid, kunt u doorgaan naar een van de volgende zelfstudies:

* [Verificatie toevoegen aan uw Android-app](app-service-mobile-android-get-started-users.md).
  Leer hoe u verificatie toevoegen aan de todolist Quick Start-project in Android met behulp van een ondersteunde id-provider.
* [Offlinesynchronisatie inschakelen voor uw Android-app](app-service-mobile-android-get-started-offline-data.md).
  Informatie over het toevoegen van Offlineondersteuning aan uw app met behulp van een back-end voor mobiele Apps. Offline synchroniseren zorgt gebruikers met een mobiele app kunnen werken&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;, zelfs wanneer er geen netwerkverbinding.

<!-- URLs -->
[Android snel starten]: app-service-mobile-android-get-started.md
[Firebase toevoegen aan uw Android-Project]:https://firebase.google.com/docs/android/setup
