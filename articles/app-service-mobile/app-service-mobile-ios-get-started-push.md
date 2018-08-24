---
title: Pushmeldingen toevoegen aan iOS-App met Azure Mobile Apps
description: Informatie over het gebruik van Azure Mobile Apps voor het verzenden van pushmeldingen aan uw iOS-app.
services: app-service\mobile
documentationcenter: ios
manager: crdun
editor: ''
author: conceptdev
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/10/2016
ms.author: crdun
ms.openlocfilehash: b84990d053d92b9408e119b8aba28c8b9f8b046e
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818995"
---
# <a name="add-push-notifications-to-your-ios-app"></a>Pushmeldingen toevoegen aan uw iOS-App

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht

In deze zelfstudie voegt u pushmeldingen kunt verzenden naar de [iOS snelstartgids] project, zodat een pushmelding wordt verzonden naar het apparaat telkens wanneer een record wordt ingevoegd.

Als u het gedownloade Quick Start-serverproject niet gebruikt, moet u de push notification-uitbreidingspakket. Zie voor meer informatie, [werken met de .NET back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) handleiding.

De [pushmeldingen biedt geen ondersteuning voor iOS-simulator](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). U moet een fysiek iOS-apparaat en een [Apple Developer Program-lidmaatschap](https://developer.apple.com/programs/ios/).

## <a name="configure-hub"></a>Notification Hub configureren

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>App voor pushmeldingen registreren

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure voor het verzenden van pushmeldingen configureren

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a id="update-server"></a>Bijwerken van de back-end om pushmeldingen te verzenden

[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Pushmeldingen toevoegen aan de app

[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Testpushmeldingen

[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <a id="more"></a>Meer

* Sjablonen beschikt u over flexibiliteit voor het verzenden van platformonafhankelijke pushes en gelokaliseerde pushes. [Over het gebruik iOS-clientbibliotheek voor Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) ziet u informatie over het registreren van sjablonen.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS snelstartgids]: app-service-mobile-ios-get-started.md
