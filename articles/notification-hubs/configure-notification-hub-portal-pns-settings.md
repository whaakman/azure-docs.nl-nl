---
title: Azure notification hub configureren met PNS-instellingen | Microsoft Docs
description: In deze quickstart leert u hoe u een notification hub kunt configureren in de Azure-portal met PNS-instellingen (Platform Notification System).
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: jowargo
ms.openlocfilehash: 7f7e4a4d75a8e118da6f026817bc4ecfcc7a60db
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56313925"
---
# <a name="configure-an-azure-notification-hub-with-platform-notification-system-settings-in-the-azure-portal"></a>Een Azure notification hub configureren met PNS-instellingen in de Azure-portal 
Azure Notification Hubs biedt een eenvoudig te gebruiken en uitgeschaalde push-engine waarmee u vanuit elke back-end (cloud of on-premises) meldingen kunt verzenden naar ieder platform (iOS, Android, Windows, Kindle, Baidu, enzovoort). Zie voor meer informatie over de service [Wat is Azure Notification Hubs?](notification-hubs-push-notification-overview.md).

[Maak een Azure notification hub met behulp van de Azure-portal](create-notification-hub-portal.md) als u nog niet hebt gedaan. In deze quickstart leert u hoe u een notification hub kunt configureren in de Azure-portal met PNS-instellingen (Platform Notification System).

## <a name="apple-push-notification-service-apns"></a>Apple Push Notification Service (APNS)
1. Op de pagina **Notification Hub** in de Azure-portal selecteert u **Apple (APNS)** onder **Instellingen** in het menu links.
2. Selecteer **Certificaat** en voer de volgende stappen uit:
    1. Selecteer het **pictogram van het bestand** en selecteer het **.p12**-bestand dat u wilt uploaden. 
    2. Geef het **wachtwoord** op.
    3. Selecteer de modus **Sandbox**. Gebruik **Productie** alleen als u pushmeldingen wilt verzenden naar gebruikers die uw app in de winkel hebben aangeschaft.

        ![APNS-certificering in Azure Portal configureren](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)
3. Selecteer **Token** en voer deze stappen uit: 
    1. Voer een **sleutel-id**, **bundel-id**, **team-id** en **token** in.
    2. Selecteer de modus **Sandbox**. Gebruik **Productie** alleen als u pushmeldingen wilt verzenden naar gebruikers die uw app in de winkel hebben aangeschaft.

        ![APNS-token in de Azure-portal configureren](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-token.png)

Zie [deze zelfstudie](notification-hubs-ios-apple-push-notification-apns-get-started.md) voor een volledige zelfstudie over het verzenden van pushmeldingen naar iOS-apparaten met behulp van Azure Notification Hubs en Apple Push Notification Service (APNS).

## <a name="google-firebase-cloud-messaging-fcm"></a>Google Firebase Cloud Messaging (FCM)
1. Op de pagina **Notification Hub** in de Azure-portal selecteert u **Google (GCM/FCM)** onder **Instellingen** in het menu links. 
2. Plak de **serversleutel** voor het FCM-project dat u eerder hebt opgeslagen. 
3. Selecteer **Opslaan** op de werkbalk. 

    ![Azure Notification Hubs - Google (FCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)
4. U ziet een bericht in waarschuwingen dat de notification hub is bijgewerkt. De knop **Opslaan** kan niet worden gekozen. 

Zie [deze zelfstudie](notification-hubs-android-push-notification-google-fcm-get-started.md) voor een volledige zelfstudie over het verzenden van pushmeldingen naar Android-apparaten met behulp van Azure Notification Hubs en Google Firebase Cloud Messaging.

## <a name="windows-push-notification-service-wns"></a>Windows Push Notification Services (WNS)
1. Op de pagina **Notification Hub** in de Azure-portal selecteert u **Windows (WNS)** onder **Instellingen** in het menu links.
2. Voer waarden in voor **Pakket-SID** en **Beveiligingssleutel**.
3. Selecteer **Opslaan** op de werkbalk.

    ![De vakken Pakket-SID en Beveiligingssleutel](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)


Zie [deze zelfstudie](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) voor een volledige zelfstudie over het verzenden van pushmeldingen naar een UWP-app (universeel Windows-platform) die wordt uitgevoerd op een Windows-apparaat.

## <a name="windows-phone---microsoft-push-notification-service"></a>Windows Phone - Microsoft Push Notification Service
1. Op de pagina **Notification Hub** in de Azure-portal selecteert u **Windows Phone (MPNS)** onder **Instellingen** in het menu links.
2. Als u niet-geverifieerde pushmeldingen wilt inschakelen, selecteert u **Niet-geverifieerde pushmeldingen inschakelen** en vervolgens **Opslaan** op de werkbalk.

    ![Azure Portal - Niet-geverifieerde pushmeldingen inschakelen](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)
3. Als u alleen **geverifieerde** pushmeldingen wilt toestaan, gaat u als volgt te werk:
    1. Selecteer **Certificaat uploaden** op de werkbalk.
    2. Selecteer het **pictogram van het bestand** en selecteer het certificaatbestand.
    3. Voer het **wachtwoord** voor het certificaat in. 
    4. Selecteer **OK** om de pagina **Certificaat uploaden** te sluiten. 
    5. Op de pagina **Windows Phone (MPNS)** selecteert u **Opslaan** op de werkbalk.

Zie [deze zelfstudie](notification-hubs-windows-mobile-push-notifications-mpns.md) voor een volledige zelfstudie over het verzenden van pushmeldingen naar een Windows Phone 8-app met behulp van Microsoft Push Notification Service (MPNS).
      
## <a name="amazon-device-messaging-adm"></a>Amazon Device Messaging (ADM)
1. Op de pagina **Notification Hub** in de Azure-portal selecteert u **Amazon (ADM)** onder **Instellingen** in het menu links.
2. Voer waarden in voor **Client-id** en **Clientgeheim**.
3. Selecteer **Opslaan** op de werkbalk.
    
    ![Azure Notification Hubs - ADM-instellingen](./media/notification-hubs-kindle-get-started/notification-hub-adm-settings.png)

Zie [deze zelfstudie](notification-hubs-kindle-amazon-adm-push-notification.md) voor een volledige zelfstudie over het verzenden van pushmeldingen naar een Kindle-toepassing met behulp van Azure Notification Hubs.

## <a name="baidu-android-china"></a>Baidu (Android China)
1. Op de pagina **Notification Hub** in de Azure-portal selecteert u **Baidu (Android China)** onder **Instellingen** in het menu links. 
2. Voer de **API-sleutel** in die u hebt gekregen via de Baidu-console, in het Baidu Cloud Push-project. 
3. Voer de **geheime sleutel** in die u hebt gekregen via de Baidu-console, in het Baidu Cloud Push-project. 
4. Selecteer **Opslaan** op de werkbalk. 

    ![Azure Notification Hubs - Baidu (Android China)](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)
4. U ziet een bericht in waarschuwingen dat de notification hub is bijgewerkt. De knop **Opslaan** kan niet worden gekozen. 

Zie [deze zelfstudie](notification-hubs-baidu-china-android-notifications-get-started.md) voor een volledige zelfstudie over het verzenden van pushmeldingen met behulp van Azure Notification Hubs en Baidu Cloud Push.

## <a name="next-steps"></a>Volgende stappen
In deze quickstart hebt u geleerd hoe u verschillende platformmeldingensystemen kunt configureren voor een notification hub in de Azure-portal. 

Zie de zelfstudies in de sectie **Zelfstudies** voor volledige stapsgewijze instructies voor het verzenden van pushmeldingen naar deze verschillende platforms.

- [Pushmeldingen verzenden naar iOS-apparaten met behulp van Azure Notification Hubs en Apple Push Notification Service (APNS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).
- [Pushmeldingen verzenden naar Android-apparaten met behulp van Azure Notification Hubs en Google Firebase Cloud Messaging](notification-hubs-android-push-notification-google-fcm-get-started.md).
- [Pushmeldingen verzenden naar een UWP-app (universeel Windows-platform) die wordt uitgevoerd op een Windows-apparaat](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
- [Pushmeldingen verzenden naar een Windows Phone 8-app met behulp van Microsoft Push Notification Service (MPNS)](notification-hubs-windows-mobile-push-notifications-mpns.md).
- [Pushmeldingen verzenden naar een Kindle-toepassing](notification-hubs-kindle-amazon-adm-push-notification.md).
- [Pushmeldingen verzenden met behulp van Azure Notification Hubs en Baidu Cloud Push](notification-hubs-baidu-china-android-notifications-get-started.md).
