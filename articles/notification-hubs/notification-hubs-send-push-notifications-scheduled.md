---
title: Hoe u geplande meldingen verzendt | Microsoft Docs
description: In dit onderwerp wordt beschreven met behulp van geplande meldingen met Azure Notification Hubs.
services: notification-hubs
documentationcenter: .net
keywords: pushmeldingen, pushmelding, pushmeldingen plannen
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: af0de9e8c18644f4ae200f6546c0dd0a41320f9f
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54471834"
---
# <a name="how-to-send-scheduled-notifications"></a>Procedures: Geplande meldingen verzenden

Als u een scenario waarin u wilt een melding wordt verzonden op een bepaald moment in de toekomst hebt, maar nog geen een eenvoudige manier om te activeren om uw back-end-code om de melding te verzenden. Standard-laag notification hubs ondersteuning voor een functie waarmee u meldingen plannen van zeven dagen in de toekomst.


## <a name="schedule-your-notifications"></a>Plan uw meldingen
Wanneer u een melding verzendt, gebruiken de [ `ScheduledNotification` klasse](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) in de Notification Hubs SDK zoals wordt weergegeven in het volgende voorbeeld:

```c#
Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));
```

## <a name="cancel-scheduled-notifications"></a>Geplande meldingen annuleren
Bovendien kunt u een eerder geplande melding met behulp van de meldings-id:

```c#
await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);
```

Er zijn geen beperkingen voor het aantal geplande meldingen die u kunt verzenden.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende zelfstudies:

 - [Pushmeldingen verzenden naar alle geregistreerde apparaten](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
 - [Pushmeldingen verzenden naar specifieke apparaten](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md)
 - [Gelokaliseerde pushmeldingen verzenden](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
 - [Pushmeldingen verzenden naar specifieke gebruikers](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) 
 - [Locatiegebaseerde pushmeldingen verzenden](notification-hubs-push-bing-spatial-data-geofencing-notification.md)
