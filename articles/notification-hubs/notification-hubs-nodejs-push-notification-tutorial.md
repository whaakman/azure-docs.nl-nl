---
title: Pushmeldingen verzenden met Azure Notification Hubs en Node.js
description: Informatie over het gebruik van Notification Hubs pushmeldingen verzendt vanuit een Node.js-toepassing.
keywords: push-bericht, push notifications,node.js push, ios push
services: notification-hubs
documentationcenter: nodejs
author: ysxu
manager: erikre
editor: 
ms.assetid: ded4749c-6c39-4ff8-b2cf-1927b3e92f93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/25/2016
ms.author: yuaxu
ms.openlocfilehash: ff2dd0c2ededa3664c48b5ff77b05466fceb4b3f
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Pushmeldingen verzenden met Azure Notification Hubs en Node.js
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a>Overzicht
> [!IMPORTANT]
> U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs) voor meer informatie.
> 
> 

Deze handleiding wordt beschreven hoe u om pushmeldingen te verzenden met behulp van Azure Notification Hubs rechtstreeks vanuit een Node.js-toepassing. 

De scenario's worden behandeld zijn pushmeldingen verzenden naar toepassingen op de volgende platforms:

* Android
* iOS
* Windows Phone
* Universele Windows-Platform 

Zie voor meer informatie over notification hubs de [Vervolgstappen](#next) sectie.

## <a name="what-are-notification-hubs"></a>Wat is Notification Hubs?
Azure Notification Hubs bieden een eenvoudig te gebruiken, meerdere platforms, schaalbare infrastructuur voor het verzenden van pushmeldingen naar mobiele apparaten. Zie voor meer informatie over de service-infrastructuur, de [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) pagina.

## <a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken
De eerste stap in deze zelfstudie is een nieuwe lege Node.js-toepassing maken. Zie voor instructies over het maken van een Node.js-toepassing [maken en implementeren van een Node.js-toepassing naar de Azure-website][nodejswebsite], [Node.js-Cloudservice] [ Node.js Cloud Service] met Windows PowerShell of [website met WebMatrix].

## <a name="configure-your-application-to-use-notification-hubs"></a>Uw toepassing configureren voor het gebruik van Notification Hubs
Voor het gebruik van Azure Notification Hubs die u wilt downloaden en gebruiken van de Node.js [azure-pakket](https://www.npmjs.com/package/azure), waaronder een ingebouwde verzameling helper-bibliotheken die met de push notification REST-services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Knooppunt Package Manager (NPM) gebruiken om het pakket te verkrijgen
1. Een opdrachtregelinterface gebruiken zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Linux) en navigeer naar de map waar u uw lege toepassing gemaakt.
2. Type **npm installeren azure sb** in het opdrachtvenster.
3. U kunt handmatig uitvoeren de **ls** of **dir** opdracht om te controleren of een **knooppunt\_modules** map is gemaakt. In deze map te zoeken naar de **azure** , dit pakket de bibliotheken die u nodig hebt bevat voor toegang tot de Notification Hub.

> [!NOTE]
> U kunt meer informatie over het installeren van NPM op de officiële [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 
> 
> 

### <a name="import-the-module"></a>Importeer de module
Start een teksteditor en voeg het volgende toe boven aan de **server.js** -bestand van de toepassing:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Een Azure Notification Hub-verbinding instellen
De **NotificationHubService** object kunt u samenwerken met notification hubs. De volgende code maakt een **NotificationHubService** -object voor de nofication hub met de naam **hubname**. Voeg deze toe aan de bovenkant van de **server.js** bestand na de instructie voor het importeren van de azure-module:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

De verbinding **connectionstring** waarde kan worden verkregen van de [Azure Portal] door de volgende stappen uit te voeren:

1. Klik in het navigatiedeelvenster links op **Bladeren**.
2. Selecteer **Notification Hubs**, en gaat u naar de hub die u wilt gebruiken voor het voorbeeld. U kunt verwijzen naar de [Windows Store aan de slag zelfstudie](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) als u informatie over het maken van een nieuwe Notification Hub nodig.
3. Selecteer **instellingen**.
4. Klik op **toegangsbeleid**. Hier ziet u beide verbindingsreeksen gedeelde en volledige toegang.

![Azure Portal - Notification Hubs](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> U kunt ook de verbindingsreeks ophalen met de **Get-AzureSbNamespace** cmdlet geleverd door [Azure PowerShell](/powershell/azureps-cmdlets-docs) of de **azure sb naamruimte weergeven** opdracht met de [Azure-opdrachtregelinterface (Azure CLI)](../cli-install-nodejs.md).
> 
> 

## <a name="general-architecture"></a>Algemene architectuur
De **NotificationHubService** object bevat de volgende instanties van objecten voor het verzenden van pushmeldingen aan specifieke apparaten en toepassingen:

* **Android** -gebruiken de **GcmService** object, dat beschikbaar is op **notificationHubService.gcm**
* **iOS** -gebruiken de **ApnsService** -object, dat openen op **notificationHubService.apns**
* **Windows Phone** -gebruiken de **MpnsService** object, dat beschikbaar is op **notificationHubService.mpns**
* **Universele Windows-Platform** -gebruiken de **WnsService** object, dat beschikbaar is op **notificationHubService.wns**

### <a name="how-to-send-push-notifications-to-android-applications"></a>Hoe: pushmeldingen verzenden naar Android-toepassingen
De **GcmService** -object biedt een **verzenden** methode die kan worden gebruikt voor het verzenden van pushmeldingen naar Android-toepassingen. De **verzenden** methode accepteert de volgende parameters:

* **Labels** -de label-id. Als er is geen code is opgegeven, wordt de melding verzonden naar alle clients.
* **Nettolading** -JSON of raw string nettolading van het bericht.
* **Callback** -callback-functie.

Zie voor meer informatie over de indeling van nettolading de **nettolading** sectie van de [GCM Server implementeren](http://developer.android.com/google/gcm/server.html#payload) document.

De volgende code gebruikt de **GcmService** exemplaar beschikbaar is gemaakt door de **NotificationHubService** een push-melding verzenden naar alle geregistreerde clients.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Hoe: pushmeldingen verzenden naar iOS-toepassingen
Hetzelfde als met de hierboven beschreven, Android-toepassingen de **ApnsService** -object biedt een **verzenden** methode die kan worden gebruikt voor het verzenden van pushmeldingen voor iOS-toepassingen. De **verzenden** methode accepteert de volgende parameters:

* **Labels** -de label-id. Als er is geen code is opgegeven, wordt de melding verzonden naar alle clients.
* **Nettolading** -JSON of string nettolading van het bericht.
* **Callback** -callback-functie.

Zie voor meer informatie de indeling van nettolading de **nettolading van de meldingen** sectie van de [Local and Push Notification Programming Guide](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) document.

De volgende code gebruikt de **ApnsService** exemplaar beschikbaar is gemaakt door de **NotificationHubService** een waarschuwingsbericht verzenden naar alle clients:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
         // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Hoe: pushmeldingen verzenden naar Windows Phone-toepassingen
De **MpnsService** -object biedt een **verzenden** methode die kan worden gebruikt om pushmeldingen te verzenden naar Windows Phone-toepassingen. De **verzenden** methode accepteert de volgende parameters:

* **Labels** -de label-id. Als er is geen code is opgegeven, wordt de melding verzonden naar alle clients.
* **Nettolading** -XML-nettolading van het bericht.
* **Doelnaam**  -  `toast` voor pop-upmeldingen. `token`voor meldingen van de tegel.
* **NotificationClass** -de prioriteit van de melding. Zie de **elementen voor HTTP-Header** sectie van de [Pushmeldingen vanaf een server](http://msdn.microsoft.com/library/hh221551.aspx) document voor geldige waarden.
* **Opties** : optioneel aanvraagheaders.
* **Callback** -callback-functie.

Voor een lijst met geldige **TargetName**, **NotificationClass** en header-opties, bekijk de [Pushmeldingen vanaf een server](http://msdn.microsoft.com/library/hh221551.aspx) pagina.

Voorbeeld van de volgende code wordt de **MpnsService** exemplaar beschikbaar is gemaakt door de **NotificationHubService** voor het verzenden van een pop-upmelding:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Hoe: pushmeldingen verzenden naar Universal Windows Platform (UWP)-toepassingen
De **WnsService** -object biedt een **verzenden** methode die kan worden gebruikt voor het verzenden van pushmeldingen voor universele Windows-Platform-toepassingen.  De **verzenden** methode accepteert de volgende parameters:

* **Labels** -de label-id. Als er is geen code is opgegeven, wordt de melding verzonden naar alle geregistreerde clients.
* **Nettolading** -nettolading van de XML-bericht.
* **Type** -het meldingstype.
* **Opties** : optioneel aanvraagheaders.
* **Callback** -callback-functie.

Zie voor een lijst van geldige typen en aanvraagheaders [Push notification service aanvraag- en reactieheaders](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

De volgende code gebruikt de **WnsService** exemplaar beschikbaar is gemaakt door de **NotificationHubService** een pop-upmelding verzendt naar een UWP-app:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
         // notification sent
      }
    });

## <a name="next-steps"></a>Volgende stappen
De codefragmenten voorbeeld hierboven kunnen u gemakkelijk maken voor service-infrastructuur voor het leveren van pushmeldingen aan een groot aantal apparaten. Nu u de basisbeginselen van het gebruik van Notification Hubs met behulp van node.js hebt geleerd, volgt u deze koppelingen voor meer informatie over hoe deze mogelijkheden verder kan worden uitgebreid.

* Zie de MSDN-verwijzing voor [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).
* Ga naar de [Azure SDK voor knooppunt] opslagplaats op GitHub voor meer voorbeelden en implementatiegegevens.

[Azure SDK voor knooppunt]: https://github.com/WindowsAzure/azure-sdk-for-node
[Next Steps]: #nextsteps
[What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
[Create a Service Namespace]: #create-a-service-namespace
[Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
[Create a Node.js Application]: #Create_a_Nodejs_Application
[Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
[How to: Create a Topic]: #How_to_Create_a_Topic
[How to: Create Subscriptions]: #How_to_Create_Subscriptions
[How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
[How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
[How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
[How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
[1]: #Next_Steps
[Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
[image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
[2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
[3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
[4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
[5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
[SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
[SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
[website met WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
[Azure Portal]: https://portal.azure.com
