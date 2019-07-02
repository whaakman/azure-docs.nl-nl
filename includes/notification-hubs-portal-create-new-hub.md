---
title: bestand opnemen
description: bestand opnemen
services: notification-hubs
author: jwargo
ms.service: notification-hubs
ms.topic: include
ms.date: 01/17/2019
ms.author: jowargo
ms.custom: include file
ms.openlocfilehash: 5afcc8e4524a0e8353766ba239d5ab9161b29d86
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509081"
---
1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer **alle services** op het menu aan de linkerkant en selecteer vervolgens **Notification Hubs** in de **Mobile** sectie. Selecteer het sterpictogram naast de naam van de service toe te voegen van de service de **Favorieten** sectie in het menu links. Nadat u hebt toegevoegd **Notification Hubs** naar **Favorieten**, selecteert u deze in het menu links.

      ![Azure Portal - Notification Hubs selecteren](./media/notification-hubs-portal-create-new-hub/all-services-select-notification-hubs.png)

1. Op de **Notification Hubs**-pagina selecteert u **Toevoegen** op de werkbalk.

      ![Toevoegen van Notification Hubs - knop op de werkbalk](./media/notification-hubs-portal-create-new-hub/add-toolbar-button.png)

1. Voer op de pagina **Notification Hub** de volgende stappen uit:

    1. Voer een naam in **Notification Hub**.  

    1. Voer een naam in **maken van een nieuwe naamruimte**. Een naamruimte bevat een of meer hubs.

    1. Selecteer een waarde van de **locatie** vervolgkeuzelijst met box. Deze waarde geeft de locatie waarin u wilt maken van de hub.

    1. Selecteer een bestaande resourcegroep in **resourcegroep**, of een naam voor een nieuwe resourcegroep maken.

    1. Selecteer **Maken**.

        ![Azure Portal - eigenschappen van de Notification Hub instellen](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

1. Selecteer **meldingen** (het belpictogram), en selecteer vervolgens **naar de resource gaan**. U kunt ook de lijst vernieuwen op de **Notification Hubs** pagina en selecteer uw hub.

      ![Azure Portal - Meldingen -> Naar de resource gaan](./media/notification-hubs-portal-create-new-hub/go-to-notification-hub.png)

1. Selecteer **Toegangsbeleid** in de lijst. Houd er rekening mee dat de twee verbindingsreeksen voor u beschikbaar zijn. U moet ze later voor het afhandelen van pushmeldingen.

      >[!IMPORTANT]
      >Voer *niet* gebruiken de **DefaultFullSharedAccessSignature** beleid in uw toepassing. Dit is bedoeld om te worden gebruikt in uw back-end alleen.
      >

      ![Azure Portal - verbindingsreeksen voor de Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)
