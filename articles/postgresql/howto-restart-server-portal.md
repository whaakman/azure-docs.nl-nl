---
title: Opnieuw opstarten van Azure Database for PostgreSQL-server met behulp van Azure portal
description: Dit artikel wordt beschreven hoe u een Azure Database for PostgreSQL-server met behulp van de Azure portal opnieuw kunt starten.
author: ajlam
ms.author: andrela
ms.service: postgresql
ms.topic: conceptual
ms.date: 3/18/2019
ms.openlocfilehash: bf73120e462b740de5d2245f8a647896ac61f2c8
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58621819"
---
# <a name="restart-azure-database-for-postgresql-server-using-the-azure-portal"></a>Opnieuw opstarten van Azure Database for PostgreSQL-server met behulp van de Azure portal
In dit onderwerp wordt beschreven hoe u een Azure Database for PostgreSQL-server opnieuw kunt starten. Mogelijk moet u de server voor onderhoudsredenen, waardoor een korte onderbreking als de server de bewerking voert opnieuw.

De server opnieuw opstarten wordt geblokkeerd als de service bezet is. De service kan bijvoorbeeld een eerder gevraagde bewerking, zoals schalen vCores verwerkt.
 
De tijd die nodig is om te voltooien van opnieuw opstarten, is afhankelijk van het herstelproces PostgreSQL. Als u wilt verlagen voor het opstarten, wordt u aangeraden dat als u de hoeveelheid activiteit die plaatsvindt op de server voorafgaand aan het opnieuw opstarten kan.

## <a name="prerequisites"></a>Vereisten
Voor deze handleiding, hebt u het volgende nodig:
- Een [Azure Database for PostgreSQL-server](quickstart-create-server-database-portal.md)

## <a name="perform-server-restart"></a>Server opnieuw moet worden opgestart

De PostgreSQL-server start opnieuw op de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com/), selecteer uw Azure Database for PostgreSQL-server.

2. Op de werkbalk van de server **overzicht** pagina, klikt u op **opnieuw**.

   ![Azure Database for PostgreSQL - overzicht - uit-knop](./media/howto-restart-server-portal/2-server.png)

3. Klik op **Ja** om te bevestigen van de server opnieuw wordt opgestart.

   ![Azure Database voor PostgreSQL - opnieuw starten bevestigen](./media/howto-restart-server-portal/3-restart-confirm.png)

4. U ziet dat de status van de server is gewijzigd in 'Opnieuw opstarten'.

   ![Azure Database voor PostgreSQL - status opnieuw starten](./media/howto-restart-server-portal/4-restarting-status.png)

5. Controleer of de server opnieuw opstarten is voltooid.

   ![Azure Database voor PostgreSQL - opnieuw opstarten geslaagd](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [hoe u parameters instelt in Azure Database for PostgreSQL](howto-configure-server-parameters-using-portal.md)