---
title: Het verzenden van meldingen voor geplande | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe gepland meldingen met Azure Notification Hubs.
services: notification-hubs
documentationcenter: .net
keywords: pushmeldingen, push-melding pushmeldingen plannen
author: ysxu
manager: erikre
editor: 
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: efac6e1ecc00359f1622d380333140bc055c83e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-send-scheduled-notifications"></a>Procedure: Een geplande meldingen verzenden
## <a name="overview"></a>Overzicht
Als u een scenario waarin u wilt geen melding verzenden door op een bepaald moment in de toekomst hebt, maar hebben geen een eenvoudige manier om te activeren om uw back-end-code om de melding te verzenden. Standard-laag Notification Hubs ondersteunt een functie waarmee u meldingen plannen op 7 dagen in de toekomst.

Wanneer u een melding verzendt, gebruiken de [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) klasse in de Notification Hubs SDK, zoals wordt weergegeven in het volgende voorbeeld:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

U kunt ook een eerder geplande melding met behulp van de notificationId annuleren:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Er zijn geen beperkingen op het aantal geplande meldingen die u kunt verzenden.

