---
title: Microsoft Push Notification Service configureren in Azure Notification Hubs | Microsoft Docs
description: Informatie over het configureren van Microsoft Push Notification Service-instellingen voor een Azure notification hub.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: 1c76b44438e6527439d0a370c92f4120424b8da5
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488257"
---
# <a name="configure-microsoft-push-notification-service-mpns-settings-for-a-notification-hub-in-the-azure-portal"></a>Instellingen voor Microsoft Push Notification Service (MPNS) voor een notification hub configureren in Azure portal
Dit artikel leest u hoe Microsoft Push Notification Service (MPNS) om instellingen te configureren voor een Azure notification hub met behulp van de Azure-portal. 

## <a name="prerequisites"></a>Vereisten
Als u al een notification hub hebt gemaakt, moet u een nu maken. Zie voor meer informatie, [een Azure notification hub maken in Azure portal](create-notification-hub-portal.md). 

## <a name="configure-microsoft-push-notification-service-mpns"></a>Microsoft Push Notification Service (MPNS) configureren

De volgende procedure bevat stappen voor het configureren van instellingen voor Microsoft Push Notification Service (MPNS) voor een notification hub: 

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

## <a name="next-steps"></a>Volgende stappen
Zie voor een zelfstudie met stapsgewijze instructies voor het pushen van meldingen naar Windows Phone-apparaten met behulp van Azure Notification Hubs en Microsoft Push Notification Service (MPNS), [Pushmeldingen naar Windows Phone-apps met behulp van melding Hubs](notification-hubs-windows-mobile-push-notifications-mpns.md).

