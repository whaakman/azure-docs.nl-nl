---
title: Het verzenden van meldingen voor geplande | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe gepland meldingen met Azure Notification Hubs.
services: notification-hubs
documentationcenter: .net
keywords: pushmeldingen, push-melding pushmeldingen plannen
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 0f4055a11d22604c0936685a7a2be3d56b259a5b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33776974"
---
# <a name="how-to-send-scheduled-notifications"></a>Procedure: Een geplande meldingen verzenden
## <a name="overview"></a>Overzicht
Als u een scenario waarin u wilt geen melding verzenden door op een bepaald moment in de toekomst hebt, maar hebben geen een eenvoudige manier om te activeren om uw back-end-code om de melding te verzenden. Standard-laag notification hubs ondersteuning voor een functie waarmee u meldingen plannen in zeven dagen in de toekomst.

Wanneer u een melding verzendt, gebruiken de [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) klasse in de Notification Hubs SDK, zoals wordt weergegeven in het volgende voorbeeld:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

U kunt ook een eerder geplande melding met behulp van de notificationId annuleren:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Er zijn geen beperkingen op het aantal geplande meldingen die u kunt verzenden.

