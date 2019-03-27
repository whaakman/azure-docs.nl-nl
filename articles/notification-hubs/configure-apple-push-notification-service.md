---
title: Apple Push Notification Service configureren in Azure Notification Hubs | Microsoft Docs
description: Leer hoe u een Azure notification hub configureren met instellingen voor Apple Push Notification Service (APNS).
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: 9a9db9f05895569b050e56cdeec1ee2ee25af0df
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488274"
---
# <a name="configure-apple-push-notification-service-apns-settings-for-a-notification-hub-in-the-azure-portal"></a>In de Azure-portal Apple Push Notification Service (APNS)-instellingen voor een notification hub configureren
In dit artikel wordt beschreven hoe u Apple Push Notification Service (APNS) om instellingen te configureren voor een Azure notification hub met behulp van de Azure-portal. 

## <a name="prerequisites"></a>Vereisten
Als u al een notification hub hebt gemaakt, moet u een nu maken. Zie voor meer informatie, [een Azure notification hub maken in Azure portal](create-notification-hub-portal.md). 

## <a name="configure-apple-push-notification-service"></a>Apple Push Notification Service configureren

De volgende procedure bevat stappen voor het configureren van instellingen voor Apple Push Notification Service (APNS) voor een notification hub:

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

## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel voor een zelfstudie met stapsgewijze instructies voor het pushen van meldingen op iOS-apparaten: [Pushmeldingen verzenden naar iOS-apparaten met behulp van Notification Hubs en APNS](notification-hubs-ios-apple-push-notification-apns-get-started.md)
