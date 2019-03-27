---
title: Stel pushmeldingen in Azure Notification Hubs | Microsoft Docs
description: Meer informatie over het instellen van Azure Notification Hubs in Azure portal met behulp van platform notification system (PNS)-instellingen.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: jowargo
ms.openlocfilehash: 04108c4b5a8ce04b971a0529393ea442cb1cf226
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58480120"
---
# <a name="set-up-push-notifications-in-a-notification-hub-in-the-azure-portal"></a>Stel pushmeldingen in een notification hub in Azure portal

Azure Notification Hubs biedt een push-engine die is eenvoudig te gebruiken en die wordt uitgeschaald. Notification Hubs gebruiken om meldingen te verzenden naar elk platform (iOS, Android, Windows, Kindle, Baidu) en uit een back-end (cloud of on-premises). Zie voor meer informatie, [wat is Azure Notification Hubs?](notification-hubs-push-notification-overview.md).

In deze snelstartgids gebruikt u de instellingen van platform notification system (PNS) in Notification Hubs voor het instellen van pushmeldingen op meerdere platforms. De Quick Start ziet u de stappen in de Azure-portal.

Als u al een notification hub hebt gemaakt, moet u een nu maken. Zie voor meer informatie, [een Azure notification hub maken in Azure portal](create-notification-hub-portal.md). 

## <a name="apple-push-notification-service"></a>Apple Push Notification Service

Instellen van Apple Push Notification Service (APNS):

1. In de Azure-portal op de **Notification Hub** weergeeft, schakelt **Apple (APNS)** in het menu links.

1. Voor **verificatiemodus**, selecteert u **certificaat** of **Token**.

   a. Als u selecteert **certificaat**:
   * Selecteer het pictogram van het bestand en selecteer vervolgens de *.p12* bestand dat u wilt uploaden.
   * Voer een wachtwoord in.
   * Selecteer de modus **Sandbox**. Voor het verzenden van pushmeldingen naar gebruikers die uw app uit de store hebt aangeschaft, selecteert u **productie** modus.

     ![Schermafbeelding van een APNS-certificaat-configuratie in Azure portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

   b. Als u selecteert **Token**:

   * Voer de waarden in voor **sleutel-ID**, **bundel-ID**, **Team-ID**, en **Token**.
   * Selecteer de modus **Sandbox**. Voor het verzenden van pushmeldingen naar gebruikers die uw app uit de store hebt aangeschaft, selecteert u **productie** modus.

     ![Schermafbeelding van een APNS-token configuratie in Azure portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-token.png)

Voor meer informatie, Zie [Pushmeldingen naar iOS met behulp van Azure Notification Hubs](notification-hubs-ios-apple-push-notification-apns-get-started.md).

## <a name="google-firebase-cloud-messaging"></a>Google Firebase Cloud Messaging

Pushmeldingen voor Google Firebase Cloud Messaging (FCM) instellen:

1. In de Azure-portal op de **Notification Hub** weergeeft, schakelt **Google (GCM/FCM)** in het menu links. 
2. Plak de **API-sleutel** van het FCM-project dat u eerder hebt opgeslagen. 
3. Selecteer **Opslaan**. 

   ![Schermopname die laat zien hoe u Notification Hubs voor Google FCM configureren](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)

Wanneer u deze stappen hebt voltooid, wordt een waarschuwing geeft aan dat de notification hub is bijgewerkt. De knop **Opslaan** kan niet worden gekozen. 

Zie voor meer informatie, [Pushmeldingen naar Android-apparaten met behulp van Notification Hubs en FCM Google](notification-hubs-android-push-notification-google-fcm-get-started.md).

## <a name="windows-push-notification-service"></a>Windows Push Notification Service

Instellen van Windows Push Notification Service (WNS):

1. In de Azure-portal op de **Notification Hub** weergeeft, schakelt **Windows (WNS)** in het menu links.
2. Voer waarden in voor **Pakket-SID** en **Beveiligingssleutel**.
3. Selecteer **Opslaan**.

   ![Schermafbeelding van de vakken pakket-SID en beveiligingssleutel](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Zie voor meer informatie, [meldingen verzenden naar UWP-apps met behulp van Azure Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).

## <a name="microsoft-push-notification-service-for-windows-phone"></a>Microsoft Push Notification Service voor Windows Phone

Instellen van Microsoft Push Notification Service (MPNS) voor Windows Phone: 

1. In de Azure-portal op de **Notification Hub** weergeeft, schakelt **Windows Phone (MPNS)** in het menu links.
1. Een niet-geverifieerde of geverifieerde pushmeldingen inschakelen:

   a. Als niet-geverifieerde pushmeldingen te verzenden, schakelt **niet-geverifieerde pushmeldingen inschakelen** > **opslaan**.

      ![Schermopname die laat zien hoe niet-geverifieerde pushmeldingen inschakelen](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

   b. Geverifieerde pushmeldingen inschakelen:
      * Selecteer op de werkbalk **certificaat uploaden**.
      * Selecteer het pictogram van het bestand en selecteer vervolgens het certificaatbestand.
      * Voer het wachtwoord voor het certificaat in.
      * Selecteer **OK**.
      * Op de **Windows Phone (MPNS)** weergeeft, schakelt **opslaan**.

Zie voor meer informatie, [Pushmeldingen naar Windows Phone-apps met behulp van Notification Hubs](notification-hubs-windows-mobile-push-notifications-mpns.md).
      
## <a name="amazon-device-messaging"></a>Amazon Device Messaging

Pushmeldingen voor Amazon Device Messaging (ADM) instellen:

1. In de Azure-portal op de **Notification Hub** weergeeft, schakelt **Amazon (ADM)** in het menu links.
2. Voer waarden in voor **Client-ID** en **Clientgeheim**.
3. Selecteer **Opslaan**.
    
   ![Schermafbeelding van de ADM-instellingen in Azure portal](./media/notification-hubs-kindle-get-started/notification-hub-adm-settings.png)

Zie voor meer informatie, [aan de slag met Notification Hubs voor Kindle-apps](notification-hubs-kindle-amazon-adm-push-notification.md).

## <a name="baidu-android-china"></a>Baidu (Android China)

Pushmeldingen voor Baidu instellen:

1. In de Azure-portal op de **Notification Hub** weergeeft, schakelt **Baidu (Android China)** in het menu links. 
2. Voer de **Api-sleutel** die u hebt verkregen via de Baidu-console in de Baidu cloud push-project. 
3. Voer de **geheime sleutel** die u hebt verkregen via de Baidu-console in de Baidu cloud push-project. 
4. Selecteer **Opslaan**. 

    ![Schermopname van Notification Hubs waarin de configuratie van de Baidu (Android China) voor pushmeldingen](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)

Wanneer u deze stappen hebt voltooid, wordt een waarschuwing geeft aan dat de notification hub is bijgewerkt. De knop **Opslaan** kan niet worden gekozen. 

Zie voor meer informatie, [aan de slag met Notification Hubs met behulp van Baidu](notification-hubs-baidu-china-android-notifications-get-started.md).

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u geleerd hoe u platform notification system om instellingen te configureren voor een notification hub in Azure portal. 

Zie voor meer informatie over hoe u pushmeldingen verzenden naar verschillende platforms, deze zelfstudies:

- [Pushmeldingen verzenden naar iOS-apparaten met behulp van Notification Hubs en APNS](notification-hubs-ios-apple-push-notification-apns-get-started.md)
- [Pushmeldingen verzenden naar Android-apparaten met behulp van Notification Hubs en FCM Google](notification-hubs-android-push-notification-google-fcm-get-started.md)
- [Pushmeldingen verzenden naar een UWP-app die wordt uitgevoerd op een Windows-apparaat](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
- [Pushmeldingen verzenden naar een Windows Phone 8-app met behulp van MPNS](notification-hubs-windows-mobile-push-notifications-mpns.md)
- [Pushmeldingen verzenden naar een Kindle-toepassing](notification-hubs-kindle-amazon-adm-push-notification.md)
- [Pushmeldingen met Notification Hubs en Baidu cloud push](notification-hubs-baidu-china-android-notifications-get-started.md)
