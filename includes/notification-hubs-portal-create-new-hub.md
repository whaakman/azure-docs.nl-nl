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
ms.openlocfilehash: b68fa345d4772134c30ce8b8b559f98113a0496f
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54453094"
---
1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **Een resource maken** > **Mobiel** > **Notification Hub**.

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
