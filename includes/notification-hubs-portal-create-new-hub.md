---
title: bestand opnemen
description: bestand opnemen
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/28/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: c718fa972b9d6cba13a13edd3c7e8df29fa63207
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33835748"
---
1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **Een resource maken** > **Web en mobiel** > **Notification Hub**.
   
      ![Azure Portal - Een Notification Hub maken](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
3. Typ een unieke naam in het vak **Notification Hub**. Selecteer uw **regio**, het **abonnement** en de **resourcegroep** (als u er al een hebt). 
   
      Als u nog geen servicebus-naamruimte hebt, kunt u de standaardnaam gebruiken. Deze wordt gemaakt op basis van de naam van de hub (als de naamruimte beschikbaar is).
    
      Als u al een service bus-naamruimte hebt waarin u de hub wilt maken, gaat u als volgt te werk.

    a. In de gebied **Naamruimte** selecteert u de koppeling **Bestaande selecteren**. 
   
    b. Selecteer **Maken**.
   
      ![Azure Portal - eigenschappen van de Notification Hub instellen](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

4. Selecteer **Meldingen** (pictogram van een klok) en selecteer **Naar de resource gaan**. 

      ![Azure Portal - Meldingen -> Naar de resource gaan](./media/notification-hubs-portal-create-new-hub/notification-go-to-resource.png)    
5. Selecteer **Toegangsbeleid** in de lijst. Let op de twee verbindingsreeksen die voor u beschikbaar zijn. Later moet u er pushmeldingen mee afhandelen.

      >[!IMPORTANT]
      >Gebruik **NIET** de DefaultFullSharedAccessSignature in uw toepassing. Deze mag alleen in uw back-end worden gebruikt.
      >
   
      ![Azure Portal - verbindingsreeksen voor de Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

