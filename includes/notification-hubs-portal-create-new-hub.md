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
ms.openlocfilehash: 00b7ffcba876b6abea59cff170331c7413a61d39
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823208"
---
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Alle services** in het menu aan de linkerkant en selecteer **Notification Hubs** in de sectie **Mobiel**. Selecteer de ster (`*`) naast de naam van de service om hem toe te voegen aan de sectie **FAVORIETEN** in het menu links. Nadat **Notification Hubs** is toegevoegd aan **FAVORIETEN**, selecteert u dit in het menu links. 

      ![Azure Portal - Notification Hubs selecteren](./media/notification-hubs-portal-create-new-hub/all-services-select-notification-hubs.png)
3. Op de **Notification Hubs**-pagina selecteert u **Toevoegen** op de werkbalk. 

      ![Toevoegen van Notification Hubs - knop op de werkbalk](./media/notification-hubs-portal-create-new-hub/add-toolbar-button.png)
4. Voer op de pagina **Notification Hub** de volgende stappen uit: 
    1. Geef een **naam** op voor de Notification **Hub**.  
    2. Geef een **naam** op voor de **naamruimte**.
    3. Selecteer een **locatie** waar u de Notification Hub wilt maken. 
    4. Selecteer een bestaande resourcegroep of voer een naam in voor de nieuwe **resourcegroep**.
    5. Selecteer **Maken**. 

        ![Azure Portal - eigenschappen van de Notification Hub instellen](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)
4. Selecteer **Meldingen** (belpictogram), en selecteer **Naar de resource gaan**, of vernieuw de lijst op de **Notification Hubs**-pagina en selecteer de Notification Hub. 

      ![Azure Portal - Meldingen -> Naar de resource gaan](./media/notification-hubs-portal-create-new-hub/go-to-notification-hub.png)
5. Selecteer **Toegangsbeleid** in de lijst. Let op de twee verbindingsreeksen die voor u beschikbaar zijn. Later moet u er pushmeldingen mee afhandelen.

      >[!IMPORTANT]
      >Gebruik **NIET** de DefaultFullSharedAccessSignature in uw toepassing. Deze mag alleen in uw back-end worden gebruikt.
      >

      ![Azure Portal - verbindingsreeksen voor de Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)
