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
ms.openlocfilehash: a5d29e77f6ba10ed3069cb3f5a3c8089f49c237d
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56313769"
---
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Alle services** in het menu aan de linkerkant en selecteer **Notification Hubs** in de sectie **Mobiel**. Selecteer de ster (`*`) naast de naam van de service om hem toe te voegen aan de sectie **FAVORIETEN** in het menu links. Nadat **Notification Hubs** is toegevoegd aan **FAVORIETEN**, selecteert u dit in het menu links. 

      ![Azure Portal - Notification Hubs selecteren](./media/notification-hubs-portal-create-new-hub/all-services-select-notification-hubs.png)
3. Op de **Notification Hubs**-pagina selecteert u **Toevoegen** op de werkbalk. 

      ![Toevoegen van Notification Hubs - knop op de werkbalk](./media/notification-hubs-portal-create-new-hub/add-toolbar-button.png)
4. Voer op de pagina **Notification Hub** de volgende stappen uit: 
    1. Geef een **naam** op voor de Notification **Hub**.  
    2. Geef een **naam** op voor de **naamruimte**. Een naamruimte bevat een of meer hubs. 
    3. Selecteer een **locatie** waar u de Notification Hub wilt maken. 
    4. Selecteer een bestaande resourcegroep of voer een naam in voor de nieuwe **resourcegroep**.
    5. Selecteer **Maken**. 

        ![Azure Portal - eigenschappen van de Notification Hub instellen](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)
4. Selecteer **Meldingen** (pictogram van een klok) en selecteer **Naar de resource gaan**. U kunt ook de lijst op de pagina **Notification Hubs** vernieuwen en uw notification hub selecteren. 

      ![Azure Portal - Meldingen -> Naar de resource gaan](./media/notification-hubs-portal-create-new-hub/go-to-notification-hub.png)
5. Selecteer **Toegangsbeleid** in de lijst. Let op de twee verbindingsreeksen die voor u beschikbaar zijn. Later moet u er pushmeldingen mee afhandelen.

      >[!IMPORTANT]
      >Gebruik **NIET** de DefaultFullSharedAccessSignature in uw toepassing. Deze mag alleen in uw back-end worden gebruikt.
      >

      ![Azure Portal - verbindingsreeksen voor de Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)
